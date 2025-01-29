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

### Cek Daftar User Yang Ada di Sistem
```
awk -F: '$3 >= 1000 && $3 < 60000 {print $1}' /etc/passwd
```
> User biasa memiliki **UID ≥ 1000** (user biasa, bukan sistem).
> User root memiliki **(UID 0)** dan tidak termasuk dalam daftar.
