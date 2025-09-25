# VLAN-dan-Spanning-Tree

Dokumentasi praktikum konfigurasi VLAN dan Spanning Tree Protocol (STP) pada switch Cisco

![Cisco](https://img.shields.io/badge/Cisco-Switch-blue?style=for-the-badge&logo=cisco&logoColor=white)
![VLAN](https://img.shields.io/badge/VLAN-Implementation-green?style=for-the-badge)
![STP](https://img.shields.io/badge/STP-Protocol-orange?style=for-the-badge)

---

## 📝 Deskripsi
Repository ini berisi **konfigurasi VLAN dan Spanning Tree** pada switch Cisco sebagai bagian dari praktikum matakuliah **Jaringan Komputer Terapan 1**. Meliputi pembuatan VLAN, pengaturan trunk, analisis STP, dan troubleshooting jaringan.

---

## ⚙️ Konfigurasi Switch
| Switch | VLAN | Trunk Ports | STP Priority | Status |
|--------|------|-------------|--------------|--------|
| **SW1** | 10 (Sales), 20 (IT) | fa0/1-6 | VLAN 10: 8192 | Non-Root Bridge |
| **SW2** | 10 (Sales), 20 (IT) | fa0/1-6 | Default | Non-Root Bridge |
| **SW-BRIDGE** | 10 (Sales), 20 (IT) | fa0/1-6 | VLAN 1: 4096 | **Root Bridge** |

---

## 🔧 Perintah Konfigurasi
### SW1
```cisco
enable
configure terminal
interface range fa0/1-6
switchport mode trunk
exit
vlan 10
name Sales
vlan 20
name IT
exit
spanning-tree vlan 10 priority 8192
end
```

### SW2
```cisco
enable
configure terminal
vlan 10
name Sales
vlan 20
name IT
interface range fa0/1-6
switchport mode trunk
exit
end
```

### SW-BRIDGE
```cisco
enable
configure terminal
interface range fa0/1-6
switchport mode trunk
exit
vlan 10
name Sales
vlan 20
name IT
exit
spanning-tree vlan 1 priority 4096
end
```

---

## 📊 Output Spanning Tree
### SW1
```
VLAN0001
  Root ID    Priority    32769
             Address     0002.4AE0.DAB3
             Cost        19
             Port        1 (FastEthernet0/1)
  
  Interface        Role Sts Cost      Prio.Nbr Type
  ---------------- ---- --- --------- -------- --------------------------------
  Fa0/1            Root FWD 19        128.1    P2p
  Fa0/2            Altn BLK 19        128.2    P2p
  Fa0/5            Desg FWD 19        128.5    P2p

VLAN0010
  Root ID    Priority    32778
             Address     0060.5C55.2602
             Cost        19
             Port        3 (FastEthernet0/3)
```

### SW2
```
VLAN0001
  Root ID    Priority    32769
             Address     0002.4AE0.DAB3
             Cost        19
             Port        1 (FastEthernet0/1)
  
  Interface        Role Sts Cost      Prio.Nbr Type
  ---------------- ---- --- --------- -------- --------------------------------
  Fa0/1            Root FWD 19        128.1    P2p
  Fa0/3            Desg FWD 19        128.3    P2p

VLAN0010
  Root ID    Priority    8202
             Address     0060.70DB.C5E3
             Cost        19
             Port        3 (FastEthernet0/3)
```

### SW-BRIDGE
```
VLAN0001
  Root ID    Priority    32769
             Address     0002.4AE0.DAB3
             This bridge is the root
  
  Interface        Role Sts Cost      Prio.Nbr Type
  ---------------- ---- --- --------- -------- --------------------------------
  Fa0/1            Desg FWD 19        128.1    P2p
  Fa0/2            Desg FWD 19        128.2    P2p
```

---

## 💡 Saran Pengembangan
1. **Tambahkan Diagram Topologi**: 
   - Gunakan [Mermaid](https://mermaid.js.org/) untuk visualisasi jaringan:
   ```mermaid
   graph LR
   SW1 -- Trunk --- SW-BRIDGE
   SW2 -- Trunk --- SW-BRIDGE
   SW1 -- VLAN10 --- PC1
   SW2 -- VLAN20 --- PC2
   ```
<img width="827" height="421" alt="image" src="https://github.com/user-attachments/assets/ff63539f-c9c6-4c18-b8ba-e4579c9e2dbf" />

2. **Eksperimen Lanjutan**:
   - Tambahkan konfigurasi **Router-on-a-Stick** untuk inter-VLAN routing
   - Uji konektivitas antar VLAN dengan `ping`
   - Simulasi failure link untuk melihat konvergensi STP

3. **Struktur File**:
   ```
   ├── configs/
   │   ├── SW1-config.txt
   │   ├── SW2-config.txt
   │   └── SW-BRIDGE-config.txt
   ├── docs/
   │   └── spanning-tree-analysis.md
   ├── .gitignore
   └── README.md
   ```

4. **Integrasi CI/CD**:
   - Gunakan GitHub Actions untuk validasi sintaks konfigurasi Cisco
   - Tambahkan laporan otomatis dengan Python/Netmiko

---

## 📚 Materi Terkait
- **VLAN (Virtual LAN)**: Segmentasi jaringan logikal
- **Trunking**: Menghubungkan switch dengan multiple VLAN
- **STP (Spanning Tree Protocol)**: Mencegah loop jaringan
- **Port Priority**: Menentukan root bridge dan path jaringan

---

## ✍️ Penulis
**luqmanaru**  
*Mahasiswa Teknik Informatika - Universitas Bina Insani*

---

## 📄 Lisensi
![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)
```
MIT License - Gratis untuk penggunaan edukasi
```

---
> 🚀 **Catatan**: Repository ini dikembangkan untuk tujuan pembelajaran matakuliah Jaringan Komputer Terapan 1. Silakan berkontribusi dengan membuka *issue* atau *pull request*!
```

---

### **Saran untuk Repository:**
1. **Judul Terbaik**: `Konfigurasi-VLAN-Switch-Cisco` (jelas & spesifik)
2. **Deskripsi Terbaik**: Opsi 1 (fokus pada materi kuliah dan teknologi)
3. **Struktur Tambahan**:
   - Tambahkan folder `docs/` untuk analisis mendalam
   - Buat `CONTRIBUTING.md` untuk panduan kontributor
   - Gunakan [GitHub Wiki](https://docs.github.com/en/communities/documenting-your-project-with-wikis) untuk tutorial step-by-step
4. **Visualisasi**: 
   - Tambahkan screenshot output CLI
   - Buat video demo konfigurasi (upload ke YouTube dan embed di README)
5. **Pengujian**:
   - Tambahkan skrip Python untuk validasi konfigurasi
   - Simulasikan dengan GNS3/EVE-NG dan sertakan file proyek

Repository ini sudah siap digunakan dan cukup komprehensif untuk keperluan akademik maupun portofolio teknis! 🎉
