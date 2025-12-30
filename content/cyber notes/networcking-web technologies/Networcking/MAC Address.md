
### **1 What is a MAC Address?**

- **MAC (Media Access Control) address** = 🆔 _unique hardware identifier_ for a network interface card (NIC).
    
- Works at **Layer 2 (Data Link Layer)** of the OSI model.
    
- Used for **local network communication**.
    

---

### **2 Format**

- **48 bits** (6 bytes).
    
- Written as:
    

`XX:XX:XX:XX:XX:XX`

or

`XX-XX-XX-XX-XX-XX`

- Example:
    

`00:1A:2B:3C:4D:5E`

📌 **Hexadecimal format** (0–9, A–F).

---

### **3 Structure**

|🔹 Part|📖 Description|
|---|---|
|First 3 bytes|**OUI (Organizationally Unique Identifier)** = manufacturer ID|
|Last 3 bytes|**NIC-specific ID** = device’s unique number|

Example:

`00:1A:2B : 3C:4D:5E   (OUI)     : (Device ID)`

---

### **4 Types of MAC Addresses**

- **🔹 Unicast** → Identifies a single device.
    
- **🔹 Multicast** → Identifies a group of devices.
    
- **🔹 Broadcast** → Sent to all devices on a local network.:``FF:FF:FF:FF:FF:FF``
    

---

### **5 Purpose**

- 🆔 Device identification on local networks.
    
- 🔄 Frame forwarding in switches.
    
- 🔒 Security (MAC filtering in routers).
    
- 📊 Device tracking (network monitoring).
    

---

### **6 How to Find a MAC Address**

| 🖥 OS         | 🛠 Command / Path                  |
| ------------- | ---------------------------------- |
| Windows       | `ipconfig /all`                    |
| macOS         | `ifconfig`                         |
| Linux         | `ip link show`                     |
| iOS / Android | Settings → Wi-Fi → Network details |

[[index]]
