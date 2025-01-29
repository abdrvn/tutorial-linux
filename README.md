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

