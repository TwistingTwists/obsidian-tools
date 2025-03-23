---
tags:
  -
---

### sed

#flashcards/linux
```bash
sed '/^\s*[a-zA-Z].*:\s.*/p;d'
```
?
 This command filters the lines that match a specific pattern. The pattern looks for lines that start with zero or more spaces, followed by one or more alphabetic characters, followed by a colon and a space. The p;d at the end of the command tells sed to print the matching lines and delete all others.
<!--SR:!2025-03-23,2,230-->

---

### tricks


"Create an ext4 disk image on your exFAT drive"
When do you need this?
?
if exFAT doesn't allow write permissions / etc. ext4 is a virtual disk!
```bash
# Create a large file to serve as a disk image (adjust size as needed)
dd if=/dev/zero of=/media/crucial_ssd/android_dev.img bs=1M count=512000

# Format it as ext4
sudo mkfs.ext4 /media/crucial_ssd/android_dev.img

# Mount it
sudo mkdir -p /mnt/android_dev
sudo mount -o loop /media/crucial_ssd/android_dev.img /mnt/android_dev

# Give yourself ownership
sudo chown $USER:$USER /mnt/android_dev

```
<!--SR:!2025-03-24,3,248-->



----
