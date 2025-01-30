## Perintah Ketika Pertama Kali Install VPS (Ubuntu 22.04 LTS) 

### Update dan Upgrade Repositori
```
sudo apt update -y && apt upgrade -y
```

### Buat User Baru
```
sudo adduser nama_user
```
> Gantilah nama_user dengan nama yang diinginkan, lalu ikuti instruksi untuk mengatur password dan informasi lainnya.

### Tambahkan User ke Grup sudo (Opsional)
```
sudo usermod -aG sudo nama_user
```
> Gantilah nama_user dengan nama yang diinginkan, perintah ini untuk memberi hak akses root ke user biasa.

### Cek Grup User dengan groups
```
groups nama_user
```
> Cek user apakah sudah masuk ke dalam grup sudo

### Cek Grup User dengan id
```
id nama_user
```
> Jika user sudah masuk grup sudo, outputnya akan mencantumkan **groups=1000(nama_user),27(sudo)**

### Cek Daftar User Tanpa UID
```
awk -F: '$3 >= 1000 && $3 < 60000 {print $1}' /etc/passwd
```
> - User biasa memiliki **UID ≥ 1000** (user biasa, bukan sistem).
> - User root memiliki **(UID = 0)** dan tidak termasuk dalam daftar.

### Cek Daftar User Dengan Nama dan UID
```
awk -F: '$3 >= 1000 && $3 < 60000 {print "User: "$1", UID: "$3}' /etc/passwd
```
> Menampilkan daftar dengan nama dan UID untuk memudahkan pengecekan.

### Beralih User dengan su (Switch User)
```
su - nama_user
```
> - `-` memastikan bahwa lingkungan user tujuan dimuat sepenuhnya.
> - Jika diminta password, masukkan password user yang akan dituju.
> - Jika ingin masuk dengan hak akses root gunakan `sudo su` sebelum `-`.

### Gunakan sudo -u untuk Beralih ke User Lain Tanpa Logout (Opsional)
```
sudo -u nama_user -i
```
> Jika kamu memiliki akses sudo, kamu bisa berpindah user tanpa harus mengetahui password user tujuan.

## Mengaktifkan Firewall dan Menambahkan Port Tertentu
### Cek Status Firewall
```
sudo ufw status verbose
```
> - Jika statusnya **inactive**, berarti UFW belum aktif.
> - Jika **active**, berarti UFW sudah berjalan.

### Cek Port yang Sedang Digunakan di Sistem
```
sudo ss -tulnp
```
> Sebelum menambahkan aturan firewall, pastikan tidak ada port yang berbenturan.

### Izinkan Port yang Diperlukan
- SSH (port 22)
```
sudo ufw allow 22/tcp
```
> ⚠️ PENTING: Jika kamu menggunakan ssh pastikan port SSH sudah diizinkan sebelum mengaktifkan UFW agar tidak terkunci dari remote access!

- HTTP (port 80)
```
sudo ufw allow 80/tcp
```

- HTTPS (port 443)
```
sudo ufw allow 443/tcp
```

- RDP / Remote Desktop (port 3389)
```
sudo ufw allow 3389/tcp
```

### Mengaktifkan Firewall
```
sudo ufw enable
```

### Mengizinkan Port Hanya untuk IP Tertentu
- **Mengizinkan Koneksi Masuk (Inbound)**
```
sudo ufw allow in from 127.0.0.1 to any port 53
```
> Contoh alamat IP `127.0.0.1`, diizinkan untuk koneksi masuk melalui port `53`

- **Mengizinkan Koneksi Keluar (Outbound)**
```
sudo ufw allow out from 127.0.0.1 to any port 53
```
> Contoh alamat IP `127.0.0.1`, diizinkan untuk koneksi keluar melalui port `53`

### Menolak Semua Koneksi Lain ke Port Tertentu
- **Menolak Semua Koneksi Masuk (Inbound)**
```
sudo ufw deny in to any port 53
```
- **Menolak Semua Koneksi Keluar (Outbound)**
```
sudo ufw deny out to any port 53
```
- **Menolak Semua Koneksi Masuk dan Keluar dari IP Tertentu**
```
sudo ufw deny in from 127.0.0.1 to any port 53
sudo ufw deny out from 127.0.0.1 to any port 53
```
> Contoh ip `127.0.0.1` tidak diizinkan untuk mengakses koneksi masuk dan keluar melalui port `53`

### Menghapus Aturan Firewall Dengan List Number
```
sudo ufw status numbered
```
> Perintah tersebut akan menampilkan list aturan firewall yang telah diterapkan
```
sudo ufw delete 1
```
> Contoh perintah diatas akan menghapus aturan yang berada di list nomor '1'

## Cara Mengganti Port Default SSH (22)
Login terlebih dahulu ke VPS dengan username, password dan port yang telah kalian tentukan sebelumnya 
### Edit Konfigurasi SSH
```
sudo nano /etc/ssh/sshd_config
```
Cari baris berikut:
```
#Port 22
```
Hapus tanda # dan ubah angka 22 menjadi port yang diinginkan, misalnya 2222:
```
Port 2222
```
> Setelah diganti tekan (Ctrl + X, lalu tekan Y dan Enter)
🚨 Catatan:
> - Jangan gunakan port yang sudah digunakan layanan lain (cek dengan `sudo netstat -tulnp`).
> - Hindari port umum seperti `80, 443, 3306, dll.`
> - Gunakan port di atas `2000` untuk keamanan tambahan. Port di atas 2000 lebih jarang digunakan oleh layanan lain.

### Izinkan Port Baru di Firewall (UFW)
```
sudo ufw allow 2222/tcp
```
### Cek Port yang Baru di Tambahkan 
```
sudo ufw status verbose
```
### Restart SSH Service
```
sudo systemctl restart ssh
```
atau
```
sudo service ssh restart
```
### Tes Login dengan Port Baru
Dari komputer lokal, coba login dengan port baru misalnya via terminal:
```
ssh -p 2222 root@IP_VPS
```
> 🚨Catatan: Jangan tutup koneksi SSH lama sebelum memastikan koneksi dengan port baru berhasil.

### Blokir Port SSH Lama [22] (Opsional)
```
sudo ufw deny 22/tcp
```
> Jika sudah berhasil login dengan port baru, tutup akses ke port `22`. Atau bisa juga menghapusnya sesuaikan dengan kebutuhan.

