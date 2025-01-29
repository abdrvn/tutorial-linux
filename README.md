## Perintah Ketika Pertama Kali Install Linux (Ubuntu 22.04) 

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
> ⚠️ PENTING: Jika kamu menggunakan ssh pastikan SSH sudah diizinkan sebelum mengaktifkan UFW agar tidak terkunci dari remote access!

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

