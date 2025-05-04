✅ Command to Create a 32GB FAT32 Partition from 64 GB drive and Leave the Rest Unused
```bash
diskutil partitionDisk /dev/disk2 MBR FAT32 MUSIC 32G free unused 0B
#Breakdown:
#/dev/disk2 — replace with your USB disk ID (check using diskutil list)

# MBR — Master Boot Record partition scheme (best for car USB compatibility)
#FAT32 MUSIC 32G — creates a FAT32 partition named MUSIC with size 32GB
#free unused 0B — a placeholder for the rest of the space, unallocated (or unused)
```
