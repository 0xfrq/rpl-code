@startuml

|Seller|
start
:Membuka aplikasi Akselera;

|Sistem|
:Menampilkan halaman utama;

|Seller|
:Membuka menu inventaris;

|Sistem|
:Menampilkan daftar produk dan stok terkini;

|Seller|
:Memilih aksi pengelolaan stok
(tambah / ubah / kurangi);

if (Aksi yang dipilih?) then (tambah produk baru)
    |Seller|
    :Mengisi informasi produk baru
    (nama, harga, stok, kategori);

    |Sistem|
    :Memvalidasi data produk;

    if (Data valid?) then (ya)
        :Menyimpan produk baru ke database;
        :Menampilkan produk baru pada daftar inventaris;
    else (tidak)
        :Menampilkan pesan error validasi;

        |Seller|
        :Memperbaiki data produk;
    endif

else (ubah stok produk)
    |Seller|
    :Memilih produk yang akan diubah stoknya;
    :Memasukkan jumlah perubahan stok;

    |Sistem|
    :Memvalidasi data perubahan stok;

    if (Data valid?) then (ya)
        :Memperbarui jumlah stok produk;
        :Menyimpan riwayat perubahan stok;

        if (Stok mencapai batas minimum?) then (ya)
            :Mengirim notifikasi stok menipis ke Seller;

            |Seller|
            :Menerima notifikasi stok menipis;
        else (tidak)
        endif

        |Sistem|
        :Menampilkan stok terbaru pada inventaris;
    else (tidak)
        :Menampilkan pesan error;

        |Seller|
        :Memperbaiki data perubahan stok;
    endif
endif

|Seller|
:Melihat riwayat pergerakan stok;

|Sistem|
:Menampilkan histori keluar masuk stok;

stop

@enduml
