# README.md

# Instalasi Web Server, Database Server, DNS Server, dan Mail Server

## Environment
- VirtualBox
- Proxmox
- Ubuntu Server
- Apache2
- MySQL
- Bind9
- Postfix

---

# Update Repository

```cpp
sudo apt update && sudo apt upgrade -y
```

---

# 1. Web Server (Apache2)

## Install Apache2

```cpp
sudo apt install apache2 -y
```

## Cek Status

```cpp
sudo systemctl status apache2
```

## Cek Firewall

```cpp
sudo ufw status
```

Jika aktif:

```cpp
sudo ufw disable
```

## Pengujian

Buka browser:

```cpp
http://IP_UBUNTU_SERVER
```

Contoh:

```cpp
http://192.168.1.101
```

---

# 2. Database Server (MySQL)

## Install MySQL

```cpp
sudo apt install mysql-server -y
```

## Verifikasi

```cpp
sudo systemctl status mysql
```

Jika belum running:

```cpp
sudo systemctl start mysql
```

## Akses Database

```cpp
sudo mysql -u root -p
```

---

# 3. DNS Server (Bind9)

## Install Bind9

```cpp
sudo apt install bind9 bind9utils bind9-doc -y
```

## Konfigurasi Domain

Edit file:

```cpp
sudo nano /etc/bind/named.conf.local
```

Isi:

```cpp
zone "david.lab" {
    type master;
    file "/etc/bind/db.david.lab";
};
```

## Copy File Zone

```cpp
sudo cp /etc/bind/db.local /etc/bind/db.david.lab
```

## Edit Zone

```cpp
sudo nano /etc/bind/db.david.lab
```

Isi:

```cpp
$TTL    604800
@       IN      SOA     alpi.lab. root.alpi.lab. (
                     2
                604800
                 86400
               2419200
                604800 )

@       IN      NS      alpi.lab.
@       IN      A       192.168.1.101
@       IN      MX 10   mail.alpi.lab.
www     IN      A       192.168.1.101
```

## Cek Konfigurasi

```cpp
sudo named-checkconf
```

## Edit Resolver

```cpp
sudo nano /etc/resolv.conf
```

Isi:

```cpp
nameserver 127.0.0.1
nameserver 1.1.1.1
```

## Restart Bind9

```cpp
sudo systemctl restart bind9
```

## Pengujian DNS

```cpp
nslookup alpi.lab
```

Jika di Windows gagal:

```cpp
ipconfig /flushdns
```

---

# 4. Mail Server (Postfix)

## Install Postfix

```cpp
sudo apt install postfix mailutils -y
```

Saat instalasi pilih:

```cpp
Internet Site
```

System mail name:

```cpp
alpi.lab
```

## Test Mail

```cpp
echo "Setup Infrastructure Selesai" | mail -s "Final Test" root
```

## Cek Inbox

```cpp
mail
```

Keluar:

```cpp
CTRL + Z
```

---

# 5. Deploy Web HTML

## Permission Folder

```cpp
sudo chmod -R $USER:$USER /var/www/html/
sudo chmod -R 755 /var/www/html/
```

## Hapus Default Apache

```cpp
sudo rm /var/www/html/index.html
```

## Upload File dari Windows

```cpp
scp -r nama_folder_web username@IP_UBUNTU_SERVER:/var/www/html
```

Contoh:

```cpp
scp -r kalkulator alpi@192.168.1.101:/var/www/html
```

## Permission HTML

```cpp
sudo chmod 644 /var/www/html/index.html
```

---

# 6. Pengujian Website

Buka browser:

```cpp
http://alpi.lab
```

Jika berhasil maka website tampil.

---

# 7. Pengujian Service

## Apache2

```cpp
systemctl status apache2
```

## MySQL

```cpp
systemctl status mysql
```

## Bind9

```cpp
systemctl status bind9
```

## Postfix

```cpp
systemctl status postfix
```

Pastikan semuanya:

```cpp
active (running)
```

---

# 8. Struktur Video

Isi video:

1. Login Ubuntu Server
2. Install Apache2
3. Install MySQL
4. Install Bind9
5. Install Postfix
6. Deploy Website
7. Pengujian Domain
8. Pengujian Service

Durasi sekitar 5-10 menit.

---

# Kesimpulan

Berhasil melakukan:

- Instalasi Web Server
- Instalasi Database Server
- Instalasi DNS Server
- Instalasi Mail Server
- Deploy Website HTML
- Pengujian Semua Service
- Akses Website Menggunakan Domain Lokal

