# Sejarah Ringkas Container \(termasuk Docker\) daripada tahun 1970

1979 - Unix V7 menyertakan sekali chroot system call, membolehkan directory root sesuatu proses diubah ke lokasi lain dalam filesystem. Ini adalah permulaan kepada isolation dalam proses OS - dalam kes ini capaian kepada filesystem bagi setiap proses telah diasingkan \(isolate\). Fahami betul-betul, pada peringkat ini, hanya capaian kepada filesytem yang diasingkan, jadi kalau struktur directory sistem tu adalah seperti berikut:-

```text
/
/var/
/home/
/home/kamal
    /
    /usr
    /var
    /tmp
/usr
/lib
/tmp
```

Dan proses B ditetapkan chroot ke /home/kamal, maknanya dia akan nampak /, /usr, /var, /tmp dalam /home/kamal dan bukan yang diluar direktori tersebut. Bagaimanapun seperti dinyatakan sebelum ini, hanya capaian kepada file system diasingkan, manakala sumber2 sistem yang lain seperti memori, cpu, networking masih lagi mendapat capaian penuh. Kalau sistem tu ada 8 GB RAM, itulah yang proses B nampak dan boleh guna juga.

1982 - Chroot system call dimasukkan ke dalam BSD.

2000 - FreeBSD Jail diperkenalkan. Jail dalam FreeBSD ni amat menyerupai virtual machine \(vm\) yang kita ada hari ini tapi mempunyai "keringanan" sama seperti docker/container. Setiap Jail boleh mempunyai alamat IP tersendiri dan ditetapkan jumlah RAM yang boleh dicapainya.

2001 - Linux VServer diperkenalkan. Sama macam Jail tapi untuk sistem Linux. Bagaimanapun ia memerlukan patching kepada kernel linux.

2004 - Solaris Container atau Zones diperkenalkan. Hampir sama dengan Jail tapi ia turut memperkenalkan teknologi unik seperti snapshot dan ZFS \(cerita pasal ni lain kali\). Joyent antara peneraju utama teknologi, ia umpama Amazon AWS walaupun mungkin agak terkedepan dan bukan masanya lagi.

2005 - OpenVZ \(Virtuozzo\) muncul dan menjadi agak popular. Banyak VPS hosting pada masa ini menawarkan VPS yang disediakan menggunakan openvz. OpenVZ ni bukan betul2 container tapi lebih kepada virtual machine tapi bukan betul2 vm jugak. Ia bersaing dengan teknologi para-virtualization seperti Xen. Pada masa ini kalau nak pilih vps yang lebih stabil kena pilih yang guna Xen dan bukannya OpenVZ. Yang menggunakan OpenVZ bagaimanapun biasanya lebih murah. Slicehost antara startup yang memanfaatkan teknologi Xen, ia umpama Digital Ocean pada masa sekarang.

2006 - Process Container diperkenalkan oleh Google. Ia membolehkan sumber-sumber utama komputer seperti memori, cpu, storan hd, rangkaian \(networking\) dihadkan dan diasingkan untuk setiap proses. Ia kemudian ditukar nama kepada Control Groups \(cgroups\) setahun kemudian dan seterusnya dimasukkan ke dalam kernel linux versi 2.6.24. Cgroups ini akan menjadi antara asas utama kepada docker/container.

Process Container dimulakan oleh Paul Menage \(jurutera Google\) tapi ditambah-baik dan dibina semula oleh jurutera-jurutera di Redhat di bawah pimpinan Tejun Heo. Kernel namespace, satu lagi blok asas container turut dibina oleh jurutera di Redhat pada masa ini. Implementasi namespace ini dianggap antara yang paling kompleks dalam pembangunan kernel linux.

2008 - LXC \(Linux Containers\) adalah implementasi pertama pengurus container \(container manager\) yang betul2 hampir lengkap. Ia dibina menggunakan cgroups dan namespace dan tidak memerlukan sebarang patches kepada kernel linux.

2011 - Warden diperkenalkan oleh Cloud Foundry. Ia menggunakan LXC pada peringkat awal tapi menukarnya kepada implementasi sendiri kemudiannya. Warden boleh mengasingkan sumber utama komputer, dijalankan sebagai daemon dan menyediakan API untuk pengurusan container. Ia juga menggunakan konsep client-server untuk menguruskan sekumpulan container dalam lebih daripada satu host. Sekali imbas, ia saling tak tumpah dengan docker.

2013 - Docker muncul. Sebagaimana Warden, Docker juga menggunakan LXC pada peringkat awal tapi menukarnya kepada implementasi sendiri kemudian.

2016 - Kepentingan keselamatan container mula disedari ramai. Usaha untuk memperkukuhkan keselamatan container mula giat dijalan oleh pelbagai pihak.

2017 - Tools berkaitan container mula matang. Pelbagai bentuk tools dibangunkan untuk membantu pengurusan container. Kubernetes, yang diperkenalkan 2 tahun sebelum ini menjadi semakin popular. Docker turut menyerahkan projek containerd mereka kepada Cloud Native Computing Foundation \(CNCF\). CNCF juga mengambil rkt sebagai standard container runtime.

2018 dan seterusnya menyaksikan perkembangan berterusan container dan juga docker bukan lagi dipentas utama dengan teknologi alternatif yang mahu mengambil-alih tempatnya seperti podman dan cri-o.

Daripada cerita di atas kita sebenarnya dapat melihat bagaimana keinginan manusia untuk mengasingkan pemprosesan dalam komputer diselesaikan melalui 2 cara iaitu virtualization dan containerization. Virtualization mencapai tahap pengasingan maksimum tapi ia memerlukan kita menjalankan sebuah komputer dalam komputer yang lain. Containerization pula hanya mengekang proses dalam komputer yang sama, dengan mengehad capaian proses tersebut kepada sumber-sumber utama komputer seperti cpu, memori, storan io dan rangkaian.

Lagi satu, dalam tujuan untuk mengasingkan proses, tidak semestinya hanya container yang boleh digunakan. Chroot, teknologi yang dibangunkan 5 dekad yang lampau masih lagi digunakan sekarang dalam pelbagai situasi, jadi boleh tengok juga sama ada anda sememangnya memerlukan container atau chroot sudah memadai.

