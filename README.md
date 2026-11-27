# Jarkom-Modul-5-2026-K-27

## Member

| Nama | NRP |
| :--- | :--- |
| Muhammad Nadhif Pasya Ikhsan | 5027251084 |
| Akhdan Hafiz Anugrah | 5027251094 |

---

## Soal 1

Pada tahap awal, dilakukan perencanaan pembagian alamat IP dari blok `10.77.0.0/16` menggunakan metode **VLSM (Variable Length Subnet Mask)** untuk **17 LAN** dan **16 link antar-router** (total 33 subnet). Alokasi diurutkan dari kebutuhan host terbesar (termasuk +1 IP untuk gateway). Untuk ukuran prefix yang sama, diterapkan aturan *tie-break*: **LAN terlebih dahulu daripada link**, kemudian mengikuti urutan Tabel Host.

### Tabel VLSM 17 Subnet LAN

| No | Switch (LAN) | Router Gateway | Host (+GW) | Network | Prefix | Gateway | Broadcast | IP Terpakai | Sisa IP |
| :-: | :--- | :--- | :-: | :--- | :-: | :--- | :--- | :-: | :-: |
| 1 | Switch-Quarters | Rei | 500 (+1) | `10.77.0.0` | `/23` | `10.77.0.1` | `10.77.1.255` | 501 | 9 |
| 2 | Switch-Comm | Hikari | 200 (+1) | `10.77.2.0` | `/24` | `10.77.2.1` | `10.77.2.255` | 201 | 53 |
| 3 | Switch-Hangar | Yui | 100 (+1) | `10.77.3.0` | `/25` | `10.77.3.1` | `10.77.3.127` | 101 | 25 |
| 4 | Switch-Bunker | Kensuke | 55 (+1) | `10.77.3.128` | `/26` | `10.77.3.129` | `10.77.3.191` | 56 | 6 |
| 5 | Switch-Medic | Maya | 26 (+1) | `10.77.3.192` | `/27` | `10.77.3.193` | `10.77.3.223` | 27 | 3 |
| 6 | Switch-Relay | Kaworu | 25 (+1) | `10.77.3.224` | `/27` | `10.77.3.225` | `10.77.3.255` | 26 | 4 |
| 7 | Switch-Garden | Shinji | 14 (+1) | `10.77.4.0` | `/27` | `10.77.4.1` | `10.77.4.31` | 15 | 15 |
| 8 | Switch-Dock | Mari | 12 (+1) | `10.77.4.32` | `/28` | `10.77.4.33` | `10.77.4.47` | 13 | 1 |
| 9 | Switch-Lab | Ritsuko | 5 (+1) | `10.77.4.48` | `/29` | `10.77.4.49` | `10.77.4.55` | 6 | 0 |
| 10 | Switch-Terminal | Fuyutsuki | 3 (+1) | `10.77.4.56` | `/29` | `10.77.4.57` | `10.77.4.63` | 4 | 2 |
| 11 | Switch-Cage | Misato | 3 (+1) | `10.77.4.64` | `/29` | `10.77.4.65` | `10.77.4.71` | 4 | 2 |
| 12 | Switch-Dogma | Toji | 2 (+1) | `10.77.4.72` | `/29` | `10.77.4.73` | `10.77.4.79` | 3 | 3 |
| 13 | Switch-Monolith | Shigeru | 2 (+1) | `10.77.4.80` | `/29` | `10.77.4.81` | `10.77.4.87` | 3 | 3 |
| 14 | Switch-Radar | Makoto | 4 (+1) | `10.77.4.88` | `/29` | `10.77.4.89` | `10.77.4.95` | 5 | 1 |
| 15 | Switch-Archive | Asuka | 1 (+1) | `10.77.4.96` | `/30` | `10.77.4.97` | `10.77.4.99` | 2 | 0 |
| 16 | Switch-Intel | Kaji | 1 (+1) | `10.77.4.100` | `/30` | `10.77.4.101` | `10.77.4.103` | 2 | 0 |
| 17 | Switch-Cmd | Gendou | 1 (+1) | `10.77.4.104` | `/30` | `10.77.4.105` | `10.77.4.107` | 2 | 0 |

### Tabel VLSM 16 Subnet Link Antar-Router (`/30`)

| No | Link Router (Induk – Anak) | Network | Prefix | IP Induk / IP Anak | Broadcast | IP Terpakai | Sisa IP |
| :-: | :--- | :--- | :-: | :--- | :--- | :-: | :-: |
| 18 | Gendou – Yui | `10.77.4.108` | `/30` | `10.77.4.109` / `10.77.4.110` | `10.77.4.111` | 2 | 0 |
| 19 | Gendou – Hikari | `10.77.4.112` | `/30` | `10.77.4.113` / `10.77.4.114` | `10.77.4.115` | 2 | 0 |
| 20 | Gendou – Asuka | `10.77.4.116` | `/30` | `10.77.4.117` / `10.77.4.118` | `10.77.4.119` | 2 | 0 |
| 21 | Gendou – Kaji | `10.77.4.120` | `/30` | `10.77.4.121` / `10.77.4.122` | `10.77.4.123` | 2 | 0 |
| 22 | Gendou – Misato | `10.77.4.124` | `/30` | `10.77.4.125` / `10.77.4.126` | `10.77.4.127` | 2 | 0 |
| 23 | Gendou – Shigeru | `10.77.4.128` | `/30` | `10.77.4.129` / `10.77.4.130` | `10.77.4.131` | 2 | 0 |
| 24 | Gendou – Fuyutsuki | `10.77.4.132` | `/30` | `10.77.4.133` / `10.77.4.134` | `10.77.4.135` | 2 | 0 |
| 25 | Yui – Mari | `10.77.4.136` | `/30` | `10.77.4.137` / `10.77.4.138` | `10.77.4.139` | 2 | 0 |
| 26 | Yui – Rei | `10.77.4.140` | `/30` | `10.77.4.141` / `10.77.4.142` | `10.77.4.143` | 2 | 0 |
| 27 | Yui – Shinji | `10.77.4.144` | `/30` | `10.77.4.145` / `10.77.4.146` | `10.77.4.147` | 2 | 0 |
| 28 | Hikari – Kaworu | `10.77.4.148` | `/30` | `10.77.4.149` / `10.77.4.150` | `10.77.4.151` | 2 | 0 |
| 29 | Asuka – Kensuke | `10.77.4.152` | `/30` | `10.77.4.153` / `10.77.4.154` | `10.77.4.155` | 2 | 0 |
| 30 | Asuka – Ritsuko | `10.77.4.156` | `/30` | `10.77.4.157` / `10.77.4.158` | `10.77.4.159` | 2 | 0 |
| 31 | Fuyutsuki – Maya | `10.77.4.160` | `/30` | `10.77.4.161` / `10.77.4.162` | `10.77.4.163` | 2 | 0 |
| 32 | Fuyutsuki – Toji | `10.77.4.164` | `/30` | `10.77.4.165` / `10.77.4.166` | `10.77.4.167` | 2 | 0 |
| 33 | Shigeru – Makoto | `10.77.4.168` | `/30` | `10.77.4.169` / `10.77.4.170` | `10.77.4.171` | 2 | 0 |

### Penjelasan Hitungan Satu Subnet (`Switch-Garden`)

- **Kebutuhan IP usable:** 14 host (`Garden-Staff`) + 1 IP gateway (`Shinji`) = **15 IP usable**.
- **Penentuan bit host ($h$):** Dicari nilai terkecil yang memenuhi $2^h - 2 \ge 15$.
  - Jika $h = 4$, maka $2^4 - 2 = 14$ (tidak mencukupi).
  - Jika $h = 5$, maka $2^5 - 2 = 30 \ge 15$ (memenuhi).
- **Prefix & ukuran blok:** Prefix $= 32 - 5 = /27$, ukuran blok $= 2^5 = 32$ alamat.
- **Alokasi alamat:** Melanjutkan setelah `Switch-Relay` (`10.77.3.224/27`, broadcast `10.77.3.255`), diperoleh:
  - Network: `10.77.4.0/27`
  - Gateway: `10.77.4.1`
  - Broadcast: `10.77.4.31`
  - Sisa IP usable: $30 - 15 = 15$ alamat.

### Pohon IP (Jalur `Dummy-Plug` $\rightarrow$ `Armisael` Dilingkari)

5 subnet pada jalur **Dummy-Plug $\rightarrow$ Armisael** yang dilingkari pada pohon IP:
1. `10.77.4.48/29` (Switch-Lab — Dummy-Plug)
2. `10.77.4.156/30` (Link Ritsuko – Asuka)
3. `10.77.4.116/30` (Link Asuka – Gendou)
4. `10.77.4.128/30` (Link Gendou – Shigeru)
5. `10.77.4.80/29` (Switch-Monolith — Armisael)

![Pohon IP Soal 1]()

---

## Soal 2

Seluruh node dipasang di GNS3 sesuai topologi, lalu setiap interface pada 17 router diberi alamat IP sesuai tabel VLSM Soal 1 melalui `/etc/network/interfaces`. Hanya **Gendou** yang terhubung ke **NAT1** menggunakan **DHCP**, dan **IP forwarding** diaktifkan pada seluruh router melalui perintah `up sysctl -w net.ipv4.ip_forward=1`.

### Konfigurasi `/etc/network/interfaces` Router

**Gendou:**
```text
auto eth0
iface eth0 inet dhcp
    up sysctl -w net.ipv4.ip_forward=1

auto eth1
iface eth1 inet static
    address 10.77.4.105
    netmask 255.255.255.252

auto eth2
iface eth2 inet static
    address 10.77.4.133
    netmask 255.255.255.252

auto eth3
iface eth3 inet static
    address 10.77.4.129
    netmask 255.255.255.252

auto eth4
iface eth4 inet static
    address 10.77.4.125
    netmask 255.255.255.252

auto eth5
iface eth5 inet static
    address 10.77.4.121
    netmask 255.255.255.252

auto eth6
iface eth6 inet static
    address 10.77.4.117
    netmask 255.255.255.252

auto eth7
iface eth7 inet static
    address 10.77.4.113
    netmask 255.255.255.252

auto eth8
iface eth8 inet static
    address 10.77.4.109
    netmask 255.255.255.252
```

**Yui, Mari, Rei, Shinji:**
```text
# Yui
auto eth0
iface eth0 inet static
    address 10.77.4.110
    netmask 255.255.255.252
    up sysctl -w net.ipv4.ip_forward=1
auto eth1
iface eth1 inet static
    address 10.77.3.1
    netmask 255.255.255.128
auto eth2
iface eth2 inet static
    address 10.77.4.137
    netmask 255.255.255.252
auto eth3
iface eth3 inet static
    address 10.77.4.141
    netmask 255.255.255.252
auto eth4
iface eth4 inet static
    address 10.77.4.145
    netmask 255.255.255.252

# Mari (eth0: 10.77.4.138/30, eth1: 10.77.4.33/28)
# Rei (eth0: 10.77.4.142/30, eth1: 10.77.0.1/23)
# Shinji (eth0: 10.77.4.146/30, eth1: 10.77.4.1/27)
```

**Hikari, Kaworu, Asuka, Kensuke, Ritsuko:**
```text
# Hikari (eth0: 10.77.4.114/30, eth1: 10.77.2.1/24, eth2: 10.77.4.149/30)
# Kaworu (eth0: 10.77.4.150/30, eth1: 10.77.3.225/27)
# Asuka (eth0: 10.77.4.118/30, eth1: 10.77.4.97/30, eth2: 10.77.4.157/30, eth3: 10.77.4.153/30)
# Kensuke (eth0: 10.77.4.154/30, eth1: 10.77.3.129/26)
# Ritsuko (eth0: 10.77.4.158/30, eth1: 10.77.4.49/29)
```

**Kaji, Misato, Shigeru, Makoto, Fuyutsuki, Maya, Toji:**
```text
# Kaji (eth0: 10.77.4.122/30, eth1: 10.77.4.101/30)
# Misato (eth0: 10.77.4.126/30, eth1: 10.77.4.65/29)
# Shigeru (eth0: 10.77.4.130/30, eth1: 10.77.4.169/30, eth2: 10.77.4.81/29)
# Makoto (eth0: 10.77.4.170/30, eth1: 10.77.4.89/29)
# Fuyutsuki (eth0: 10.77.4.134/30, eth1: 10.77.4.57/29, eth2: 10.77.4.161/30, eth3: 10.77.4.165/30)
# Maya (eth0: 10.77.4.162/30, eth1: 10.77.3.193/27)
# Toji (eth0: 10.77.4.166/30, eth1: 10.77.4.73/29)
```

### Bukti Verifikasi (`ip -br a` dan Ping Antar-Router)

![Hasil ip -br di Router]()

![Hasil Ping Dua Ujung Link Antar-Router]()

---

## Soal 3

Seluruh subnet dihubungkan menggunakan **routing statis** dengan jumlah rute seminimal mungkin. Router selain Gendou menggunakan satu *default route* ke router induk di atasnya dan rute spesifik ke subnet di bawahnya. Pada **Gendou**, diterapkan **route summarization** untuk subnet satu cabang yang bersebelahan dan sejajar batas CIDR:
- Cabang Yui: `10.77.4.136/30` + `10.77.4.140/30` $\rightarrow$ `10.77.4.136/29`
- Cabang Asuka: `10.77.4.152/30` + `10.77.4.156/30` $\rightarrow$ `10.77.4.152/29`
- Cabang Shigeru: `10.77.4.80/29` + `10.77.4.88/29` $\rightarrow$ `10.77.4.80/28`
- Cabang Fuyutsuki: `10.77.4.160/30` + `10.77.4.164/30` $\rightarrow$ `10.77.4.160/29`

### Script `/root/soal3.sh` (Gendou)

```bash
cat << 'EOF' > /root/soal3.sh
#!/bin/sh
# Cabang Yui
ip route replace 10.77.0.0/23 via 10.77.4.110
ip route replace 10.77.3.0/25 via 10.77.4.110
ip route replace 10.77.4.0/27 via 10.77.4.110
ip route replace 10.77.4.32/28 via 10.77.4.110
ip route replace 10.77.4.136/29 via 10.77.4.110
ip route replace 10.77.4.144/30 via 10.77.4.110

# Cabang Hikari
ip route replace 10.77.2.0/24 via 10.77.4.114
ip route replace 10.77.3.224/27 via 10.77.4.114
ip route replace 10.77.4.148/30 via 10.77.4.114

# Cabang Asuka
ip route replace 10.77.3.128/26 via 10.77.4.118
ip route replace 10.77.4.48/29 via 10.77.4.118
ip route replace 10.77.4.96/30 via 10.77.4.118
ip route replace 10.77.4.152/29 via 10.77.4.118

# Cabang Kaji & Misato
ip route replace 10.77.4.100/30 via 10.77.4.122
ip route replace 10.77.4.64/29 via 10.77.4.126

# Cabang Shigeru
ip route replace 10.77.4.80/28 via 10.77.4.130
ip route replace 10.77.4.168/30 via 10.77.4.130

# Cabang Fuyutsuki
ip route replace 10.77.3.192/27 via 10.77.4.134
ip route replace 10.77.4.56/29 via 10.77.4.134
ip route replace 10.77.4.72/29 via 10.77.4.134
ip route replace 10.77.4.160/29 via 10.77.4.134
EOF
chmod +x /root/soal3.sh && /root/soal3.sh
```

### Script `/root/soal3.sh` (Router Perantara & Router Ujung)

```bash
# Yui
ip route replace default via 10.77.4.109
ip route replace 10.77.4.32/28 via 10.77.4.138
ip route replace 10.77.0.0/23 via 10.77.4.142
ip route replace 10.77.4.0/27 via 10.77.4.146

# Hikari
ip route replace default via 10.77.4.113
ip route replace 10.77.3.224/27 via 10.77.4.150

# Asuka
ip route replace default via 10.77.4.117
ip route replace 10.77.3.128/26 via 10.77.4.154
ip route replace 10.77.4.48/29 via 10.77.4.158

# Shigeru
ip route replace default via 10.77.4.129
ip route replace 10.77.4.88/29 via 10.77.4.170

# Fuyutsuki
ip route replace default via 10.77.4.133
ip route replace 10.77.3.192/27 via 10.77.4.162
ip route replace 10.77.4.72/29 via 10.77.4.166

# 11 Router Ujung: masing-masing 1 default route ke IP router induknya
```

### Script Pengujian `/root/soal3_tes.sh` (di Armisael)

Setelah IP dan gateway sementara dipasang di Armisael (`10.77.4.83/29`), Lilith (`10.77.4.74/29`), Sachiel (`10.77.3.130/26`), dan Comm-Net (`10.77.2.2/24`), dilakukan pengujian ping dari Armisael:

```bash
cat << 'EOF' > /root/soal3_tes.sh
#!/bin/sh
echo "=== Ping Lilith (10.77.4.74) ==="
ping -c 3 10.77.4.74
echo "=== Ping Sachiel (10.77.3.130) ==="
ping -c 3 10.77.3.130
echo "=== Ping Comm-Net (10.77.2.2) ==="
ping -c 3 10.77.2.2
EOF
chmod +x /root/soal3_tes.sh && /root/soal3_tes.sh
```

![Hasil Ping Armisael ke Lilith, Sachiel, dan Comm-Net]()

---

## Soal 4

Agar seluruh jaringan internal dapat mengakses internet lewat Gendou meskipun IP dari NAT1 berganti-ganti, digunakan target **`MASQUERADE`** pada chain `POSTROUTING` tabel `nat` di Gendou khusus untuk traffic dari `10.77.0.0/16` yang keluar melalui `eth0`. Traffic antar-LAN tetap mempertahankan IP asli pengirimnya. Selain itu, ditambahkan `nameserver 8.8.8.8` pada `/etc/resolv.conf` di node uji agar bisa melakukan resolusi DNS.

### Script `/root/soal4.sh` (Gendou)

```bash
cat << 'EOF' > /root/soal4.sh
#!/bin/sh
command -v iptables >/dev/null 2>&1 || (apt-get update -qq && apt-get install -y -qq iptables)

iptables -t nat -F POSTROUTING
iptables -t nat -A POSTROUTING -s 10.77.0.0/16 -o eth0 -j MASQUERADE
EOF
chmod +x /root/soal4.sh && /root/soal4.sh
```

### Script `/root/soal4.sh` & `/root/soal4_tes.sh` (Armisael & Lilith)

```bash
cat << 'EOF' > /root/soal4.sh
#!/bin/sh
echo "nameserver 8.8.8.8" > /etc/resolv.conf
echo "nameserver 1.1.1.1" >> /etc/resolv.conf
EOF
chmod +x /root/soal4.sh && /root/soal4.sh

cat << 'EOF' > /root/soal4_tes.sh
#!/bin/sh
ping -c 3 8.8.8.8
ping -c 3 deb.debian.org
EOF
chmod +x /root/soal4_tes.sh && /root/soal4_tes.sh
```

### Bukti Verifikasi (Ping Internet & Counter NAT di Gendou)

![Hasil Ping 8.8.8.8 dan deb.debian.org dari Armisael dan Lilith]()

![Counter Rule NAT POSTROUTING di Gendou]()

---

## Soal 5

Untuk mengunci pintu masuk dari NAT1 agar koneksi baru dari luar tidak dapat masuk namun jaringan internal tetap dapat keluar, policy chain `INPUT` dan `FORWARD` pada **Gendou** diubah menjadi `DROP`. Rule firewall mengizinkan loopback, DHCP dari NAT1 (`eth0` UDP 67 $\rightarrow$ 68), koneksi `ESTABLISHED,RELATED`, traffic internal ke internet, traffic antar-LAN, dan traffic internal ke Gendou sendiri. Seluruh konfigurasi Soal 2 hingga 5 digabungkan dalam `/root/gendo.sh`.

### Script `/root/gendo.sh` (Gendou)

```bash
cat << 'EOF' > /root/gendo.sh
#!/bin/sh
# Soal 2: IP Forwarding
sysctl -w net.ipv4.ip_forward=1 >/dev/null

# Soal 3: Routing Statis (Summarized)
ip route replace 10.77.0.0/23 via 10.77.4.110
ip route replace 10.77.3.0/25 via 10.77.4.110
ip route replace 10.77.4.0/27 via 10.77.4.110
ip route replace 10.77.4.32/28 via 10.77.4.110
ip route replace 10.77.4.136/29 via 10.77.4.110
ip route replace 10.77.4.144/30 via 10.77.4.110

ip route replace 10.77.2.0/24 via 10.77.4.114
ip route replace 10.77.3.224/27 via 10.77.4.114
ip route replace 10.77.4.148/30 via 10.77.4.114

ip route replace 10.77.3.128/26 via 10.77.4.118
ip route replace 10.77.4.48/29 via 10.77.4.118
ip route replace 10.77.4.96/30 via 10.77.4.118
ip route replace 10.77.4.152/29 via 10.77.4.118

ip route replace 10.77.4.100/30 via 10.77.4.122
ip route replace 10.77.4.64/29 via 10.77.4.126

ip route replace 10.77.4.80/28 via 10.77.4.130
ip route replace 10.77.4.168/30 via 10.77.4.130

ip route replace 10.77.3.192/27 via 10.77.4.134
ip route replace 10.77.4.56/29 via 10.77.4.134
ip route replace 10.77.4.72/29 via 10.77.4.134
ip route replace 10.77.4.160/29 via 10.77.4.134

# Soal 4: NAT Masquerade
iptables -t nat -F POSTROUTING
iptables -t nat -A POSTROUTING -s 10.77.0.0/16 -o eth0 -j MASQUERADE

# Soal 5: Firewall (Kunci NAT1)
iptables -F
iptables -X
iptables -P INPUT DROP
iptables -P FORWARD DROP
iptables -P OUTPUT ACCEPT

# Loopback & DHCP dari NAT1
iptables -A INPUT -i lo -j ACCEPT
iptables -A INPUT -i eth0 -p udp --sport 67 --dport 68 -j ACCEPT

# (a) Balasan koneksi yang sudah ada
iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
iptables -A FORWARD -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT

# (d) Traffic dari jaringan internal ke Gendou sendiri
iptables -A INPUT ! -i eth0 -s 10.77.0.0/16 -j ACCEPT

# (b) Traffic dari jaringan internal ke internet
iptables -A FORWARD ! -i eth0 -o eth0 -s 10.77.0.0/16 -j ACCEPT

# (c) Traffic antar-LAN
iptables -A FORWARD ! -i eth0 ! -o eth0 -s 10.77.0.0/16 -d 10.77.0.0/16 -j ACCEPT
EOF
chmod +x /root/gendo.sh && /root/gendo.sh
```

### Bukti Verifikasi (`iptables -S` di Gendou & Ping `8.8.8.8` dari Armisael)

![Output iptables -S di Gendou]()

![Hasil Ping 8.8.8.8 dari Armisael]()

---

## Soal 6

Seluruh 25 node non-router diberikan konfigurasi **IP statis, gateway, dan DNS** secara permanen di `/etc/network/interfaces` menggunakan script `/root/soal6.sh` agar konfigurasi tetap bertahan setelah restart.

### Daftar Alamat IP Node Non-Router

| Node | IP / Prefix | Gateway | Node | IP / Prefix | Gateway |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Quarters | `10.77.0.2/23` | `10.77.0.1` | Magi-Melchior | `10.77.4.60/29` | `10.77.4.57` |
| Comm-Net | `10.77.2.2/24` | `10.77.2.1` | Unit-01 | `10.77.4.66/29` | `10.77.4.65` |
| Hangar-Crew | `10.77.3.2/25` | `10.77.3.1` | Unit-02 | `10.77.4.67/29` | `10.77.4.65` |
| Sachiel | `10.77.3.130/26` | `10.77.3.129` | Unit-00 | `10.77.4.68/29` | `10.77.4.65` |
| Pen-Pen | `10.77.3.194/27` | `10.77.3.193` | Lilith | `10.77.4.74/29` | `10.77.4.73` |
| Medic-Bay | `10.77.3.195/27` | `10.77.3.193` | Adam | `10.77.4.75/29` | `10.77.4.73` |
| Relay-Node | `10.77.3.226/27` | `10.77.3.225` | Zeruel | `10.77.4.82/29` | `10.77.4.81` |
| Garden-Staff | `10.77.4.2/27` | `10.77.4.1` | Armisael | `10.77.4.83/29` | `10.77.4.81` |
| Dock-Crew | `10.77.4.34/28` | `10.77.4.33` | Radar-Array | `10.77.4.90/29` | `10.77.4.89` |
| Entry-Plug | `10.77.4.50/29` | `10.77.4.49` | Archive-Vault | `10.77.4.98/30` | `10.77.4.97` |
| Dummy-Plug | `10.77.4.51/29` | `10.77.4.49` | Spy | `10.77.4.102/30` | `10.77.4.101` |
| Magi-Casper | `10.77.4.58/29` | `10.77.4.57` | Tokyo3-Core | `10.77.4.106/30` | `10.77.4.105` |
| Magi-Balthasar | `10.77.4.59/29` | `10.77.4.57` | | | |

### Contoh Script `/root/soal6.sh` & `/root/soal6_tes.sh` (Unit-01)

```bash
cat << 'EOF' > /root/soal6.sh
#!/bin/sh
cat << 'NET' > /etc/network/interfaces
auto eth0
iface eth0 inet static
    address 10.77.4.66
    netmask 255.255.255.248
    gateway 10.77.4.65
    up echo "nameserver 8.8.8.8" > /etc/resolv.conf
NET
ip addr flush dev eth0 && ip addr add 10.77.4.66/29 dev eth0 && ip link set eth0 up
ip route replace default via 10.77.4.65
echo "nameserver 8.8.8.8" > /etc/resolv.conf
EOF
chmod +x /root/soal6.sh && /root/soal6.sh

cat << 'EOF' > /root/soal6_tes.sh
#!/bin/sh
ip -br a
ping -c 3 10.77.4.65
EOF
chmod +x /root/soal6_tes.sh && /root/soal6_tes.sh
```

![Output ip -br dan Ping Gateway di Unit-01]()

---

## Soal 7

Pada ketiga node MAGI (`Magi-Casper`, `Magi-Balthasar`, dan `Magi-Melchior`), dipasang paket `iptables` dan `netcat-traditional` melalui `/root/<nama>.sh`, kemudian dijalankan listener TCP sesuai glosarium: port `5000` di Casper & Melchior, serta port `5000` dan `5001` di Balthasar. Pengujian dilakukan dengan mengirim pesan dari **Lilith** ke keempat port layanan tersebut.

### Script di Ketiga Node MAGI

```bash
# Magi-Casper (/root/casper.sh)
cat << 'EOF' > /root/casper.sh
#!/bin/sh
apt-get update -qq && apt-get install -y -qq iptables netcat-traditional
pkill -f "nc.traditional -l -p 5000" 2>/dev/null
: > /tmp/casper_5000.log
(while true; do nc.traditional -l -p 5000 >> /tmp/casper_5000.log 2>&1; done) &
EOF
chmod +x /root/casper.sh && /root/casper.sh

# Magi-Balthasar (/root/balthasar.sh)
cat << 'EOF' > /root/balthasar.sh
#!/bin/sh
apt-get update -qq && apt-get install -y -qq iptables netcat-traditional
pkill -f "nc.traditional -l -p 5000" 2>/dev/null
pkill -f "nc.traditional -l -p 5001" 2>/dev/null
: > /tmp/balthasar_5000.log
: > /tmp/balthasar_5001.log
(while true; do nc.traditional -l -p 5000 >> /tmp/balthasar_5000.log 2>&1; done) &
(while true; do nc.traditional -l -p 5001 >> /tmp/balthasar_5001.log 2>&1; done) &
EOF
chmod +x /root/balthasar.sh && /root/balthasar.sh

# Magi-Melchior (/root/melchior.sh)
cat << 'EOF' > /root/melchior.sh
#!/bin/sh
apt-get update -qq && apt-get install -y -qq iptables netcat-traditional
pkill -f "nc.traditional -l -p 5000" 2>/dev/null
: > /tmp/melchior_5000.log
(while true; do nc.traditional -l -p 5000 >> /tmp/melchior_5000.log 2>&1; done) &
EOF
chmod +x /root/melchior.sh && /root/melchior.sh
```

### Script Pengujian `/root/soal7_tes.sh` (di Lilith)

```bash
cat << 'EOF' > /root/soal7_tes.sh
#!/bin/sh
echo "Halo Casper 5000 dari Lilith" | nc -w 2 10.77.4.58 5000
echo "Halo Balthasar 5000 dari Lilith" | nc -w 2 10.77.4.59 5000
echo "Halo Balthasar 5001 dari Lilith" | nc -w 2 10.77.4.59 5001
echo "Halo Melchior 5000 dari Lilith" | nc -w 2 10.77.4.60 5000
EOF
chmod +x /root/soal7_tes.sh && /root/soal7_tes.sh
```

### Bukti Pesan Diterima di Tiap Node MAGI

![Log Pesan di Magi-Casper]()

![Log Pesan di Magi-Balthasar (5000 dan 5001)]()

![Log Pesan di Magi-Melchior]()

---

## Soal 8

Pada **Pen-Pen** dan **Tokyo3-Core**, diaktifkan layanan `nc` pada port `80` dengan balasan `Welcome to {hostname}`, serta port `8080` pada Tokyo3-Core dengan balasan `syifa imutz`. Pengujian dari **Lilith** memastikan ketiga port layanan merespons dengan benar, sementara koneksi ke port yang tidak dilayani (`22`) langsung gagal (`Connection refused`, durasi 0 detik, exit code 1) dan tidak menggantung.

### Script `/root/soal8.sh` (Pen-Pen & Tokyo3-Core)

```bash
# Pen-Pen (/root/soal8.sh)
cat << 'EOF' > /root/soal8.sh
#!/bin/sh
command -v nc.traditional >/dev/null 2>&1 || (apt-get update -qq && apt-get install -y -qq netcat-traditional)
pkill -f "nc.traditional -l -p 80" 2>/dev/null
(while true; do printf "Welcome to %s\n" "$(hostname)" | nc.traditional -l -p 80 -q 1 >/dev/null 2>&1; done) &
EOF
chmod +x /root/soal8.sh && /root/soal8.sh

# Tokyo3-Core (/root/soal8.sh)
cat << 'EOF' > /root/soal8.sh
#!/bin/sh
command -v nc.traditional >/dev/null 2>&1 || (apt-get update -qq && apt-get install -y -qq netcat-traditional)
pkill -f "nc.traditional -l -p 80" 2>/dev/null
pkill -f "nc.traditional -l -p 8080" 2>/dev/null
(while true; do printf "Welcome to %s\n" "$(hostname)" | nc.traditional -l -p 80 -q 1 >/dev/null 2>&1; done) &
(while true; do printf "syifa imutz\n" | nc.traditional -l -p 8080 -q 1 >/dev/null 2>&1; done) &
EOF
chmod +x /root/soal8.sh && /root/soal8.sh
```

### Script Pengujian `/root/soal8_tes.sh` (di Lilith)

```bash
cat << 'EOF' > /root/soal8_tes.sh
#!/bin/sh
echo "=== 1. Pen-Pen Port 80 (10.77.3.194:80) ==="
nc -w 2 10.77.3.194 80 < /dev/null

echo "=== 2. Tokyo3-Core Port 80 (10.77.4.106:80) ==="
nc -w 2 10.77.4.106 80 < /dev/null

echo "=== 3. Tokyo3-Core Port 8080 (10.77.4.106:8080) ==="
nc -w 2 10.77.4.106 8080 < /dev/null

echo "=== 4. Port 22 Pen-Pen ==="
t1=$(date +%s)
nc -v -w 5 10.77.3.194 22 < /dev/null
rc=$?
t2=$(date +%s)
echo "Exit code: $rc | Durasi: $((t2 - t1)) detik"

echo "=== 5. Port 22 Tokyo3-Core ==="
t1=$(date +%s)
nc -v -w 5 10.77.4.106 22 < /dev/null
rc=$?
t2=$(date +%s)
echo "Exit code: $rc | Durasi: $((t2 - t1)) detik"
EOF
chmod +x /root/soal8_tes.sh && /root/soal8_tes.sh
```

![Hasil Pengujian nc dari Lilith ke Pen-Pen dan Tokyo3-Core]()

---

## Soal 9

Setelah menjalankan script sabotase `break_routing.sh` di **Shigeru**, Lilith tidak mendapatkan balasan ping dari Armisael, sementara Armisael masih dapat mem-ping gateway-nya (`10.77.4.81`).

### Analisis Penyebab Kerusakan

Berdasarkan pemeriksaan `ip_forward`, `ip route`, `ip rule`, dan `ip route show table 100` di Shigeru, ditemukan **4 penyebab**:
1. **`ip_forward` dimatikan (`0`):** Shigeru tidak meneruskan paket antar-interface. Armisael tetap bisa mem-ping gateway-nya sendiri karena trafik ditujukan ke Shigeru secara lokal (*INPUT/OUTPUT*), bukan di-*forward*.
2. **Default route dibelokkan ke Makoto (`10.77.4.170` via `eth1`):** Seharusnya mengarah ke Gendou (`10.77.4.129` via `eth0`), sehingga memicu *routing loop* dengan Makoto.
3. **Rute `blackhole 10.77.4.72/29`:** Membuang diam-diam semua paket menuju subnet Switch-Dogma (Lilith).
4. **Policy-Based Routing (`iif eth2 lookup 100 priority 90` + `unreachable default table 100`):** Memaksa seluruh paket dari `eth2` (Switch-Monolith) masuk ke `table 100` sebelum melihat tabel `main`, sehingga langsung ditolak sebagai *unreachable*.

### Script Perbaikan `/root/soal9.sh` (di Shigeru)

```bash
cat << 'EOF' > /root/soal9.sh
#!/bin/sh
sysctl -w net.ipv4.ip_forward=1 >/dev/null
ip route replace default via 10.77.4.129 dev eth0
ip route del blackhole 10.77.4.72/29 2>/dev/null
ip rule del iif eth2 lookup 100 priority 90 2>/dev/null
ip route flush table 100 2>/dev/null
EOF
chmod +x /root/soal9.sh && /root/soal9.sh
```

### Bukti Verifikasi (Ping Lilith ke Armisael)

![Hasil Ping Lilith ke Armisael Setelah Perbaikan]()

---

## Soal 10

Setelah menjalankan script sabotase `break_nat.sh` di **Misato** dan **Gendou**, node di Switch-Cage (`Unit-01`, `Unit-02`, `Unit-00`) tidak dapat mem-ping `8.8.8.8`, tetapi masih dapat mem-ping Lilith (`10.77.4.74`), sedangkan **Spy** di Switch-Intel tetap dapat mem-ping `8.8.8.8` secara normal.

### Analisis Penyebab & Penjelasan

Dari hasil pemeriksaan `iptables -t nat -L POSTROUTING -v -n`, ditemukan **2 penyebab**:
1. **SNAT palsu di Misato:** Rule `POSTROUTING` menerjemahkan sumber dari `10.77.4.64/29` menuju tujuan luar `10.77.0.0/16` menjadi `172.31.99.1`. Paket tersebut langsung di-DROP oleh firewall `FORWARD` Gendou karena bukan berasal dari `10.77.0.0/16`.
2. **Rule `RETURN` di Gendou:** Rule pada baris pertama `POSTROUTING` untuk sumber `10.77.4.64/29` keluar `eth0` membuat paket langsung keluar dari chain sebelum menyentuh `MASQUERADE`, sehingga paket keluar ke internet masih membawa IP privat `10.77.4.x`.

- **Mengapa Cage terdampak tetapi Intel tidak (dan Cage $\rightarrow$ Lilith normal)?**
  Kedua rule sabotase hanya menargetkan subnet `10.77.4.64/29` (Switch-Cage). Spy (`10.77.4.100/30`) berada di bawah router Kaji sehingga tidak melewati Misato dan tidak terkena rule `RETURN` di Gendou. Sementara itu, ping Cage $\rightarrow$ Lilith tetap normal karena rule SNAT Misato menggunakan syarat `! -d 10.77.0.0/16` (tidak berlaku untuk tujuan internal) dan tidak keluar melalui `eth0` Gendou.
- **Di router mana alamat sumber diterjemahkan?**
  Alamat sumber diterjemahkan di router **Gendou** melalui rule `MASQUERADE` pada chain `POSTROUTING` tabel `nat` ke arah interface `eth0` (NAT1).

### Script Perbaikan `/root/soal10.sh` (di Misato & Gendou)

```bash
# Misato (/root/soal10.sh)
cat << 'EOF' > /root/soal10.sh
#!/bin/sh
iptables -t nat -D POSTROUTING -s 10.77.4.64/29 ! -d 10.77.0.0/16 -o eth0 -j SNAT --to-source 172.31.99.1 2>/dev/null
EOF
chmod +x /root/soal10.sh && /root/soal10.sh

# Gendou (/root/soal10.sh)
cat << 'EOF' > /root/soal10.sh
#!/bin/sh
iptables -t nat -D POSTROUTING -s 10.77.4.64/29 -o eth0 -j RETURN 2>/dev/null
EOF
chmod +x /root/soal10.sh && /root/soal10.sh
```

### Bukti Verifikasi (Ping `8.8.8.8` dari Unit-01 & NAT Gendou)

![Hasil Ping 8.8.8.8 dari Unit-01]()

![Output iptables -t nat -L POSTROUTING -v -n di Gendou]()

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


