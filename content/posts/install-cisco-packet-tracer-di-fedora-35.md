---
title: "Install Cisco Packet Tracer Di Fedora 35"
date: 2021-11-29T10:21:07+07:00
draft: false
tags : ["fedora", "cisco packet tracer", "tutorial"]
author : "MAIswantoro"
---
Cisco Packet Tracer, adalah sebuah software yang sering dipakai oleh mahasiswa ataupun pelajar untuk mempelajari jaringan. Software ini digunakan untuk simulasi topologi jaringan. Terdapat banyak macam router, switch dan perangkat jaringan cisco lainnya yang bisa digunakan untuk simulasi. 
Cisco Packet Tracer tersedia gratis di web netacad.com, hanya dengan mendaftar dan mengikuti kelas packet tracer. Software ini mendukut banyak sistem operasi, tidak hanya windows, tapi juga MacOS dan Linux Ubuntu.
Dalam tulisan ini saya akan mencatat langkah-langkah yang saya kerjakan saat menginstall aplikasi ini pada sistem operasi Fedora 35.
Hal-hal yang perlu diperhatikan adalah untuk installer software nya, bisa di download pada web netacad, yang kita download adalah installer versi .deb punya ubuntu.
Langkah-langkanya adalah sebagai berikut :
1. Buka folder yang berisi file yang sudah kita download.
2. Klik kanan pada foldernya, kemudian open folder in terminal
3. Buat folder baru dengan nama pt80
{{<highlight bash>}}
$ mkdir pt80
{{</highlight>}} 
4. copy file .deb ke folder pt80
{{<highlight bash>}}
$ cp CiscoPacketTracer_801_Ubuntu_64bit.deb pt80/
{{</highlight>}} 
5. pindah ke foder pt80 tadi dengan cd pt80
6. extract file .deb tadi dengan command ar
{{<highlight bash>}}
$ ar -xv CiscoPacketTracer_801_Ubuntu_64bit.deb
{{</highlight>}} 
7. buat folder data dan control
{{<highlight bash>}}
$ mkdir data
$ mkdir control
{{</highlight>}}
8. extract data.tar.xz dan control.tar.xz ke folder yang sudah kita buat
{{<highlight bash>}}
$ tar -C control -Jxf control.tar.xz
$ tar -C data -Jxf data.tar.xz
{{</highlight>}}
9. pindahkan file yang ada di folder data ke /usr dan /opt
{{<highlight bash>}}
$ cd data
$ sudo cp -r usr /
$ sudo cp -r opt /
{{</highlight>}}
Selesai. aplikasi sudah terinstall. akan tetapi kita perlu membuat shortcut untuk memudahkan kita membuka aplikasinya.
Untuk fedora 35, masih ada bugs dimana ketika saat membuka pertama kali akan menemui blank screen berwarna putih di halaman login, untuk itu kita harus melakukan modifikasi file .desktop nya
disini saya menggunakan gedit untuk melakukan modifikasi filenya.
{{<highlight bash>}}
$ sudo gedit /usr/share/applications/cisco-pt.desktop
{{</highlight>}}
kemudian tambahkan --no-sandbox args pada baris Exec=...
sehingga menjadi seperti ini
{{<highlight bash>}}
Exec=/opt/pt/packettracer --no-sandbox args %f
{{</highlight>}}
kemudian lakukan beberapa command berikut untuk install shortcutnya.
{{<highlight bash>}}
sudo xdg-desktop-menu install /usr/share/applications/cisco-pt.desktop
sudo xdg-desktop-menu install /usr/share/applications/cisco-ptsa.desktop
sudo update-mime-database /usr/share/mime
sudo gtk-update-icon-cache --force --ignore-theme-index /usr/share/icons/gnome
sudo xdg-mime default cisco-ptsa.desktop x-scheme-handler/pttp
sudo ln -sf /opt/pt/PacketTracer /usr/local/bin/PacketTracer
{{</highlight>}}

Selesai, dan sekarang Packet Tracer sudah bisa digunakan.
![Gambar 1](/img/packettracer.png)
Gambar Packet Tracer


Link Referensi :
1. https://gist.github.com/siliconjesus/c3590b8d4fdb6ebea57bb1ccd66c8434
2. https://netacad.com