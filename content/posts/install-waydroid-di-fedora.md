---
title: "Install Waydroid di Fedora 34"
date: 2021-10-29T10:00:10+07:00
draft: false
tags : ["fedora", "waydroid", "tutorial"]
author : "MAIswantoro"
---
Waydroid - aplikasi yang bertujuan untuk menjalankan android apps di linux. Waydroid berjalan yang di wayland display server ini merupakan pengembangan dari anbox. Untuk menjalankan waydroid hal yang perlu diperhatikan adalah sebagai berikut :
1. Kernel yang support ashmem dan binder. Untuk kernel bawaan dari distro biasanya belum mengaktifkan module ini, di fedora, khususnya fedora 34, bisa melakukan compile ulang kernel seperti tutorial pada [web ini](https://yanqiyu.info/2021/10/25/waydroid-fedora/) atau dengan menggunakan [kernel xanmod](https://copr.fedorainfracloud.org/coprs/rmnscnce/kernel-xanmod/) di copr. 
2. Desktop Environtment yang berjalan pada Wayland.

Nah, untuk kali ini saya akan membuat sebuah catatan pengalaman saya dalam menginstall Waydroid di Fedora 34. Sesuai keterangan di atas, waydroid hanya bisa berjalan dengan kernel yang module ashmem dan binder nya aktif. Sehingga yang saya lakukan pertama kali adalah menginstall kernel xanmod.

Install kernel xanmod dengan command berikut :
{{<highlight bash>}}
$ sudo dnf copr enable rmnscnce/kernel-xanmod
$ sudo dnf install kernel-xanmod-edge 
{{</highlight>}}

Kernel xanmod akan mengakibatkan crash pada *lkmd* sehingga kita perlu menambahkan kernel parameter psi=1 dengan cara sebagai berikut :
{{<highlight bash>}}
$ sudo grubby --update-kernel=ALL --args="psi=1"
{{</highlight>}}

kemudian lakukan update grub dengan menjalankan command berikut jika kalian menggunakan UEFI mode
{{<highlight bash>}}
$ sudo grub2-mkconfig -o /etc/grub2-efi.cfg 
{{</highlight>}}
atau command berikut jika menggunakan Legacy BIOS
{{<highlight bash>}}
$ sudo grub2-mkconfig -o /etc/grub2.cfg 
{{</highlight>}}
restart dan pilih kernel xanmod pada grub.

Langkah kedua adalah melakukan installasi waydroid, tutorial dalam bahasa inggris dapat dibaca pada halaman [web berikut](https://yanqiyu.info/2021/10/25/waydroid-fedora)
Langkah pertama tambahakan repo copr waydroid dan install waydroid-nya
{{<highlight bash>}}
$ sudo dnf copr enable yanqiyu/waydroid
$ sudo dnf install waydroid
{{</highlight>}}

karena kita menggunakan kernel xanmod, kita harus mengubah isi dari /etc/gbinder.d/anbox.conf menjadi
{{<highlight bash>}}
[Protocol]
/dev/anbox-binder = aidl2
/dev/anbox-vndbinder = aidl2
/dev/anbox-hwbinder = hidl

[ServiceManager]
/dev/anbox-binder = aidl2
/dev/anbox-vndbinder = aidl2
/dev/anbox-hwbinder = hidl
{{</highlight>}}

selain melakukan edit file diatas, pada kernel xanmod juga perlu melakukan set SELINUX ke permissive, dengan cara sebagai berikut :

{{<highlight bash>}}
$ sudo setenforce 0
{{</highlight>}}

setelahnya tinggal lakukan sesuai dengan web [waydroid](https://waydro.id/#howto)
{{<highlight bash>}}
$ sudo systemctl start waydroid-container.service
$ sudo waydroid init 
$ waydroid show-full-ui
{{</highlight>}}

dan viola, waydroid siap digunakan.

**sebagai catatan : jika anda menginginkan menggunakan google playstore, pada saat init, gunakan command berikut :**
{{<highlight bash>}}
$ sudo waydroid init -s GAPPS
{{</highlight>}}

berikut beberapa gambar waydroid yang sudah terinstall
![Gambar 1](/img/ss1.png)
Halaman Awal Waydroid
![Gambar 2](/img/ss2.png)
Pengaturan pada waydroid
![Gambar 3](/img/ps.png)
Playstore di waydroid


Link Referensi :
1. Karoboniru's Blog - [Waydroid on Fedora](https://yanqiyu.info/2021/10/25/waydroid-fedora/)
2. rmnscnce copr - [Kernel Xanmod](https://copr.fedorainfracloud.org/coprs/rmnscnce/kernel-xanmod/)
3. [Waydroid](https://waydro.id)
4. [Group Telegram Waydroid](https://t.me/waydroid)
