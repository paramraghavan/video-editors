✅ Command to Create a 32GB FAT32 Partition from 64 GB drive and Leave the Rest Unused
```bash
diskutil partitionDisk /dev/disk2 MBR FAT32 MUSIC 32G free unused 0B
#Breakdown:
#/dev/disk2 — replace with your USB disk ID (check using diskutil list)

# MBR — Master Boot Record partition scheme (best for car USB compatibility)
#FAT32 MUSIC 32G — creates a FAT32 partition named MUSIC with size 32GB
#free unused 0B — a placeholder for the rest of the space, unallocated (or unused)
```

To delete a **partition** using `diskutil` in macOS Terminal, follow these steps **carefully**:

---

### 🔍 1. **List all disks and partitions**

Run this to see your disks and partitions:

```bash
diskutil list
```

Look for the correct **partition identifier**, like `/dev/disk2s2`.

---

### 🧨 2. **Delete a specific partition**

Use this command:

```bash
diskutil eraseVolume free none /dev/disk2s2
```

* Replace `/dev/disk2s2` with the **exact identifier** of the partition you want to delete.
* `free none` makes the space unallocated (i.e., removes the volume).

---

### 💣 3. (Optional) Delete all partitions on the disk

If you want to erase the entire disk and remove all partitions:

```bash
diskutil eraseDisk free NONE /dev/disk2
```

This will make the entire disk **empty and unformatted**.

---

### ⚠️ Warnings

* Triple-check the disk/partition identifier — mistakes can wipe the wrong drive.
* If the `Partition` tab is greyed out in Disk Utility, the above method will still work from Terminal.

Would you like help identifying which disk is your USB drive or internal one?
