## Tutorial Install RDP menggunakan XRPD dan Desktop Environment XFCE di Ubuntu 22.04 LTS
### Update & Install Dependensi Dasar
```
sudo apt update && sudo apt upgrade -y
sudo apt install -y wget curl nano software-properties-common
```
> Jalankan perintah diatas untuk memastikan sistem dalam kondisi terbaru dan menghindari konflik paket.

### Install XFCE Desktop Environment
```
sudo apt install xfce4 xfce4-goodies -y
```
> Penjelasan:
> - `xfce4` → paket XFCE utama.
> - `xfce4-goodies` → Menambahkan fitur tambahan seperti power manager, terminal, dsb.

### Install Paket Pendukung Tambahan
Beberapa paket tambahan diperlukan agar XRDP dan XFCE berjalan optimal:
```
sudo apt install dbus-x11 xorgxrdp x11-xserver-utils -y
```
> Penjelasan:
> - `dbus-x11` → Diperlukan agar XFCE bisa berjalan dengan XRDP.
> - `xorgxrdp` → Driver Xorg untuk XRDP agar tampilan lebih stabil.
> - `x11-xserver-utils` → Berisi perintah tambahan untuk pengelolaan X server.

### Install XRDP
```
sudo apt install xrdp -y
```
Setelah itu, pastikan XRDP berjalan dan aktif saat booting:
```
sudo systemctl enable xrdp
sudo systemctl start xrdp
```
Cek status XRDP:
```
sudo systemctl status xrdp
```
> Jika berjalan dengan baik, output-nya harus menunjukkan **active (running)**.

### Memasukkan user xrdp ke grup ssl-cert
```
sudo adduser xrdp ssl-cert
```


