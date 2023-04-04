# How To Start Raspberry Pi

> 原文：<https://blog.devgenius.io/how-to-start-raspberry-pi-633fd664c29f?source=collection_archive---------21----------------------->

![](img/302b0aa0c309b2d1f5de80968201f899.png)

Source : [https://www.komplett.no/product/1136633/datautstyr/pc-komponenter/hovedkort/integrert-cpu/raspberry-pi-4-model-b-4gb-starter-kit](https://www.komplett.no/product/1136633/datautstyr/pc-komponenter/hovedkort/integrert-cpu/raspberry-pi-4-model-b-4gb-starter-kit)

Download sistem operasi Raspbian dan dua tool untuk mendukung instalasi.

a. Download Raspbian OS image di sini : [https://www.raspberrypi.org/downloads/](https://www.raspberrypi.org/downloads/)

b. Download SDformatter di sini : [https://www.sdcard.org/downloads/formatter_4/](https://www.sdcard.org/downloads/formatter_4/)

c. Download Win32DiskImager di sini : [https://Sourceforge.Net/Projects/Win32diskimager/](https://Sourceforge.Net/Projects/Win32diskimager/)

1.  Buka situs remi dari pengembang Raspberry pi

![](img/1cd9e5c69aee0af079bab0fc40ae5770.png)

situs resmi Raspberry Pi

2\. Klik logo yang bertuliskan OS Raspbian dan setelah itu klik Download ZIP pada Raspbian Stretch Witch Desktop seperti pada gambar berikut ini

![](img/83a2121cb933649585b6bcf3fa7182a1.png)

3\. Klik link installation guide yang terletak pada akhir dari kalimat pertama seperti gambar berikut ini

![](img/5c21245527ac7976d9c933e6b4970392.png)

4\. Pada halaman installation guide kita klik link download etcher yang terletak di bagian kalimat di paling bawah halaman tersebut, seperti gambar berikut ini

![](img/207b750fca344de78fa09ca748440818.png)

Kemudian setelah masuk ke situs resminya etcher yaitu [http://www.etcher.io](http://www.etcher.io) kita download software etcher ini dimana nantinya digunakan untuk melakukan instalasi OS Raspbian di dalam SDcard.

![](img/dadd7198951d17651dd75765c2a7a389.png)

5\. kembali ke halaman installation guide dan kita klik link NOOBS yang terletak pada kalimat kedua seperti pada gambar berikut ini

![](img/d0c84b32199d0e0b5e6a55673452c309.png)

Masuk ke halaman NOOBS dan cari di halaman tersebut link download sdcard.org yang terletak pada kalimat HOW TO FORMAT AN SD CARD AS FAT seperti gambar berikut ini

![](img/a31085f0651565a9a7492f3c4c1996eb.png)

Masuk ke situs resmi dari SD Association yaitu [http://www.sdcard.org/downloads/formatter_4/](http://www.sdcard.org/downloads/formatter_4/) dan lakukan pendownloatan software seperti pada gambar berikut ini

![](img/3bc4519f6d37360c55ff19c2d4e6dc6a.png)

6\. Setelah semua software selesai didownload langkah selanjutnya adalah instalasi yang dimulai dengan memasukan SD card kedalam adapter, dan memasukannya kedalam port USB Komputer atau laptop kita seperti gambar berikut ini

![](img/5e6e90de9887aaa2af2dfece28ee0e27.png)

SD Card dan Adapter

7\. buka software SD Card Formatter yang sudah didownload, lakukan pengaturan yaitu untuk bagian Select Cardnya kita pilih lokasi SDcard kita, kemudian untuk bagian Formatting Option kita pilih Quick Format, lalu bagian Volume label kita biarkan saja tetap boot, setelah itu kita klik Format dan tunggu sampai proses pemformatan SDcard selesai.

![](img/68af408fec6e0d76d85c98f59fe3e1cf.png)

8\. Setelah SDcard selesai di format, masukan OS Raspbian kedalam SDcard dengan software Etcher dengan cara kita pilih SDcard kita sebagai pilihan utama bagian yang akan di masukan OS Raspbian, kemudian kita klik Select Image dan kita pilih OS Raspbian yang sudah kita download diawal tadi, setelah itu kita klik Flash dan tunggu prosesnya sampai selesai

![](img/cf9d2505b39344f245df06ee47c1ea72.png)

9\. Setelah semua proses instalasi OS kedalam SDcard selesai langkah selanjutnya masukan SDcard kedalam Slot SDcard yang terdapat didalam board Raspberry Pi lalu rangkai keseluruhan komponen seperti keyboard, mouse, monitor, dan juga power supply seperti pada gambar berikut ini

![](img/668a17fb6c115c08a99c3b33fe2a3cb2.png)

Rangkaian keseluruhan komponen

10\. Hidupkan CPU dan lihat OS Raspbian sudah muncul di layar monitor anda.

**Cara mengatur Proxy pada Raspberry Pi**

Setelah Raspberry Pi selesai diinstall serta dapat digunakan pada PC, selanjutnya adalah menyambungkan RasberryPi ke internet menggunakn proxy dengan cara mengetik kalimat beikut diterminal

pi@raspberrypi:~ $ sudo nano /etc/environment

Tambahkan perintah berikut ini :

```
export http_proxy="http://username:password@proxyipaddress:proxyport"export https_proxy="http://username:password@proxyipaddress:proxyport"export no_proxy="localhost, 127.0.0.1"
```

Untuk menkoneksikan RasberryPi ke internet tanpa koneksi yaitu dengan menghapus dulu proxy dan langsung mengkoneksikan ke WiFi sama seperti koneksi WiFi pada windows. Selanjutnya download beberapa aplikasi atau *package* menggunakana terminal dengan menulis “sudo apt-get install nama aplikasi”, contoh aplikasi yang harus didownload seperti GIMP, Python, gcc/C++, Paket, dan lain-lain. RasberryPi juga perlu diinstall WiringPi untuk mencompile aplikasi diluar seperti aplikasi untuk menyalakan led, dan lain-lain. Untuk mengcompile file .c menggunakan perintah pi@raspberrypi:~ $ gcc –o test test.c dan untuk menjalankannya menggunakan perintah pi@raspberrypi:~ $ ./test.