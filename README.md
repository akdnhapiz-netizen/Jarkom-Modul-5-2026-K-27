# Jarkom-Modul-5-2026-K-27

## Member

| Nama | NRP |
| :--- | :--- |
| Muhammad Nadhif Pasya Ikhsan | 5027251084 |
| Akhdan Hafiz Anugrah | 5027251094 |

---
## Soal 11 — Casper Harus Jadi Hantu

### Tujuan Pengerjaan
Mengamankan node `Magi-Casper` (`10.77.4.58/29`) menggunakan `iptables` agar menolak seluruh permintaan `ping` masuk (`ICMP Echo Request` / type 8) secara diam-diam (`DROP`) dari perangkat mana pun—termasuk perangkat yang berada dalam satu switch (`Switch-Terminal1`, subnet `10.77.4.56/29`)—sambil tetap mempertahankan kemampuan `Magi-Casper` untuk mem-ping perangkat lain serta menjaga layanan TCP port `5000` tetap berjalan normal.

### Node & Alur Komunikasi
| Node | Peran | IP Address | Terhubung ke |
| :--- | :--- | :--- | :--- |
| `Magi-Casper` | Server target firewall & listener TCP `5000` | `10.77.4.58/29` | `Switch-Terminal1` (`e1`) |
| `Magi-Balthasar` | Client penguji ping satu switch & TCP `5000` | `10.77.4.59/29` | `Switch-Terminal1` (`e2`) |
| `Lilith` | Client penguji ping beda subnet & TCP `5000` | `10.77.4.74/29` | `Switch-Dogma` (`e2`) |

```text
Magi-Balthasar (10.77.4.59/29) <---> Switch-Terminal1 <---> Magi-Casper (10.77.4.58/29)
Lilith (10.77.4.74/29) <---> Toji <---> Fuyutsuki <---> Switch-Terminal1 <---> Magi-Casper
```
Karena `Magi-Balthasar` dan `Magi-Casper` berada dalam satu domain Layer 2 (`Switch-Terminal1`), paket antar keduanya tidak melewati router `Fuyutsuki`, sehingga rule `INPUT` wajib dipasang langsung di host `Magi-Casper`.

### Script Konfigurasi (`/root/soal11_casper.sh`)
```sh
#!/bin/sh
set -e

iptables -P INPUT ACCEPT
iptables -P FORWARD ACCEPT
iptables -P OUTPUT ACCEPT
iptables -F
iptables -X

# Izinkan loopback lokal
iptables -A INPUT -i lo -j ACCEPT

# Tolak diam-diam semua ICMP Echo Request (ping) masuk ke Magi-Casper
iptables -A INPUT -p icmp --icmp-type echo-request -j DROP

# Pastikan listener TCP 5000 aktif
if ! (ss -tln 2>/dev/null | grep -q ':5000 ' || netstat -tln 2>/dev/null | grep -q ':5000 '); then
    nohup sh -c 'while true; do nc -l -p 5000 >> /root/casper_5000.log 2>&1; done' >/dev/null 2>&1 &
    sleep 1
fi

echo "[OK] Hardening Soal 11 aktif di Magi-Casper (10.77.4.58)."
iptables -S
```

### Hasil Pengujian (*Test Case*) & Dokumentasi

| No | Penguji | Target & Uji | Ekspektasi | Hasil Aktual |
| :-: | :--- | :--- | :--- | :--- |
| 1 | `Lilith` (`10.77.4.74`) | `ping -c 4 10.77.4.58` & `nc` TCP `5000` | Ping gagal (`100% loss`), TCP `5000` berhasil (`exit 0`) | Ping `100% loss` (`exit 1`), TCP `5000` `exit 0` |
| 2 | `Magi-Balthasar` (`10.77.4.59`) | `ping -c 4 10.77.4.58` & `nc` TCP `5000` | Ping gagal (`100% loss`), TCP `5000` berhasil (`exit 0`) | Ping `100% loss` (`exit 1`), TCP `5000` `exit 0` |
| 3 | `Magi-Casper` (`10.77.4.58`) | Ping keluar ke `Magi-Balthasar`, cek listener `5000`, & counter `INPUT` | Ping keluar berhasil (`0% loss`), listener aktif, rule `DROP` naik | Ping ke `10.77.4.59` `0% loss` (`4/4`), listener `0.0.0.0:5000` aktif, counter rule `#2 DROP icmptype 8` = `4 pkts` |

1. **Uji dari `Lilith` (Beda Subnet):**
  ![alt text](<asset/foto 1.png>)

2. **Uji dari `Magi-Balthasar` (Satu Switch):**
   ![alt text](<asset/foto 2.png>)

3. **Uji balik, status listener TCP `5000`, dan counter rule `INPUT` di `Magi-Casper`:**
   ![alt text](<asset/foto 3.png>)

---

## Soal 12 — Teknisi yang Kelewat Semangat

### Tujuan Pengerjaan
Menganalisis penyebab kerusakan firewall akibat script sabotase `break_fw.sh` pada `Magi-Casper` (`10.77.4.58/29`), kemudian memulihkan konektivitas internet (`ping 8.8.8.8` dan `apt-get update`) serta akses TCP port `5000` bagi `Unit-00` (`10.77.4.68/29`) dengan tetap mempertahankan **policy default `INPUT DROP`** dan hardening Soal 11.

### Analisis Kerusakan (`break_fw.sh`)
Setelah `break_fw.sh` dijalankan, `iptables -S` pada `Magi-Casper` menunjukkan tiga masalah utama:
1. Policy default `-P INPUT DROP` aktif **tanpa** aturan `conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT`, sehingga paket balasan dari internet (`ping 8.8.8.8` maupun `apt-get`) ikut di-`DROP`.
2. Adanya aturan `-A OUTPUT -p udp --dport 53 -j DROP` yang memblokir seluruh query DNS keluar sehingga `apt-get update` gagal me-resolve `deb.debian.org`.
3. Tidak adanya aturan `-A INPUT -p tcp --dport 5000 -j ACCEPT`, sehingga koneksi masuk ke layanan TCP `5000` dari `Unit-00` ikut terblokir oleh policy `INPUT DROP`.

### Script Perbaikan (`/root/soal12_casper.sh`)
```sh
#!/bin/sh
set -e

iptables -F
iptables -X

iptables -P INPUT DROP
iptables -P FORWARD DROP
iptables -P OUTPUT ACCEPT

iptables -A INPUT -i lo -j ACCEPT
iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
iptables -A INPUT -p icmp --icmp-type echo-request -j DROP
iptables -A INPUT -p tcp --dport 5000 -j ACCEPT

if ! (ss -tln 2>/dev/null | grep -q ':5000 ' || netstat -tln 2>/dev/null | grep -q ':5000 '); then
    nohup sh -c 'while true; do nc -l -p 5000 >> /root/casper_5000.log 2>&1; done' >/dev/null 2>&1 &
    sleep 1
fi

echo "[OK] Perbaikan firewall Soal 12 pada Magi-Casper (10.77.4.58) selesai."
iptables -S
```

### Hasil Pengujian (*Test Case*) & Dokumentasi

| Fase | Node | Pengujian | Hasil Aktual |
| :--- | :--- | :--- | :--- |
| Sebelum Fix | `Magi-Casper` | `iptables -S`, `ping -c 3 8.8.8.8`, `apt-get update` | `ping` `100% loss`; `apt-get update` gagal (`Temporary failure resolving 'deb.debian.org'`) |
| Sebelum Fix | `Unit-00` | `nc -w 3 10.77.4.58 5000` | Gagal (`Exit code nc 5000: 1`) |
| Sesudah Fix | `Magi-Casper` | Cek policy, `ping -c 4 8.8.8.8`, `apt-get update`, listener `5000` | Policy `INPUT DROP`; `ping 8.8.8.8` `0% loss`; `apt-get update` sukses (`Hit:1..3`); listener `5000` aktif |
| Sesudah Fix | `Unit-00` | `nc` TCP `5000`, `ping 10.77.4.58`, `nc` TCP `9999` | TCP `5000` sukses (`exit 0`); `ping` tetap gagal (`exit 1`); TCP `9999` gagal/DROP (`exit 1`) |

1. **Bukti Kondisi Rusak (Sebelum Fix) di `Magi-Casper` & `Unit-00`:**
   ![alt text](<asset/foto 4.png>)
   ![alt text](<asset/foto 5.png>)

2. **Verifikasi Setelah Fix di `Magi-Casper`:**
   ![alt text](<asset/foto 6.png>)

3. **Verifikasi Setelah Fix dari `Unit-00`:**
   ![alt text](<asset/foto 7.png>)

---

## Soal 13 — Brankas Balthasar

### Tujuan Pengerjaan
Membatasi akses menuju layanan TCP port `5000` dan `5001` pada server `Magi-Balthasar` (`10.77.4.59/29`) agar hanya `Magi-Casper` dan seluruh `Pilot` (`Unit-01`, `Unit-02`, `Unit-00`, `Sachiel`) yang diizinkan terhubung, sementara seluruh perangkat lain—termasuk tetangga satu switch (`Magi-Melchior`) dan lintas subnet (`Lilith`)—ditolak seketika menggunakan `REJECT --reject-with tcp-reset` tanpa *timeout*, serta lalu lintas internet (`ping 8.8.8.8`) tetap berjalan normal.

### Kebijakan Whitelist & Penolakan
| Node | IP Address | Kategori | Kebijakan Port `5000` & `5001` |
| :--- | :--- | :--- | :--- |
| `Magi-Casper` | `10.77.4.58/29` | MAGI | `ACCEPT` |
| `Unit-01` | `10.77.4.66/29` | Pilot | `ACCEPT` |
| `Unit-02` | `10.77.4.67/29` | Pilot | `ACCEPT` |
| `Unit-00` | `10.77.4.68/29` | Pilot | `ACCEPT` |
| `Sachiel` | `10.77.3.130/26` | Pilot | `ACCEPT` |
| `Magi-Melchior` | `10.77.4.60/29` | MAGI (satu switch) | `REJECT --reject-with tcp-reset` |
| `Lilith` | `10.77.4.74/29` | Staf Geofront | `REJECT --reject-with tcp-reset` |

### Script Konfigurasi (`/root/soal13_balthasar.sh`)
```sh
#!/bin/sh
set -e

iptables -P INPUT ACCEPT
iptables -P FORWARD ACCEPT
iptables -P OUTPUT ACCEPT

iptables -N BALTHASAR_VAULT 2>/dev/null || iptables -F BALTHASAR_VAULT

if ! iptables -C INPUT -p tcp --dport 5000 -j BALTHASAR_VAULT 2>/dev/null; then
    iptables -I INPUT 1 -p tcp --dport 5000 -j BALTHASAR_VAULT
fi
if ! iptables -C INPUT -p tcp --dport 5001 -j BALTHASAR_VAULT 2>/dev/null; then
    iptables -I INPUT 2 -p tcp --dport 5001 -j BALTHASAR_VAULT
fi

# Izinkan hanya Magi-Casper & seluruh Pilot (Unit-01, Unit-02, Unit-00, Sachiel)
for IP_ALLOW in 10.77.4.58 10.77.4.66 10.77.4.67 10.77.4.68 10.77.3.130; do
    iptables -A BALTHASAR_VAULT -s "$IP_ALLOW" -j ACCEPT
done

# Tolak seketika (TCP RST) semua sumber lain tanpa menggantung
iptables -A BALTHASAR_VAULT -p tcp -j REJECT --reject-with tcp-reset

if ! (ss -tln 2>/dev/null | grep -q ':5000 ' || netstat -tln 2>/dev/null | grep -q ':5000 '); then
    nohup sh -c 'while true; do nc -l -p 5000 >> /root/balthasar_5000.log 2>&1; done' >/dev/null 2>&1 &
fi
if ! (ss -tln 2>/dev/null | grep -q ':5001 ' || netstat -tln 2>/dev/null | grep -q ':5001 '); then
    nohup sh -c 'while true; do nc -l -p 5001 >> /root/balthasar_5001.log 2>&1; done' >/dev/null 2>&1 &
fi
sleep 1

echo "[OK] Konfigurasi Brankas Balthasar (Soal 13) aktif di 10.77.4.59."
iptables -L BALTHASAR_VAULT -n -v --line-numbers
```

### Hasil Pengujian (*Test Case*) & Dokumentasi

1. **Aktivasi Custom Chain `BALTHASAR_VAULT` pada `Magi-Balthasar`:**
   ![alt text](<asset/foto 8.png>)

2. **Uji Node Diizinkan (`Unit-01` & `Unit-02`) — Port `5000` & `5001` Sukses (`Exit code: 0`):**
   ![alt text](<asset/foto 9.png>)
   ![alt text](<asset/foto 10.png>)

3. **Uji Node Ditolak Seketika (`Lilith` & `Magi-Melchior`) — `TCP RST` (`Exit code: 1`, Durasi `0–1 detik`):**
   ![alt text](<asset/foto 11.png>)
   ![alt text](<asset/foto 12.png>)

4. **Verifikasi Akhir di `Magi-Balthasar` (`ping 8.8.8.8` `0% loss`, log pesan masuk `Unit-01` & `Unit-02`, counter `20 pkts` ACCEPT & `9 pkts` REJECT):**
   ![alt text](<asset/foto 13.png>)

---

## Soal 14 — Jam Kunjungan Sang Pinguin

### Tujuan Pengerjaan
Membatasi akses layanan `ping` (`ICMP Echo Request`) dan `TCP port 80` menuju server `Pen-Pen` (`10.77.3.194/27`) berdasarkan kategori perangkat pengirim dan jadwal kunjungan menggunakan modul `-m time` pada `iptables` dalam zona waktu **UTC** ($\text{UTC} = \text{WIB} - 7\text{ jam}$), tanpa mengubah jam sistem container.

### Aturan Jadwal & Kategori
| Kategori | Node | Jadwal Resmi (WIB) | Jadwal UTC (`-m time`) |
| :--- | :--- | :--- | :--- |
| **Pilot** | `Unit-01`, `Unit-02`, `Unit-00`, `Sachiel` | Senin–Jumat `08.00–16.00 WIB` | `01:00–09:00 UTC` (`Mon,Tue,Wed,Thu,Fri`) |
| **Staf Geofront** | `Lilith`, `Adam` | Setiap hari `17.00–23.00 WIB` | `10:00–16:00 UTC` |
| **Dummy System** | `Dummy-Plug`, `Entry-Plug`, `Spy` | Tidak pernah diizinkan | Selalu ditolak (`REJECT tcp-reset` / `DROP`) |
| **Lainnya** | Selain tiga kategori di atas (`Magi-Casper`, dll.) | Bebas tanpa batasan waktu | `ACCEPT` (default chain) |

### Script Konfigurasi (`/root/soal14_penpen.sh`)
```sh
#!/bin/sh
set -e

PILOT_START="01:00"
PILOT_STOP="09:00"
PILOT_DAYS="Mon,Tue,Wed,Thu,Fri"

STAF_START="10:00"
STAF_STOP="16:00"

iptables -P INPUT ACCEPT
iptables -P FORWARD ACCEPT
iptables -P OUTPUT ACCEPT

iptables -N PENPEN_SCHEDULE 2>/dev/null || iptables -F PENPEN_SCHEDULE

if ! iptables -C INPUT -p icmp --icmp-type echo-request -j PENPEN_SCHEDULE 2>/dev/null; then
    iptables -I INPUT 1 -p icmp --icmp-type echo-request -j PENPEN_SCHEDULE
fi
if ! iptables -C INPUT -p tcp --dport 80 -j PENPEN_SCHEDULE 2>/dev/null; then
    iptables -I INPUT 2 -p tcp --dport 80 -j PENPEN_SCHEDULE
fi

# Kategori Pilot
for IP_PILOT in 10.77.4.66 10.77.4.67 10.77.4.68 10.77.3.130; do
    iptables -A PENPEN_SCHEDULE -s "$IP_PILOT" -m time --timestart "$PILOT_START" --timestop "$PILOT_STOP" --weekdays "$PILOT_DAYS" -j ACCEPT
    iptables -A PENPEN_SCHEDULE -s "$IP_PILOT" -p tcp -j REJECT --reject-with tcp-reset
    iptables -A PENPEN_SCHEDULE -s "$IP_PILOT" -j DROP
done

# Kategori Staf Geofront
for IP_STAF in 10.77.4.74 10.77.4.75; do
    iptables -A PENPEN_SCHEDULE -s "$IP_STAF" -m time --timestart "$STAF_START" --timestop "$STAF_STOP" -j ACCEPT
    iptables -A PENPEN_SCHEDULE -s "$IP_STAF" -p tcp -j REJECT --reject-with tcp-reset
    iptables -A PENPEN_SCHEDULE -s "$IP_STAF" -j DROP
done

# Kategori Terduga Dummy System (Selalu ditolak)
for IP_DUMMY in 10.77.4.51 10.77.4.50 10.77.4.102; do
    iptables -A PENPEN_SCHEDULE -s "$IP_DUMMY" -p tcp -j REJECT --reject-with tcp-reset
    iptables -A PENPEN_SCHEDULE -s "$IP_DUMMY" -j DROP
done

if ! (ss -tln 2>/dev/null | grep -q ':80 ' || netstat -tln 2>/dev/null | grep -q ':80 '); then
    nohup sh -c 'while true; do printf "Welcome to Pen-Pen\n" | nc -l -p 80 >/dev/null 2>&1; done' >/dev/null 2>&1 &
    sleep 1
fi

echo "[OK] Jadwal Pen-Pen aktif pada $(date -u)."
iptables -L PENPEN_SCHEDULE -n -v --line-numbers
```

### Hasil Pengujian (*Test Case*) & Dokumentasi

1. **Aktivasi Jadwal Resmi di `Pen-Pen` (`Wed Nov 25 06:51:51 UTC 2026`):**
   ![alt text](<asset/foto 14.png>)

2. **Uji Kategori Lainnya (`Magi-Casper`) — Bebas Akses (`ping` & TCP `80` `Welcome to Pen-Pen` sukses):**
   ![alt text](<asset/foto 15.png>)

3. **Uji Kategori Pilot (`Unit-01` pada `06:57:30 UTC`, masuk rentang `01:00–09:00 UTC`) — Sukses:**
   ![alt text](<asset/foto 16.png>)

4. **Uji Kategori Staf Geofront (`Lilith` pada `07:01:09 UTC` dengan simulasi jadwal cocok) — Sukses:**
   ![alt text](<asset/foto 17.png>)

5. **Uji Kategori Terduga Dummy System (`Spy` pada `07:01:38 UTC`) — Selalu Ditolak (`ping` `100% loss`, TCP `80` ditolak):**
   ![alt text](<asset/foto 18.png>)

6. **Pengembalian Variabel Jadwal ke Kondisi Resmi (`01:00–09:00` & `10:00–16:00 UTC`) di `Pen-Pen`:**
   ![alt text](<asset/foto 19.png>)

---

## Soal 15 — Banjir Ping di Pusat Komando

### Tujuan Pengerjaan
Membatasi node `Tokyo3-Core` (`10.77.4.106/30`) menggunakan modul `-m limit` pada `iptables` agar hanya menjawab maksimal **5 permintaan ping per detik** (`5/sec`), membuang paket ping berlebih secara diam-diam (`DROP`), tanpa mengganggu ping normal (jeda $\ge 3$ detik) maupun ping keluar yang diinisiasi oleh `Tokyo3-Core` sendiri.

### Script Konfigurasi (`/root/soal15_tokyo3.sh`)
```sh
#!/bin/sh
set -e

iptables -P INPUT ACCEPT
iptables -P FORWARD ACCEPT
iptables -P OUTPUT ACCEPT

iptables -N PING_LIMIT 2>/dev/null || iptables -F PING_LIMIT

if ! iptables -C INPUT -i lo -j ACCEPT 2>/dev/null; then
    iptables -I INPUT 1 -i lo -j ACCEPT
fi
if ! iptables -C INPUT -p icmp --icmp-type echo-request -j PING_LIMIT 2>/dev/null; then
    iptables -A INPUT -p icmp --icmp-type echo-request -j PING_LIMIT
fi

iptables -A PING_LIMIT -p icmp --icmp-type echo-request -m limit --limit 5/sec --limit-burst 5 -j ACCEPT
iptables -A PING_LIMIT -p icmp --icmp-type echo-request -j DROP

echo "[OK] Pembatasan ping 5/detik (Soal 15) aktif di Tokyo3-Core."
iptables -L PING_LIMIT -n -v --line-numbers
```

### Hasil Pengujian (*Test Case*) & Dokumentasi

| Penguji | Skenario Uji | Ekspektasi | Hasil Aktual |
| :--- | :--- | :--- | :--- |
| `Lilith` (`10.77.4.74`) | Banjir ping (`ping -i 0.1 -c 50 10.77.4.106`) | Sebagian paket hilang (*dropped*) | `50 transmitted, 3 received, 94% packet loss` |
| `Lilith` (`10.77.4.74`) | Ping normal (`ping -i 3 -c 4 10.77.4.106`) | `0% packet loss` | `4 transmitted, 4 received, 0% packet loss` |
| `Tokyo3-Core` (`10.77.4.106`) | Counter `PING_LIMIT` & ping keluar ke `Lilith` (`-i 0.1 -c 15`) dan `8.8.8.8` | Rule `DROP` naik `> 0 pkts`, ping keluar `0% loss` | Rule #1 `ACCEPT`: `70 pkts`; Rule #2 `DROP`: `37 pkts`; ping ke `Lilith` (`15/15`) & `8.8.8.8` (`3/3`) `0% loss` |

1. **Aktivasi Chain `PING_LIMIT` di `Tokyo3-Core`:**
   ![alt text](<asset/foto 20.png>)

2. **Uji Banjir Ping & Ping Normal dari `Lilith`:**
   ![alt text](<asset/foto 21.png>)

3. **Verifikasi Counter `PING_LIMIT` & Ping Keluar pada `Tokyo3-Core`:**
   ![alt text](<asset/foto 22.png>)

---

## Soal 16 — Pencuri yang Mencoba Setiap Pintu

### Tujuan Pengerjaan
Mencatat (`LOG`) dengan prefix **`SCAN-NERV:`** dan menolak seketika (`REJECT --reject-with tcp-reset`) setiap upaya koneksi TCP dari `Dummy-Plug` (`10.77.4.51/29`) menuju port sensitif **`22`, `23`, dan `3306`** di server `Tokyo3-Core` (`10.77.4.106/30`), tanpa mengganggu akses port layanan normal (`80` dan `8080`) serta tanpa mengganggu klien lainnya (`Lilith`).

### Script Konfigurasi (`/root/soal16_tokyo3.sh`)
```sh
#!/bin/sh
set -e

iptables -N NERV_PORTSCAN 2>/dev/null || iptables -F NERV_PORTSCAN

if ! iptables -C INPUT -s 10.77.4.51 -p tcp -m multiport --dports 22,23,3306 -j NERV_PORTSCAN 2>/dev/null; then
    iptables -I INPUT 1 -s 10.77.4.51 -p tcp -m multiport --dports 22,23,3306 -j NERV_PORTSCAN
fi

iptables -A NERV_PORTSCAN -j LOG --log-prefix "SCAN-NERV: " --log-level 4
iptables -A NERV_PORTSCAN -p tcp -j REJECT --reject-with tcp-reset

if ! (ss -tln 2>/dev/null | grep -q ':80 ' || netstat -tln 2>/dev/null | grep -q ':80 '); then
    nohup sh -c 'while true; do printf "Welcome to Tokyo3-Core\n" | nc -l -p 80 >/dev/null 2>&1; done' >/dev/null 2>&1 &
fi
if ! (ss -tln 2>/dev/null | grep -q ':8080 ' || netstat -tln 2>/dev/null | grep -q ':8080 '); then
    nohup sh -c 'while true; do printf "syifa imutz\n" | nc -l -p 8080 >/dev/null 2>&1; done' >/dev/null 2>&1 &
fi
sleep 1

echo "[OK] Deteksi port scan Soal 16 aktif di Tokyo3-Core."
iptables -L NERV_PORTSCAN -n -v --line-numbers
```

### Hasil Pengujian (*Test Case*) & Dokumentasi

1. **Aktivasi Chain `NERV_PORTSCAN` di `Tokyo3-Core`:**
   ![alt text](<asset/foto 23.png>)

2. **Uji Pemindaian Port (`22`, `23`, `3306`) & Layanan Normal (`80`, `8080`) dari `Dummy-Plug`:**
   - Port `22`, `23`, `3306` ditolak seketika (`Exit code: 1 | Durasi: 0 detik`).
   - Port `80` (`Welcome to Tokyo3-Core`) & `8080` (`syifa imutz`) berhasil (`Exit code: 0`).
   ![alt text](<asset/foto 24.png>)

3. **Uji Klien Normal `Lilith` ke Port `80` & `8080` (`Exit code: 0`):**
   ![alt text](<asset/foto 25.png>)

4. **Verifikasi Counter `LOG` (`SCAN-NERV: `) & `REJECT` di `Tokyo3-Core` (`3 pkts / 180 bytes`):**
   ![alt text](<asset/foto 26.png>)

---

## Soal 17 — Sihir Hitam di Jalur Paket

### Tujuan Pengerjaan
Mengalihkan secara diam-diam (*transparent DNAT*) setiap koneksi **TCP** dari `Magi-Casper` (`10.77.4.58/29`) yang ditujukan ke `Zeruel` (`10.77.4.82/29`) pada router `Fuyutsuki` agar diterima oleh `Pen-Pen` (`10.77.3.194/27`), tanpa membuat `Magi-Casper` menyadari pengalihan tersebut (*reverse-DNAT* otomatis oleh `conntrack`) dan tanpa mengalihkan lalu lintas `ping` (`ICMP`).

### Alur Komunikasi
```text
[TCP 9000] Magi-Casper (10.77.4.58) --> dst: 10.77.4.82:9000 --> Fuyutsuki [PREROUTING DNAT -> 10.77.3.194] --> Pen-Pen (10.77.3.194:9000)
[ICMP]     Magi-Casper (10.77.4.58) --> dst: 10.77.4.82      --> Fuyutsuki (tidak di-DNAT) --> Shigeru --> Zeruel (10.77.4.82)
```

### Script Konfigurasi (`/root/soal17_fuyutsuki.sh`)
```sh
#!/bin/sh
set -e

sysctl -w net.ipv4.ip_forward=1 >/dev/null 2>&1 || echo 1 > /proc/sys/net/ipv4/ip_forward

if ! iptables -t nat -C PREROUTING -s 10.77.4.58 -d 10.77.4.82 -p tcp -j DNAT --to-destination 10.77.3.194 2>/dev/null; then
    iptables -t nat -A PREROUTING -s 10.77.4.58 -d 10.77.4.82 -p tcp -j DNAT --to-destination 10.77.3.194
fi

echo "[OK] Pengalihan TCP (DNAT) Soal 17 aktif di Fuyutsuki."
iptables -t nat -L PREROUTING -n -v --line-numbers
```

### Hasil Pengujian (*Test Case*) & Dokumentasi

1. **Aktivasi Rule `PREROUTING DNAT` di Router `Fuyutsuki` & Listener TCP `9000` di `Zeruel` dan `Pen-Pen`:**
   ![alt text](<asset/foto 27.png>)
   ![alt text](<asset/foto 28.png>)
   ![alt text](<asset/foto 29.png>)

2. **Uji Pengiriman Pesan TCP `9000` & `ping` dari `Magi-Casper` ke `10.77.4.82`:**
   - `nc` ke `10.77.4.82:9000` berhasil (`Exit code nc 9000: 0`).
   - `ping -c 4 10.77.4.82` tetap menuju `Zeruel` (`0% packet loss`).
   ![alt text](<asset/foto 30.png>)

3. **Verifikasi Log `/root/listen_9000.log` di `Pen-Pen` (Tercatat) vs `Zeruel` (Kosong) & Counter `PREROUTING` di `Fuyutsuki`:**
   ![alt text](<asset/foto 31.png>)
   ![alt text](<asset/foto 32.png>)
   ![alt text](<asset/foto 33.png>)

---

## Soal 18 — Gerbang Monolith dan Radar

### Tujuan Pengerjaan
Melindungi Zona Sensitif yang mencakup `Switch-Monolith` (`10.77.4.80/29`) dan `Switch-Radar` (`10.77.4.88/29`)—diagregasi presisi menjadi **`10.77.4.80/28`**—dengan memasang firewall stateful pada router `Shigeru` berdasarkan interface datang (`-i eth0` dari arah `Gendou`), sehingga seluruh koneksi baru dari luar zona diblokir tanpa mendaftarkan subnet luar satu per satu, perangkat dalam zona bebas menginisiasi koneksi keluar, dan hanya `ping` dari subnet `Switch-Terminal` (`10.77.4.56/29`) yang diizinkan masuk.

### Script Konfigurasi (`/root/soal18_shigeru.sh`)
```sh
#!/bin/sh
set -e

sysctl -w net.ipv4.ip_forward=1 >/dev/null 2>&1 || echo 1 > /proc/sys/net/ipv4/ip_forward

IF_GENDO=$(ip -4 -o addr show | awk '$4 ~ /^10\.77\.4\.130\// {print $2; exit}')
IF_GENDO="${IF_GENDO:-eth0}"

iptables -N SHIGERU_GATE 2>/dev/null || iptables -F SHIGERU_GATE

if ! iptables -C FORWARD -i "$IF_GENDO" -d 10.77.4.80/28 -j SHIGERU_GATE 2>/dev/null; then
    iptables -I FORWARD 1 -i "$IF_GENDO" -d 10.77.4.80/28 -j SHIGERU_GATE
fi

iptables -A SHIGERU_GATE -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
iptables -A SHIGERU_GATE -s 10.77.4.56/29 -p icmp --icmp-type echo-request -j ACCEPT
iptables -A SHIGERU_GATE -j DROP

echo "[OK] Gerbang Monolith & Radar (Soal 18) aktif di Shigeru."
iptables -L SHIGERU_GATE -n -v --line-numbers
```

### Hasil Pengujian (*Test Case*) & Dokumentasi

1. **Aktivasi Chain `SHIGERU_GATE` di Router `Shigeru`:**
   ![alt text](<asset/foto 34.png>)

2. **Uji Pengecualian Ping Subnet Terminal (`Magi-Casper` $\rightarrow$ `Zeruel` & `Radar-Array`) — Berhasil (`0% loss`):**
   *(Catatan: pada baris uji TCP `9000` di `Magi-Casper` terlihat `exit 0` karena rule DNAT Soal 17 pada `Fuyutsuki` mengalihkan TCP `10.77.4.58 -> 10.77.4.82` ke `Pen-Pen` sebelum mencapai `Shigeru`.)*
   ![alt text](<asset/foto 35.png>)

3. **Uji Pemblokiran dari Luar Zona (`Lilith` $\rightarrow$ `Zeruel` & `Radar-Array`) — Ditolak (`100% loss`, `exit 1`):**
   ![alt text](<asset/foto 36.png>)

4. **Uji Koneksi Keluar dari Dalam Zona (`Zeruel` $\rightarrow$ `Lilith` & `Seele 8.8.8.8`) — Berhasil (`0% loss`):**
   ![alt text](<asset/foto 37.png>)

5. **Verifikasi Counter `SHIGERU_GATE` di Router `Shigeru` (`23 pkts` `RELATED,ESTABLISHED`, `12 pkts` `Terminal ICMP`, `7 pkts` `DROP`):**
   ![alt text](<asset/foto 38.png>)

---

## Soal 19 — Hukuman untuk Sang Pengkhianat

### Tujuan Pengerjaan
Mengisolasi total node `Armisael` (`10.77.4.83/29`) dari seluruh lalu lintas jaringan masuk dan keluar—termasuk gateway `Shigeru` (`10.77.4.81/29`) dan tetangga satu switch `Zeruel` (`10.77.4.82/29`)—**tanpa memasang aturan firewall di dalam `Armisael` sendiri**, sambil memastikan `Zeruel` tetap dapat berkomunikasi normal ke `Lilith` (`10.77.4.74/29`) dan `Seele` (`8.8.8.8`).

### Script Konfigurasi

1. **Pada Router `Shigeru` (`/root/soal19_shigeru.sh`):**
   ```sh
   #!/bin/sh
   set -e

   IP_ARMISAEL="10.77.4.83"

   iptables -N ISOLATE_ARMISAEL 2>/dev/null || iptables -F ISOLATE_ARMISAEL

   if ! iptables -C INPUT -s "$IP_ARMISAEL" -j ISOLATE_ARMISAEL 2>/dev/null; then
       iptables -I INPUT 1 -s "$IP_ARMISAEL" -j ISOLATE_ARMISAEL
   fi
   if ! iptables -C OUTPUT -d "$IP_ARMISAEL" -j ISOLATE_ARMISAEL 2>/dev/null; then
       iptables -I OUTPUT 1 -d "$IP_ARMISAEL" -j ISOLATE_ARMISAEL
   fi
   if ! iptables -C FORWARD -s "$IP_ARMISAEL" -j ISOLATE_ARMISAEL 2>/dev/null; then
       iptables -I FORWARD 1 -s "$IP_ARMISAEL" -j ISOLATE_ARMISAEL
   fi
   if ! iptables -C FORWARD -d "$IP_ARMISAEL" -j ISOLATE_ARMISAEL 2>/dev/null; then
       iptables -I FORWARD 2 -d "$IP_ARMISAEL" -j ISOLATE_ARMISAEL
   fi

   iptables -A ISOLATE_ARMISAEL -j DROP

   echo "[OK] Isolasi Armisael (10.77.4.83) aktif di Shigeru."
   ```

2. **Pada Tetangga Satu Switch `Zeruel` (`/root/soal19_zeruel.sh`):**
   ```sh
   #!/bin/sh
   set -e

   IP_ARMISAEL="10.77.4.83"

   iptables -N BLOCK_ARMISAEL 2>/dev/null || iptables -F BLOCK_ARMISAEL

   if ! iptables -C INPUT -s "$IP_ARMISAEL" -j BLOCK_ARMISAEL 2>/dev/null; then
       iptables -I INPUT 1 -s "$IP_ARMISAEL" -j BLOCK_ARMISAEL
   fi
   if ! iptables -C OUTPUT -d "$IP_ARMISAEL" -j BLOCK_ARMISAEL 2>/dev/null; then
       iptables -I OUTPUT 1 -d "$IP_ARMISAEL" -j BLOCK_ARMISAEL
   fi

   iptables -A BLOCK_ARMISAEL -j DROP

   echo "[OK] Blokir tetangga satu switch Armisael aktif di Zeruel."
   ```

### Hasil Pengujian (*Test Case*) & Dokumentasi

1. **Aktivasi Isolasi di `Shigeru` & `Zeruel`, serta Listener TCP `9000` di `Armisael`, `Zeruel`, dan `Lilith`:**
   ![alt text](<asset/foto 39.png>)
   ![alt text](<asset/foto 40.png>)
   ![alt text](<asset/foto 41.png>)
   ![alt text](<asset/foto 42.png>)
   ![alt text](<asset/foto 43.png>)

2. **Uji Isolasi Total dari `Armisael` — Seluruh Ping & NC ke `Shigeru`, `Zeruel`, `Lilith`, dan `Seele` Gagal (`exit: 1`):**
   ![alt text](<asset/foto 44.png>)

3. **Uji Fungsi Normal `Zeruel` — Koneksi ke `Seele` & `Lilith` Sukses (`exit: 0`), ke `Armisael` Gagal (`exit: 1`):**
   ![alt text](<asset/foto 45.png>)

---

## Soal 20 — Uji Kebakaran Geofront

### Tujuan Pengerjaan
Memastikan ketahanan dan idempotensi konfigurasi jaringan pada tiga node terpilih (**`Fuyutsuki`**, **`Magi-Casper`**, dan **`Tokyo3-Core`**) dengan menyimpan aturan firewall (`iptables-save`) serta script rute statis (`ip route replace`), menghapus seluruh aturan dan rute tambahan, lalu memulihkannya sebanyak **dua kali berturut-turut** menggunakan `iptables-restore` dan script routing tanpa menimbulkan duplikasi aturan maupun rute.

### Ringkasan Hasil Uji Kebakaran (3 Node)

| Node | Kondisi Kosong (`filter` / `nat`) | Pemulihan Ke-1 (`filter` / `nat`) | Pemulihan Ke-2 (`filter` / `nat`) | Uji Fungsi Kembali |
| :--- | :---: | :---: | :---: | :--- |
| **`Fuyutsuki`** (Router) | `3` / `4` | `3` / `5` | `3` / `5` | `ping` ke `Pen-Pen` (`10.77.3.194`) & `Lilith` (`10.77.4.74`) `0% loss` |
| **`Magi-Casper`** (Server) | `3` / `4` | `7` / `4` | `7` / `4` | `ping 8.8.8.8` `0% loss` & `nc` DNAT `10.77.4.82:9000` (`NC DNAT OK`) |
| **`Tokyo3-Core`** (Server) | `3` / `4` | `12` / `4` | `12` / `4` | `ping 8.8.8.8` `0% loss` |

### Dokumentasi Pengujian (*Test Case*)

1. **Uji Kebakaran & Pemulihan Ganda pada Router `Fuyutsuki`:**
   ![alt text](<asset/foto 46.png>)

2. **Uji Kebakaran & Pemulihan Ganda pada `Magi-Casper`:**
   ![alt text](<asset/foto 47.png>)

3. **Uji Kebakaran & Pemulihan Ganda pada `Tokyo3-Core`:**
   ![alt text](<asset/foto 48.png>)

---


