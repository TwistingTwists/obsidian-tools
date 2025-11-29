---
tags:
  - notch
---

## ✅ Option A – Direct copy from prod to local (no files)

This is usually the cleanest if you already have both connections in DBeaver.

### 1. Make sure both DBs are connected

- In DBeaver’s **Database Navigator**, you should see:
    
    - `Prod` connection
        
    - `Local` connection
        

### 2. Ensure the table exists in local

- In **Prod**, right-click the table → **Generate SQL** → **DDL**.
    
- Run that DDL on your **Local** connection to create the exact same table structure (if it doesn’t already exist).
    

### 3. Use Data Transfer (table → table)

1. In **Prod**, right-click the table you want:
    
    - **Export Data…** (or **Data Transfer…** depending on version).
        
2. In the **Data Transfer Wizard**:
    
    - For **Source**, it will show your prod table.
        
    - For **Target**, choose **Database** (or **Database Table**).
        
3. Choose your **Local connection** and the target table (same name or whatever you want).
    
4. Click **Next**:
    
    - You can add a **WHERE** clause or **LIMIT 100** if you only want ~100 rows.
        
    - Example SQL filter: `LIMIT 100` (Postgres/MySQL) or `FETCH FIRST 100 ROWS ONLY` (Oracle).
        
5. Map columns (DBeaver usually does this automatically).
    
6. Click **Finish** to run.
    

That’s it – the 100 rows from prod are copied directly into your local table.