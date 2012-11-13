_Testing_ adalah sebahagian daripada proses pembangunan perisian komputer. Setiap kali baris-baris kod ditulis, diubah atau dipadam, ia mesti melalui proses testing bagi memastikan program tersebut masih lagi berjalan sebagaimana yang diharapkan. Ini bermakna kita sama sekali tidak boleh mengelak daripada melakukan _testing_ terhadap program atau aplikasi yang kita bangunkan.

## Automated Testing
Testing biasanya dilakukan secara manual dengan pengaturcara cuba menjalankan aturcara atau aplikasi yang dibangunkan dan memerhatikan sama ada ia mengeluarkan output yang dikehendaki ataupun semua _features_ berfungsi sebagai mana diharapkan. Ambil dcontoh aturcara ringkas berikut:-

    function add(num1, num2) {
        return num1 + num2;
    }

    print add(1, 2) # akan paparkan 3
    print add(2, 2) # akan paparkan 4

Andaikan aturcara diatas ditulis menggunakan bahasa PHP dan disimpan dalam fail bernama `add.php`. Untuk menguji aturcara ini, pengaturcara akan `execute` fail tersebut dan memerhatikan sama ada ia memaparkan out yang diharapkan iaitu:-

    3
    4

Aturcara tersebut mungkin boleh dijalankan seperti berikut:-

    php add.php
    3
    4

Di atas kita dapati aturcara tersebut memaparkan output yang kita kehendaki. Sekarang kita andaikan berlaku sedikit kesilapan dalam kod tersebut. Katakan ia ditulis seperti berikut:-

    function add(num1, num2) {
        return num1 * num2;
    }

    print add(1, 2) # akan paparkan 3
    print add(2, 2) # akan paparkan 4

Apabila dijalankan, kita akan dapati outputnya berlainan:-

    2
    4

Ini bermakna program tersebut gagal dalam proses _testing_ kita.