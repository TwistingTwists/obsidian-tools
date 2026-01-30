# Semantic Search Implementation Plan
**Date:** 2026-01-30

## Overview
Add semantic search capabilities to obsidian-tools by:
1. Generating embeddings for note content during parquet ingest (fmq)
2. Extending query.py to support semantic search alongside tag-based filtering
3. Enabling hybrid queries combining both tag filters and semantic similarity

---

## Phase 1: FMQ (Rust) Changes

### 1.1 Add embedding dependency
**File:** `fmq/Cargo.toml`

Add embedding crate (evaluate options):
- `ort` (ONNX Runtime) - lightweight, local inference
- `huggingface-hub` - for remote embeddings
- `ndarray` - for vector operations

```toml
ort = { version = "2", features = ["download-binaries"] }  # or huggingface-hub
ndarray = "0.15"
```

### 1.2 Extend CLI args
**File:** `fmq/src/main.rs`

Add new CLI options:
```rust
#[derive(Parser)]
struct Cli {
    #[arg(long, default_value = ".")]
    vault: PathBuf,
    #[arg(long, default_value = "vault.parquet")]
    output: PathBuf,
    #[arg(long)]
    with_embeddings: bool,
    #[arg(long, default_value = "all-MiniLM-L6-v2")]
    embedding_model: String,
}
```

### 1.3 Add embedding computation
**File:** `fmq/src/main.rs`

Create new function:
```rust
fn compute_embeddings(notes: &[ParsedNote], model_name: &str) -> Result<Vec<Vec<f32>>> {
    // Load embedding model (ONNX or HF)
    // For each note, combine: body + frontmatter fields
    // Return vector of embeddings
}
```

Embedding input: concatenate `_body` + key frontmatter fields (tags, title, etc.)

### 1.4 Extend parquet schema
Add new columns to `schema`:
- `_embedding` (LargeList of Float32) - vectorized representation
- `_embedding_model` (Utf8) - model name for reproducibility
- `_embedding_dim` (Int32) - dimension count

Update field construction:
```rust
fields.push(Field::new("_embedding", DataType::List(...), false));
fields.push(Field::new("_embedding_model", DataType::Utf8, false));
```

Build embedding columns using `ListBuilder<PrimitiveBuilder<Float32Type>>`.

### 1.5 Update ingest flow
```
collect_md_files() 
  → parse_note() 
  → [NEW] compute_embeddings() 
  → build schema with embedding columns 
  → write parquet
```

---

## Phase 2: Query.py (Python) Changes

### 2.1 Add embedding model dependency
**File:** `local-exploration-db/requirements.txt` (create if missing)

```
polars>=1.0
numpy
scikit-learn  # for cosine_similarity
sentence-transformers  # for embeddings (or ort for ONNX)
```

### 2.2 Extend query parser
**File:** `local-exploration-db/query.py`

Current syntax:
```bash
'tags:turbopuffer'
"tags:clippings AND tags:turbopuffer"
```

New syntax:
```bash
'semantic:DFS traversal patterns'
'tags:graphs semantic:dynamic programming tips'  # hybrid
"tags:clippings AND semantic:best practices"
```

Implement parser to distinguish:
- `tags:X` → exact tag match
- `semantic:X` → similarity search

### 2.3 Add semantic search function
```python
def semantic_search(query_text: str, df, embedding_col="_embedding", top_k=10):
    # Load same embedding model as fmq used
    model = SentenceTransformer("all-MiniLM-L6-v2")
    
    # Embed query
    query_embedding = model.encode(query_text, convert_to_tensor=True)
    
    # Compute similarities (cosine or L2 distance)
    embeddings = np.array(df[embedding_col].to_list())
    similarities = cosine_similarity([query_embedding], embeddings)[0]
    
    # Return top-k indices
    top_indices = np.argsort(similarities)[-top_k:][::-1]
    return df[top_indices], similarities[top_indices]
```

### 2.4 Update main query flow
```python
def execute_query(query_string: str, df):
    tag_filters = parse_tag_clauses(query_string)
    semantic_clauses = parse_semantic_clauses(query_string)
    
    # Apply tag filters first (reduce search space)
    result = df.filter(tag_filters)
    
    # Apply semantic search
    if semantic_clauses:
        for semantic_query in semantic_clauses:
            semantic_results, scores = semantic_search(semantic_query, result)
            result = semantic_results  # intersection
    
    return result
```

### 2.5 Output formatting
Add relevance scores to output:
```
Path | Tags | Semantic Score | Snippet
```

---

## Phase 3: Testing & Integration

### 3.1 Test cases
- Query with tags only (existing functionality)
- Query with semantics only
- Hybrid query (tags + semantic)
- Edge cases: empty results, typos in semantic queries

### 3.2 Update CLI/README
```bash
# New ingest with embeddings
./fmq/target/release/fmq --vault ~/my-vault --output vault.parquet --with-embeddings

# Semantic query
cd local-exploration-db
uv run python query.py "semantic:graph algorithms"

# Hybrid
uv run python query.py "tags:learning semantic:DFS patterns"
```

### 3.3 Performance benchmarks
- Embedding generation time (scales with vault size)
- Query latency for semantic search (top-k vs brute force)
- Parquet file size increase

---

## Implementation Priority
1. **Must:** FMQ embedding column + computation
2. **Must:** query.py semantic search function
3. **Nice:** Hybrid query parser
4. **Nice:** Performance optimization (FAISS index for large vaults)

## Open Questions
- Which embedding model? (all-MiniLM-L6-v2 vs GPT-3.5 vs local ONNX)
- Local vs remote embeddings? (latency vs cost)
- Approximate nearest neighbor indexing for >10k notes?
