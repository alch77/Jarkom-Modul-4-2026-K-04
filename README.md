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