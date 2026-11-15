# midproject_k02_008_023_071
## Anggota Kelompok

| Nama | NRP |
| :--- | :--- |
| Barra Ahza Fakhrullah | 5027251023 |
| Silfi Rochmatul AuliyH | 5027251008 |
| Muhammad Atallah Mas'udi | 5027251071 |

---

## Dokumentasi & Output

### 1. desain topologi
![Deskripsi Output Gambar](./assets/topologi.png)
Alur desain topologi ini disusun berurutan dari atas ke bawah, mulai dari R-ISP dan server internet 8.8.8.8 di paling atas sebagai pintu masuk internet, turun ke R-PUSAT di Balai Kota sebagai pusat jaringan, lalu bercabang ke lima router wilayah (R-DC, R-BARAT, R-UTARA, R-TIMUR, dan R-SELATAN), hingga turun lagi dari R-UTARA ke R-PELABUHAN di bagian paling bawah, di mana setiap router terhubung ke switch dan satu perangkat perwakilan untuk masing-masing jaringan. Untuk sambungan antar-router digunakan kabel merah berbentuk petir (kabel Serial) bukan kabel LAN lurus biasa, karena kabel petir memang dipakai untuk menghubungkan router jarak jauh antar-gedung/wilayah dan jumlah colokan LAN bawaan pada router terbatas (hanya 3 port), sehingga ditambahkan kartu port Serial (HWIC-2T) agar semua cabang dari R-PUSAT bisa terpasang sekaligus membedakan jalur utama antar-router dengan jalur LAN lokal. Sementara itu, untuk jalur LAN ke kantor, server, CCTV, dan sensor digunakan kabel LAN lurus biasa melalui switch supaya sambungannya stabil, sedangkan khusus empat area WiFi Publik dipasang Access Point karena router dan switch tidak memiliki pemancar WiFi bawaan, sehingga jaringan bisa dipancarkan secara wireless (tanpa kabel) layaknya WiFi umum. Sebagai perangkat penggunanya di jaringan WiFi tersebut dipilih Smartphone karena paling menggambarkan pengguna WiFi publik di lapangan dan sudah memiliki penerima WiFi bawaan, berbeda dengan PC atau server yang memang lebih pas terhubung dengan kabel.


# Dokumentasi Soal 3, 4, dan 5 — Kelompok 2

## 3. Static Routing

Static routing menghubungkan jaringan antarwilayah secara manual. R-PUSAT dan R-UTARA memiliki rute ke LAN di bawahnya, sedangkan default route mengarah ke atas atau keluar.

### Pemeriksaan awal routing R-UTARA

![Pemeriksaan awal routing R-UTARA](<assets/Screenshot 2026-11-15 135154.png>)

R-UTARA memiliki rute static menuju dua LAN Pelabuhan melalui 10.16.243.54. Default route diarahkan ke R-PUSAT melalui 10.16.243.37. Jaringan WiFi dan sensor Utara terhubung langsung ke router.

### Tabel routing R-PUSAT

![Tabel routing R-PUSAT](<assets/Screenshot 2026-11-15 223059.png>)

R-PUSAT memiliki rute static menuju LAN DC, Utara, Pelabuhan, Selatan, Timur, dan Barat. Default route menuju 203.0.113.10 mengarahkan trafik keluar ke R-ISP ketika tidak ada rute yang lebih spesifik. Huruf S menunjukkan static route, sedangkan C menunjukkan jaringan connected.

### Tabel routing R-UTARA

![Tabel routing R-UTARA](<assets/Screenshot 2026-11-15 223140.png>)

Dua jaringan Pelabuhan, 10.16.160.0/19 dan 10.16.242.0/25, diarahkan melalui 10.16.243.54. Default route menuju 10.16.243.37 menunjukkan bahwa rute umum mengarah ke router di atasnya, yaitu R-PUSAT.

### Default route R-PELABUHAN

![Default route R-PELABUHAN](<assets/Screenshot 2026-11-15 223207.png>)

R-PELABUHAN terhubung langsung dengan LAN SCADA dan Kantor Pelabuhan. Untuk tujuan di luar jaringan connected, default route mengarah ke R-UTARA melalui 10.16.243.53, sesuai ketentuan arah default route.

### Ping menuju Kantor Pelabuhan

![Ping menuju Kantor Pelabuhan](<assets/Screenshot 2026-11-15 223236.png>)

Ping ke 10.16.242.2 menerima empat balasan dari empat paket yang dikirim, dengan packet loss 0%. Hasil ini membuktikan perangkat sumber dapat berkomunikasi dengan perangkat Kantor Pelabuhan pada saat pengujian. Jalur sumber diperjelas oleh traceroute berikutnya.

### Traceroute menuju Kantor Pelabuhan

![Traceroute menuju Kantor Pelabuhan](<assets/Screenshot 2026-11-15 223322.png>)

Traceroute ke 10.16.242.2 melewati gateway Balai Kota 10.16.224.1, R-UTARA 10.16.243.38, dan R-PELABUHAN 10.16.243.54 sebelum mencapai tujuan. Urutan ini sesuai jalur Balai Kota → R-PUSAT → R-UTARA → R-PELABUHAN → PC Kantor Pelabuhan.

### Ping menuju Puskesmas/Sekolah Barat

![Ping menuju Puskesmas/Sekolah Barat](<assets/Screenshot 2026-11-15 223359.png>)

Ping ke 10.16.238.2 memperoleh empat balasan dan packet loss 0%. Bukti ini menunjukkan komunikasi menuju perangkat pada LAN Puskesmas/Sekolah Barat berhasil saat diuji.

### Traceroute menuju wilayah Barat

![Traceroute menuju wilayah Barat](<assets/Screenshot 2026-11-15 223423.png>)

Traceroute menuju 10.16.238.2 melewati 10.16.224.1 pada R-PUSAT dan 10.16.243.50 pada R-BARAT, kemudian mencapai perangkat tujuan. Jalur ini lebih pendek daripada jalur Pelabuhan karena R-BARAT terhubung langsung ke R-PUSAT.

Bukti ping dan traceroute di atas menunjukkan dua tujuan LAN yang diuji. Bukti ini tidak sendirian membuktikan seluruh pasangan LAN; hasil pengujian seluruh LAN perlu dicatat terpisah.

## 4. DHCP

Bukti berikut menunjukkan pembagian IP otomatis pada LAN Pegawai Balai Kota dan CCTV Kota. DHCP CCTV dilayani server DC melalui relay pada R-DC.

### Request ulang DHCP Pegawai Balai Kota

![Request ulang DHCP Pegawai Balai Kota](<images/Screenshot 2026-11-15 223544.png>)

Setelah `ipconfig /release` dan `ipconfig /renew`, PC memperoleh IP **10.16.224.2**, subnet mask **255.255.248.0**, gateway **10.16.224.1**, dan DNS **10.16.240.2**. Output `ipconfig /all` menunjukkan DHCP server **10.16.224.1**, yaitu R-PUSAT.

### Binding dan pool DHCP R-PUSAT

![Binding dan pool DHCP R-PUSAT](<images/Screenshot 2026-11-15 223618.png>)

Perintah `show ip dhcp binding` mencatat IP **10.16.224.2** untuk MAC **0000.0CE7.8B11** dengan tipe Automatic, sesuai klien Pegawai. Pool **PEGAWAI_BALAIKOTA** menunjukkan satu alamat disewakan dan satu alamat dikecualikan.

### Konfigurasi DHCP Pegawai Balai Kota

![Konfigurasi DHCP Pegawai Balai Kota](<images/Screenshot 2026-11-15 223738.png>)

Pool **PEGAWAI_BALAIKOTA** melayani jaringan **10.16.224.0/21**, dengan gateway **10.16.224.1** dan DNS **10.16.240.2**. Alamat gateway dikecualikan melalui `ip dhcp excluded-address` agar tidak dibagikan kepada klien.

### Request ulang DHCP CCTV Kota

![Request ulang DHCP CCTV Kota](<images/Screenshot 2026-11-15 223815.png>)

PC CCTV memperoleh IP **10.16.96.3**, subnet mask **255.255.224.0**, gateway **10.16.96.1**, dan DNS **10.16.240.2** setelah renewal. DHCP server yang tercantum adalah **10.16.240.2**, membuktikan klien memperoleh konfigurasi dari server DC.

### DHCP relay pada R-DC

![DHCP relay pada R-DC](<images/Screenshot 2026-11-15 223932.png>)

Interface **GigabitEthernet0/0** pada LAN CCTV menggunakan IP **10.16.96.1/19**. Konfigurasi `ip helper-address 10.16.240.2` meneruskan permintaan DHCP klien CCTV ke server pada LAN DC.

### IP statis server DC

![IP statis server DC](<images/Screenshot 2026-11-15 224038.png>)

Server **SRV-DC-CORE** menggunakan pengaturan Static dengan IP **10.16.240.2/24**, gateway **10.16.240.1**, dan DNS **10.16.240.2**. IP tetap ini menjadi alamat tujuan DHCP relay.

### Pool CCTV pada server DC

![Pool CCTV pada server DC](<images/Screenshot 2026-11-15 224054.png>)

Layanan DHCP pada **SRV-DC-CORE** aktif dengan pool **CCTV_KOTA**, alamat awal **10.16.96.2**, mask **255.255.224.0**, dan kapasitas **6.000 klien**. Gateway yang dibagikan adalah **10.16.96.1** dan DNS **10.16.240.2**. Gambar ini menunjukkan konfigurasi pool, bukan daftar lease server.

## 5. NAT

R-PUSAT menggunakan NAT overload agar klien internal dapat mengakses server **8.8.8.8** melalui satu IP publik **203.0.113.9**.

### Akses 8.8.8.8 dari Balai Kota

![Akses 8.8.8.8 dari Balai Kota](<images/Screenshot 2026-11-15 224119.png>)

PC Pegawai dengan IP privat **10.16.224.2** berhasil melakukan ping ke **8.8.8.8**. Empat paket mendapat balasan dengan **packet loss 0%**, menunjukkan akses ke server internet simulasi dari LAN Balai Kota.

### Akses 8.8.8.8 dari CCTV Kota

![Akses 8.8.8.8 dari CCTV Kota](<images/Screenshot 2026-11-15 224146.png>)

PC CCTV dengan IP privat **10.16.96.3** juga berhasil melakukan ping ke **8.8.8.8** dengan empat balasan dan **packet loss 0%**. Bersama bukti sebelumnya, pengujian menunjukkan akses dari dua LAN berbeda.

### Tabel translasi dan statistik NAT

![Tabel translasi dan statistik NAT](<images/Screenshot 2026-11-15 224228.png>)

Tabel `show ip nat translations` menunjukkan IP privat **10.16.224.2** dan **10.16.96.3** diterjemahkan menjadi satu IP publik **203.0.113.9** untuk mengakses **8.8.8.8**. Identitas ICMP membedakan sesi melalui NAT overload/PAT. Statistik NAT menunjukkan **Serial0/0/0** sebagai interface outside dan interface jaringan internal sebagai inside.

### Rule NAT overload pada R-PUSAT

![Rule NAT overload pada R-PUSAT](<images/Screenshot 2026-11-15 224302.png>)

Rule `ip nat inside source list 20 interface Serial0/0/0 overload` menerjemahkan sumber yang cocok dengan ACL 20 menggunakan IP interface menuju ISP. Opsi `overload` memungkinkan beberapa klien memakai satu IP publik. Default route menuju **203.0.113.10** mengarahkan trafik keluar ke R-ISP.

### Tabel routing R-ISP tanpa rute privat

![Tabel routing R-ISP tanpa rute privat](<images/Screenshot 2026-11-15 224336.png>)

R-ISP hanya menampilkan jaringan terhubung **8.8.8.0/24** dan **203.0.113.8/30**, beserta alamat lokal interfacenya. Tidak terdapat rute menuju jaringan privat internal **10.16.x.x**. Dikombinasikan dengan ping dan tabel translasi, hasil ini mendukung bahwa akses klien berjalan melalui NAT R-PUSAT.
