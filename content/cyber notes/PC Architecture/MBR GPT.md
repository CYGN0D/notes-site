## 🧠 1 What Are MBR and GPT?

Both **MBR (Master Boot Record)** and **GPT (GUID Partition Table)** are **partition table formats** — meaning they define **how partitions and data are organized on a storage device** (HDD/SSD/USB).  
They also play a key role in **how a computer boots**.

| Feature            | MBR                                      | GPT                                     |
| ------------------ | ---------------------------------------- | --------------------------------------- |
| Full Name          | Master Boot Record                       | GUID Partition Table                    |
| Year Introduced    | 1983 (IBM PC-DOS)                        | 2000s (part of UEFI standard)           |
| Partition Limit    | 4 primary partitions (or 3 + 1 extended) | 128 partitions (no extended needed)     |
| Max Disk Size      | 2 TB                                     | Practically unlimited (ZB range)        |
| Boot System        | BIOS                                     | UEFI                                    |
| Boot Code Location | First 512 bytes (sector 0)               | UEFI firmware reads GPT header directly |
| Redundancy         | None                                     | Backup GPT header at end of disk        |
| CRC Protection     | ❌ No                                     | ✅ Yes (integrity-checked headers)       |

---

## ⚙️ 2 How MBR Booting Works (BIOS Boot Mode)

### 🧩 Structure of MBR Disk

| Sector          | Contents                 | Size      |
| --------------- | ------------------------ | --------- |
| **Sector 0**    | Master Boot Record (MBR) | 512 bytes |
| **Sector 1–n**  | Partition Data           | depends   |
| **End of disk** | Nothing special          |           |

**Sector 0 (MBR)** contains:

1. **Bootloader Code** – 446 bytes
    
2. **Partition Table** – 64 bytes (4×16-byte entries)
    
3. **Boot Signature** – 2 bytes (`0x55AA`)
    

---

### 🧬 Boot Flow (BIOS + MBR)

1. **BIOS** runs the POST (Power-On Self Test).
    
2. It looks for a **bootable disk** (with `0x55AA` signature).
    
3. BIOS loads the **first 512 bytes** (MBR) into memory at `0x7C00`.
    
4. The **bootloader code** inside MBR executes.
    
5. That code:
    
    - Reads the **partition table**.
        
    - Finds the **active (bootable)** partition.
        
    - Loads the **Volume Boot Record (VBR)** of that partition.
        
6. The **VBR** loads the **OS bootloader** (like NTLDR, BOOTMGR, GRUB, etc.).
    
7. The **bootloader** finally loads the **operating system kernel**.
    

---

### 🧱 Example (Windows on MBR)

`BIOS → MBR → Bootmgr → winload.exe → Kernel`

---

## 🚀 3 How GPT Booting Works (UEFI Boot Mode)

### 🧩 Structure of GPT Disk

|Area|Description|
|---|---|
|**Protective MBR**|Dummy MBR at sector 0 (for legacy tools)|
|**Primary GPT Header**|Sector 1 – identifies as GPT disk|
|**Partition Entries**|Follow the header (usually 128 entries × 128 bytes)|
|**Partition Data**|Actual OS/data partitions|
|**Backup GPT Header**|Stored at the **end of disk**|

---

### 🧬 Boot Flow (UEFI + GPT)

1. **UEFI firmware** runs POST.
    
2. It scans storage for an **EFI System Partition (ESP)**.
    
3. The **ESP** is a small FAT32 partition (100–300 MB) with boot files.
    
4. UEFI directly loads the **EFI executable** (e.g., `bootx64.efi`).
    
5. The EFI executable (bootloader) initializes and loads the **OS kernel**.
    

---

### 🧱 Example (Windows on GPT)

`UEFI → EFI System Partition → \EFI\Microsoft\Boot\bootmgfw.efi → winload.efi → Kernel`

For Linux:

`UEFI → EFI System Partition → \EFI\ubuntu\grubx64.efi → grub.cfg → Kernel`

---

## ⚖️ 4 Comparison: MBR vs GPT in Boot

|Aspect|BIOS + MBR|UEFI + GPT|
|---|---|---|
|Firmware|Legacy BIOS|UEFI|
|Boot File|MBR boot code|`.efi` file on ESP|
|Boot Disk ID|Active partition flag|GUID Partition ID|
|Security|None|Secure Boot support|
|Boot Partition Type|Must be “Active”|ESP (FAT32)|
|Recovery|Corruption = boot failure|Redundant GPT + CRC|
|Max OS Partitions|4 primary|128+|
|Speed|Slower (sequential load)|Faster (direct .efi load)|

---

## 🧩 5 Protective MBR (in GPT Disks)

Even GPT disks contain a **"protective MBR"** at sector 0.

- It prevents **legacy disk utilities** (that don’t recognize GPT) from thinking the disk is empty.
    
- It defines **one fake partition** spanning the entire disk.