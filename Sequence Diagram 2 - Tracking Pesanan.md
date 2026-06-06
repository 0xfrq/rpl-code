@startuml

actor Customer
participant "Mobile Application" as App
participant "Tracking Service" as Tracking
database "Database" as DB

== Membuka Halaman Tracking ==

Customer -> App : Membuka aplikasi Akselera
App --> Customer : Menampilkan halaman utama

Customer -> App : Membuka menu riwayat pesanan
App -> Tracking : Mengirim permintaan daftar pesanan Customer
Tracking -> DB : Mengambil data seluruh pesanan Customer
DB --> Tracking : Mengembalikan daftar pesanan
Tracking --> App : Mengirimkan daftar pesanan
App --> Customer : Menampilkan daftar riwayat pesanan

== Melihat Detail Status Pesanan ==

Customer -> App : Memilih pesanan yang ingin dilacak
App -> Tracking : Mengirim permintaan status pesanan
Tracking -> DB : Mengambil data status pesanan terkini
DB --> Tracking : Mengembalikan data status pesanan

alt Data status tersedia
    Tracking -> DB : Mengambil histori perubahan status pesanan
    DB --> Tracking : Mengembalikan histori status pesanan
    Tracking --> App : Mengirimkan data status dan histori pesanan
    App --> Customer : Menampilkan perkembangan pesanan secara real-time

    == Pembaruan Status Otomatis ==

    Tracking -> DB : Memeriksa apakah ada pembaruan status terbaru
    DB --> Tracking : Mengembalikan status terbaru

    alt Status pesanan berubah
        Tracking --> App : Mengirim pembaruan status pesanan
        App --> Customer : Menampilkan notifikasi perubahan status pesanan
        App --> Customer : Memperbarui tampilan tracking secara real-time
    else Status tidak berubah
        Tracking --> App : Mengembalikan status tidak ada perubahan
        App --> Customer : Menampilkan status pesanan tetap terkini
    end

else Data tidak ditemukan
    Tracking --> App : Mengembalikan pesan data tidak ditemukan
    App --> Customer : Menampilkan pesan pesanan tidak ditemukan
end

@enduml
