# PRAKTIKUM JARKOM MODUL 4 KELOMPOK 04 - 2026

## Anggota Kelompok

| Nama | NRP |
| :--- | :--- |
| Albert Chen | 5027251034 |
| Boma Sahya Aryaguna | 5027251125 |

---

## Laporan

- Template spreadsheet: [Subnetting Modul 4 - K04.xlsx](Subnetting%20Modul%204%20-%20K04.xlsx)
- Project Cisco Packet Tracer: [projects/topo.pkt](projects/topo.pkt)
- Konfigurasi static routing tiap router: [routings/cpt](routings/cpt)
- Konfigurasi router GNS3 (CIDR): [routings/gns3](routings/gns3)

---

## Topologi CPT - VLSM

Berikut topologi yang dibuat untuk Cisco Packet Tracer.

![Topologi CPT](Assets/Topologi%20CPT.png)

Topologi terdiri dari 13 router `2811`, 13 switch, dan 25 host (PC, Laptop, dan Server). Alamat dasar yang digunakan adalah `192.213.0.0/19`. Router **Cyberia** adalah satu-satunya router yang terhubung ke Cloud, dan dari Cyberia seluruh trafik masuk ke **Wired** sebagai hub yang bercabang ke Psyche, Copland, Schumann, Navi, dan rantai Tachibana sampai Shibuya.

---

## Perhitungan VLSM

Setelah dilakukan analisa terhadap topologi, terdapat **25 subnet** yang perlu dibentuk (A1 sampai A25). Jumlah IP tiap subnet dihitung dari jumlah host ditambah interface router pada subnet tersebut.

| Subnet | Kebutuhan IP | Prefix | Network ID | Broadcast | Gateway |
| :--- | ---: | :---: | :--- | :--- | :--- |
| A1 - Cyberia-Switch1 | 126 | /25 | 192.213.14.0 | 192.213.14.127 | 192.213.14.1 |
| A2 - Cyberia-Wired | 2 | /30 | 192.213.16.56 | 192.213.16.59 | - |
| A3 - Switch2 | 4 | /29 | 192.213.16.32 | 192.213.16.39 | 192.213.16.33 |
| A4 - Psyche-Switch3 | 881 | /22 | 192.213.0.0 | 192.213.3.255 | 192.213.0.1 |
| A5 - Copland-Juri | 32 | /26 | 192.213.15.0 | 192.213.15.63 | 192.213.15.1 |
| A6 - Copland-Switch4 | 301 | /23 | 192.213.8.0 | 192.213.9.255 | 192.213.8.1 |
| A7 - Schumann-Akashic | 4 | /29 | 192.213.16.40 | 192.213.16.47 | 192.213.16.41 |
| A8 - Schumann-Accela | 2 | /30 | 192.213.16.68 | 192.213.16.71 | - |
| A9 - Accela-Switch5 | 514 | /22 | 192.213.4.0 | 192.213.7.255 | 192.213.4.1 |
| A10 - Wired-Navi | 2 | /30 | 192.213.16.60 | 192.213.16.63 | - |
| A11 - Navi-Switch6 | 254 | /24 | 192.213.12.0 | 192.213.12.255 | 192.213.12.1 |
| A12 - Navi-Switch7 | 58 | /26 | 192.213.15.64 | 192.213.15.127 | 192.213.15.65 |
| A13 - Navi-Switch8 | 143 | /24 | 192.213.13.0 | 192.213.13.255 | 192.213.13.1 |
| A14 - Wired-Tachibana | 2 | /30 | 192.213.16.64 | 192.213.16.67 | - |
| A15 - Tachibana-Switch9 | 267 | /23 | 192.213.10.0 | 192.213.11.255 | 192.213.10.1 |
| A16 - Tachibana-Deus | 2 | /30 | 192.213.16.72 | 192.213.16.75 | - |
| A17 - Deus-Switch10 | 99 | /25 | 192.213.14.128 | 192.213.14.255 | 192.213.14.129 |
| A18 - Deus-Protocol7 | 2 | /30 | 192.213.16.76 | 192.213.16.79 | - |
| A19 - Protocol7-Switch11 | 33 | /26 | 192.213.15.128 | 192.213.15.191 | 192.213.15.129 |
| A20 - Protocol7-Knights | 2 | /30 | 192.213.16.80 | 192.213.16.83 | - |
| A21 - Knights-Eiri | 3 | /29 | 192.213.16.48 | 192.213.16.55 | 192.213.16.49 |
| A22 - Knights-Phantoma | 2 | /30 | 192.213.16.84 | 192.213.16.87 | - |
| A23 - Phantoma-Switch12 | 15 | /27 | 192.213.16.0 | 192.213.16.31 | 192.213.16.1 |
| A24 - Phantoma-Shibuya | 2 | /30 | 192.213.16.88 | 192.213.16.91 | - |
| A25 - Shibuya-Switch13 | 62 | /26 | 192.213.15.192 | 192.213.15.255 | 192.213.15.193 |

Total penggunaan alamat:

| Keterangan | Nilai |
| :--- | ---: |
| Jumlah subnet | 25 |
| Total IP yang dibutuhkan | 2814 |
| Total IP usable yang dialokasikan | 4138 |
| Prefix root | /19 (8192 alamat, 8190 usable) |

Blok `/20` hanya menyediakan 4094 IP usable, sehingga tidak cukup untuk 4138 IP. Karena itu prefix root terkecil yang memenuhi kebutuhan adalah **/19**. Setiap subnet memakai prefix terkecil yang masih memenuhi jumlah host (tidak ada subnet yang bisa diperkecil satu tingkat lagi), dan tidak ada blok yang saling tumpang tindih.

Dan berikut adalah hasil VLSM Tree yang ada:

![Tree VLSM](Assets/Tree%20VLSM.png)

---

## Static Routing

Pada praktikum ini hanya diharuskan melakukan static routing, sehingga tiap router dikonfigurasi agar memiliki rute menuju subnet yang tidak terhubung langsung. Pola yang dipakai:

- Router tepi (Psyche, Copland, Schumann, Navi, Tachibana, Accela, Deus, Protocol7, Knights, Phantoma, Shibuya) memakai **default route** ke arah Wired.
- **Wired** sebagai hub memakai rute spesifik ke setiap cabang, ditambah default route ke Cyberia.
- **Cyberia** memakai rute agregat ke arah Wired dan tidak memakai default route, karena tidak ada rute balik dari Cloud.
- Rute diagregasi seefisien mungkin, misalnya `192.213.0.0/21` (A4 + A9), `192.213.15.0/24` (A5 + A12 + A19 + A25), `192.213.16.64/27` (A14, A8, A16, A18, A20, A22, A24), dan `192.213.16.80/28` (A20 + A22 + A24).

Ringkasan tabel routing:

| Router | Connected (C) | Static (S) | Default (S*) |
| :--- | ---: | ---: | :--- |
| Cyberia | 2 | 10 | - |
| Wired | 4 | 15 | 192.213.16.57 |
| Psyche | 2 | 0 | 192.213.16.33 |
| Copland | 3 | 0 | 192.213.16.33 |
| Schumann | 3 | 1 | 192.213.16.33 |
| Navi | 4 | 0 | 192.213.16.61 |
| Tachibana | 3 | 6 | 192.213.16.65 |
| Accela | 2 | 0 | 192.213.16.69 |
| Deus | 3 | 4 | 192.213.16.73 |
| Protocol7 | 3 | 5 | 192.213.16.77 |
| Knights | 3 | 3 | 192.213.16.81 |
| Phantoma | 3 | 1 | 192.213.16.85 |
| Shibuya | 2 | 0 | 192.213.16.89 |

Total 45 static route non-default + 12 default route. Konfigurasi lengkap tiap router ada di folder [`routings/cpt`](routings/cpt):

| Router | File |
| :--- | :--- |
| Cyberia | [cyberia.txt](routings/cpt/cyberia.txt) |
| Wired | [wired.txt](routings/cpt/wired.txt) |
| Psyche | [psyche.txt](routings/cpt/psyche.txt) |
| Copland | [copland.txt](routings/cpt/copland.txt) |
| Schumann | [schumann.txt](routings/cpt/schumann.txt) |
| Navi | [navi.txt](routings/cpt/navi.txt) |
| Tachibana | [tachibana.txt](routings/cpt/tachibana.txt) |
| Accela | [accela.txt](routings/cpt/accela.txt) |
| Deus | [deus.txt](routings/cpt/deus.txt) |
| Protocol7 | [protocol7.txt](routings/cpt/protocol7.txt) |
| Knights | [knights.txt](routings/cpt/knights.txt) |
| Phantoma | [phantoma.txt](routings/cpt/phantoma.txt) |
| Shibuya | [shibuya.txt](routings/cpt/shibuya.txt) |

---

## Setting Static IP

Static IP dipasang pada interface router dan pada tiap host (Desktop > IP Configuration), mengikuti range yang sudah dihitung pada spreadsheet. Gateway host selalu memakai IP interface router pada subnet yang sama.

| Host | IP | Netmask | Gateway |
| :--- | :--- | :--- | :--- |
| Karl | 192.213.0.2 | 255.255.252.0 | 192.213.0.1 |
| Lin | 192.213.0.3 | 255.255.252.0 | 192.213.0.1 |
| Omnipresence | 192.213.4.2 | 255.255.252.0 | 192.213.4.1 |
| Denpa | 192.213.4.3 | 255.255.252.0 | 192.213.4.1 |
| Masayuki | 192.213.8.2 | 255.255.254.0 | 192.213.8.1 |
| Collective | 192.213.10.2 | 255.255.254.0 | 192.213.10.1 |
| Ghost | 192.213.10.3 | 255.255.254.0 | 192.213.10.1 |
| Alice | 192.213.12.2 | 255.255.255.0 | 192.213.12.1 |
| Reika | 192.213.12.3 | 255.255.255.0 | 192.213.12.1 |
| Yasuo | 192.213.13.2 | 255.255.255.0 | 192.213.13.1 |
| Miho | 192.213.13.3 | 255.255.255.0 | 192.213.13.1 |
| Kuma | 192.213.13.4 | 255.255.255.0 | 192.213.13.1 |
| JJ | 192.213.14.2 | 255.255.255.128 | 192.213.14.1 |
| MyuMyu | 192.213.14.3 | 255.255.255.128 | 192.213.14.1 |
| Lilith | 192.213.14.130 | 255.255.255.128 | 192.213.14.129 |
| Specter | 192.213.14.131 | 255.255.255.128 | 192.213.14.129 |
| Juri | 192.213.15.2 | 255.255.255.192 | 192.213.15.1 |
| Taro | 192.213.15.66 | 255.255.255.192 | 192.213.15.65 |
| Chisa | 192.213.15.130 | 255.255.255.192 | 192.213.15.129 |
| Hodgeson | 192.213.15.131 | 255.255.255.192 | 192.213.15.129 |
| Lain | 192.213.15.194 | 255.255.255.192 | 192.213.15.193 |
| Mika | 192.213.15.195 | 255.255.255.192 | 192.213.15.193 |
| Semi | 192.213.16.2 | 255.255.255.224 | 192.213.16.1 |
| Akashic | 192.213.16.42 | 255.255.255.248 | 192.213.16.41 |
| Eiri | 192.213.16.50 | 255.255.255.248 | 192.213.16.49 |

Interface Cyberia `fa0/0` diarahkan ke Cloud tanpa IP. Segmen ini tidak memiliki subnet pada mastersheet, karena Cloud hanya jalur keluar (di GNS3 memakai NAT).

---

---

## Topologi GNS3 - CIDR

Topologi yang sama juga dibuat di GNS3 dengan image `ardhptr21/alpinet:latest`. Isinya 13 router, 13 switch, 25 host, dan satu NAT (NAT1) yang terhubung ke Cyberia. Sama seperti di CPT, Cyberia adalah satu-satunya router yang punya jalur keluar, sedangkan router lainnya hanya terhubung ke router tetangga dan switch masing-masing.

![Topologi GNS3](Assets/Topologi%20GNS3.png)

---

## Perhitungan CIDR

Jumlah subnet dan kebutuhan IP tiap subnet sama dengan perhitungan VLSM di atas (25 subnet, total 2814 IP), sehingga prefix tiap subnet juga sama. Bedanya ada di cara menentukan network ID. Pada CIDR subnet tidak langsung dibagi dari satu blok besar, tetapi digabung dulu dari bagian yang paling jauh dari NAT sampai menjadi satu blok, baru hasil penggabungan itu dipecah lagi menjadi alamat tiap subnet.

Langkah yang dikerjakan:

1. Subnet A1 sampai A25 didata beserta prefix-nya.
2. Dimulai dari router paling ujung (Shibuya), subnet milik tiap router digabung dua-dua mulai dari yang terkecil. Prefix hasil gabungan satu tingkat di atas prefix terbesar yang digabung, misalnya A24 /30 + A23 /27 menjadi C1 /26.
3. Hasil gabungan satu router ikut digabung dengan subnet router di atasnya, begitu terus sampai Wired. Terakhir subnet milik Cyberia (A1 dan A2) ikut digabung sehingga terbentuk satu blok **192.213.0.0/19**.
4. Blok /19 dipecah lagi mengikuti urutan penggabungan tadi (bukan dibagi rata dua), sehingga didapat network ID tiap subnet.

Hasil pembagian IP:

| Subnet | Kebutuhan IP | Prefix | Network ID | Broadcast | Gateway |
| :--- | ---: | :---: | :--- | :--- | :--- |
| A1 - Cyberia-Switch1 | 126 | /25 | 192.213.20.0 | 192.213.20.127 | 192.213.20.1 |
| A2 - Cyberia-Wired | 2 | /30 | 192.213.20.144 | 192.213.20.147 | - |
| A3 - Switch2 | 4 | /29 | 192.213.20.128 | 192.213.20.135 | 192.213.20.129 |
| A4 - Psyche-Switch3 | 881 | /22 | 192.213.16.0 | 192.213.19.255 | 192.213.16.1 |
| A5 - Copland-Juri | 32 | /26 | 192.213.26.0 | 192.213.26.63 | 192.213.26.1 |
| A6 - Copland-Switch4 | 301 | /23 | 192.213.24.0 | 192.213.25.255 | 192.213.24.1 |
| A7 - Schumann-Akashic | 4 | /29 | 192.213.4.0 | 192.213.4.7 | 192.213.4.1 |
| A8 - Schumann-Accela | 2 | /30 | 192.213.4.8 | 192.213.4.11 | - |
| A9 - Accela-Switch5 | 514 | /22 | 192.213.0.0 | 192.213.3.255 | 192.213.0.1 |
| A10 - Wired-Navi | 2 | /30 | 192.213.20.148 | 192.213.20.151 | - |
| A11 - Navi-Switch6 | 254 | /24 | 192.213.28.0 | 192.213.28.255 | 192.213.28.1 |
| A12 - Navi-Switch7 | 58 | /26 | 192.213.29.0 | 192.213.29.63 | 192.213.29.1 |
| A13 - Navi-Switch8 | 143 | /24 | 192.213.30.0 | 192.213.30.255 | 192.213.30.1 |
| A14 - Wired-Tachibana | 2 | /30 | 192.213.20.136 | 192.213.20.139 | - |
| A15 - Tachibana-Switch9 | 267 | /23 | 192.213.12.0 | 192.213.13.255 | 192.213.12.1 |
| A16 - Tachibana-Deus | 2 | /30 | 192.213.14.0 | 192.213.14.3 | - |
| A17 - Deus-Switch10 | 99 | /25 | 192.213.10.0 | 192.213.10.127 | 192.213.10.1 |
| A18 - Deus-Protocol7 | 2 | /30 | 192.213.10.128 | 192.213.10.131 | - |
| A19 - Protocol7-Switch11 | 33 | /26 | 192.213.9.0 | 192.213.9.63 | 192.213.9.1 |
| A20 - Protocol7-Knights | 2 | /30 | 192.213.9.64 | 192.213.9.67 | - |
| A21 - Knights-Eiri | 3 | /29 | 192.213.8.128 | 192.213.8.135 | 192.213.8.129 |
| A22 - Knights-Phantoma | 2 | /30 | 192.213.8.136 | 192.213.8.139 | - |
| A23 - Phantoma-Switch12 | 15 | /27 | 192.213.8.64 | 192.213.8.95 | 192.213.8.65 |
| A24 - Phantoma-Shibuya | 2 | /30 | 192.213.8.96 | 192.213.8.99 | - |
| A25 - Shibuya-Switch13 | 62 | /26 | 192.213.8.0 | 192.213.8.63 | 192.213.8.1 |

Total IP yang dibutuhkan 2814 dan semuanya masih muat di dalam blok 192.213.0.0/19 tanpa ada subnet yang tumpang tindih.

Urutan penggabungan yang dilakukan:

| Hasil | Gabungan dari | Router |
| :--- | :--- | :--- |
| C1 /26 | A24 /30 + A23 /27 | Phantoma |
| C2 /25 | A25 /26 + C1 /26 | Phantoma |
| D1 /28 | A22 /30 + A21 /29 | Knights |
| D2 /24 | D1 /28 + C2 /25 | Knights |
| E1 /25 | A20 /30 + A19 /26 | Protocol7 |
| E2 /23 | E1 /25 + D2 /24 | Protocol7 |
| F1 /24 | A18 /30 + A17 /25 | Deus |
| F2 /22 | F1 /24 + E2 /23 | Deus |
| G1 /22 | A5 /26 + A6 /23 | Copland |
| G2 /28 | A8 /30 + A7 /29 | Schumann |
| G3 /21 | G2 /28 + A9 /22 | Schumann |
| G4 /23 | A12 /26 + A11 /24 | Navi |
| G5 /22 | A13 /24 + G4 /23 | Navi |
| G6 /22 | A16 /30 + A15 /23 | Tachibana |
| G7 /21 | F2 /22 + G6 /22 | Tachibana |
| H1 /29 | A2 /30 + A10 /30 | Wired + Cyberia |
| H2 /28 | A14 /30 + A3 /29 | Wired + Cyberia |
| H3 /27 | H1 /29 + H2 /28 | Wired + Cyberia |
| H4 /24 | H3 /27 + A1 /25 | Wired + Cyberia |
| H5 /21 | H4 /24 + A4 /22 | Wired + Cyberia |
| H6 /21 | G1 /22 + G5 /22 | Wired + Cyberia |
| H7 /20 | G3 /21 + G7 /21 | Wired + Cyberia |
| H8 /20 | H5 /21 + H6 /21 | Wired + Cyberia |
| H9 /19 | H7 /20 + H8 /20 | Wired + Cyberia |

Dan berikut adalah hasil CIDR Tree yang ada:

![Tree CIDR](Assets/Tree%20CIDR.png)

---

## Static Routing GNS3

Karena memakai CIDR, subnet di bawah satu router sudah berada dalam satu blok yang rapat, sehingga router di atasnya cukup memakai satu rute untuk satu cabang. Pola yang dipakai:

- Router tepi dan router rantai (Psyche, Copland, Schumann, Navi, Tachibana, Accela, Deus, Protocol7, Knights, Phantoma, Shibuya) memakai **default route** ke router di atasnya.
- **Wired** memakai satu rute untuk tiap cabang (Psyche, Copland, Schumann + Accela, Navi, Tachibana sampai Shibuya) dan default route ke Cyberia.
- **Cyberia** hanya memakai satu rute `192.213.0.0/19` ke arah Wired, karena A1 dan A2 sudah terhubung langsung. Keluar ke internet lewat NAT (`MASQUERADE`).

Ringkasan tabel routing:

| Router | Connected | Static | Default |
| :--- | ---: | ---: | :--- |
| Cyberia | 2 | 1 | - |
| Wired | 4 | 5 | 192.213.20.145 |
| Psyche | 2 | 0 | 192.213.20.129 |
| Copland | 3 | 0 | 192.213.20.129 |
| Schumann | 3 | 1 | 192.213.20.129 |
| Navi | 4 | 0 | 192.213.20.149 |
| Tachibana | 3 | 1 | 192.213.20.137 |
| Accela | 2 | 0 | 192.213.4.9 |
| Deus | 3 | 1 | 192.213.14.1 |
| Protocol7 | 3 | 1 | 192.213.10.129 |
| Knights | 3 | 1 | 192.213.9.65 |
| Phantoma | 3 | 1 | 192.213.8.137 |
| Shibuya | 2 | 0 | 192.213.8.97 |

Total 12 static route non-default + 12 default route. Konfigurasi lengkap tiap router ada di folder [`routings/gns3`](routings/gns3):

| Router | File |
| :--- | :--- |
| Cyberia | [cyberia.txt](routings/gns3/cyberia.txt) |
| Wired | [wired.txt](routings/gns3/wired.txt) |
| Psyche | [psyche.txt](routings/gns3/psyche.txt) |
| Copland | [copland.txt](routings/gns3/copland.txt) |
| Schumann | [schumann.txt](routings/gns3/schumann.txt) |
| Navi | [navi.txt](routings/gns3/navi.txt) |
| Tachibana | [tachibana.txt](routings/gns3/tachibana.txt) |
| Accela | [accela.txt](routings/gns3/accela.txt) |
| Deus | [deus.txt](routings/gns3/deus.txt) |
| Protocol7 | [protocol7.txt](routings/gns3/protocol7.txt) |
| Knights | [knights.txt](routings/gns3/knights.txt) |
| Phantoma | [phantoma.txt](routings/gns3/phantoma.txt) |
| Shibuya | [shibuya.txt](routings/gns3/shibuya.txt) |

---

## Setting Static IP GNS3

Konfigurasi dimasukkan lewat Edit Network Configuration tiap node, sedangkan routing dan `ip_forward` ditulis dengan `up` pada interface terakhir. Gateway host selalu memakai IP interface router pada subnet yang sama.

| Host | IP | Netmask | Gateway |
| :--- | :--- | :--- | :--- |
| Karl | 192.213.16.2 | 255.255.252.0 | 192.213.16.1 |
| Lin | 192.213.16.3 | 255.255.252.0 | 192.213.16.1 |
| Omnipresence | 192.213.0.2 | 255.255.252.0 | 192.213.0.1 |
| Denpa | 192.213.0.3 | 255.255.252.0 | 192.213.0.1 |
| Masayuki | 192.213.24.2 | 255.255.254.0 | 192.213.24.1 |
| Juri | 192.213.26.2 | 255.255.255.192 | 192.213.26.1 |
| Akashic | 192.213.4.2 | 255.255.255.248 | 192.213.4.1 |
| Collective | 192.213.12.2 | 255.255.254.0 | 192.213.12.1 |
| Ghost | 192.213.12.3 | 255.255.254.0 | 192.213.12.1 |
| Alice | 192.213.28.2 | 255.255.255.0 | 192.213.28.1 |
| Reika | 192.213.28.3 | 255.255.255.0 | 192.213.28.1 |
| Taro | 192.213.29.2 | 255.255.255.192 | 192.213.29.1 |
| Yasuo | 192.213.30.2 | 255.255.255.0 | 192.213.30.1 |
| Miho | 192.213.30.3 | 255.255.255.0 | 192.213.30.1 |
| Kuma | 192.213.30.4 | 255.255.255.0 | 192.213.30.1 |
| JJ | 192.213.20.2 | 255.255.255.128 | 192.213.20.1 |
| MyuMyu | 192.213.20.3 | 255.255.255.128 | 192.213.20.1 |
| Lilith | 192.213.10.2 | 255.255.255.128 | 192.213.10.1 |
| Specter | 192.213.10.3 | 255.255.255.128 | 192.213.10.1 |
| Chisa | 192.213.9.2 | 255.255.255.192 | 192.213.9.1 |
| Hodgeson | 192.213.9.3 | 255.255.255.192 | 192.213.9.1 |
| Eiri | 192.213.8.130 | 255.255.255.248 | 192.213.8.129 |
| Semi | 192.213.8.66 | 255.255.255.224 | 192.213.8.65 |
| Lain | 192.213.8.2 | 255.255.255.192 | 192.213.8.1 |
| Mika | 192.213.8.3 | 255.255.255.192 | 192.213.8.1 |

Interface `eth0` Cyberia mengambil IP dari NAT1 lewat DHCP.

---

## Pengujian GNS3

Pengujian dilakukan lewat console node. Lain di Shibuya dipakai sebagai titik uji karena paling jauh dari NAT.

Lain ke Karl (ujung ke ujung topologi):

![Ping Lain ke Karl](Assets/Ping%20Lain%20ke%20Karl.png)

Omnipresence ke Eiri:

![Ping Omnipresence ke Eiri](Assets/Ping%20Omnipresence%20ke%20Eiri.png)

Lain ke internet (8.8.8.8 dan google.com):

![Ping Lain ke Internet](Assets/Ping%20Lain%20ke%20Internet.png)

Lain ke seluruh 62 IP di topologi (semua IP host dan interface router), 62 dari 62 berhasil:

![Ping Semua IP](Assets/Ping%20Semua%20IP.png)

Tabel routing Wired (`ip route`):

![Routing Wired](Assets/Routing%20Wired.png)
