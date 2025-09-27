---
tags:
  - rust
---

try this repo - [GitHub - sunshowers/cancelling-async-rust: Notes on my RustConf 2025 talk: Cancelling async Rust](https://github.com/sunshowers/cancelling-async-rust#)


read this - [400 - Dealing with cancel safety in async Rust / RFD / Oxide](https://rfd.shared.oxide.computer/rfd/400)

[Using Rust async for Query Execution and Cancelling Long-Running Queries - Apache DataFusion Blog](https://datafusion.apache.org/blog/2025/06/30/cancellation/)  

  ```
  type AdReviewPreviewProps = {

adIdeaId: string;

idea?: any;

generatedImages?: GeneratedAdImage[];

// Updated to use proper GeneratedAdImage type instead of any[]

loading: boolean;

isGeneratingImage: boolean;

variant: any;

refetchImageGenHistory: any;

onImageNavigation?: (imageId: string) => void;

isAdmin: boolean;

optimisticActiveImageId?: string | null;

};
  ```