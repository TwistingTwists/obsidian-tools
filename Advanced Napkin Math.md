

#flashcards/system_design/napkin_math


Cloud Computing costs are:
CPU:  --  / core / month
Memory:   --  / GB / month
SSD:--    / GB / month
Disk: --  / GB / month
Cloud-storage (S3): --  / GB / month
Network: --  / GB / month
?
CPU: $10 / core / month
Memory: $1 / GB / month
SSD: $0.1 / GB / month
Disk: $0.01 / GB / month
Cloud-storage (S3): $0.02 / GB / month
Network: $0.01 / GB / month
<!--SR:!2025-03-23,3,250-->

---


Sequential RAM read (8 KiB)  ---
Sequential RAM write (8KiB) ---
Sequential SSD read (8 KiB)  ---
 Sequential SSD write, -fsync (8KiB) ---
 Sequential SSD write, +fsync (8KiB)  --
 Random SSD Read (8 KiB)
 Random HDD Read (8 KiB)
 TCP echo server (localhost)
 ?
 Sequential RAM read (8 KiB)  ---1 ns
Sequential RAM write (8KiB) --- 5 ns
Sequential SSD read (8 KiB)  --- 1μs
 Sequential SSD write, -fsync (8KiB) --- 10 μs
 Sequential SSD write, +fsync (8KiB)  -- 1 ms
 Random SSD Read (8 KiB) --  100 μs
 Random HDD Read (8 KiB)  --  10 ms
 TCP echo server (localhost) -- 15  μs
<!--SR:!2025-03-21,1,230-->

---
