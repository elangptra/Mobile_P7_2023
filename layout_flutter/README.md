<style>
    body {text-align: justify}
</style>

# <b>Laporan Pertemuan 7 - Layout dan Navigasi</b>
<b> Nama: Elang Putra Adam

Kelas: TI 3G

NIM: 2141720074 </b>

## <b>Praktikum 1: Membangun Layout di Flutter</b>

Selesaikan langkah-langkah praktikum berikut ini menggunakan editor Visual Studio Code (VS Code) atau Android Studio atau code editor lain kesukaan Anda.

### <b>Langkah 1: Buat Project Baru</b>

Buatlah sebuah project flutter baru dengan nama layout_flutter. Atau sesuaikan style laporan praktikum yang Anda buat.

<b>Jawab:</b>

![Alt text](images/image.png)

### <b>Langkah 2: Buka file lib/main.dart</b>

Buka file main.dart lalu ganti dengan kode berikut. Isi nama dan NIM Anda di text title.

    import 'package:flutter/material.dart';

    void main() => runApp(const MyApp());

    class MyApp extends StatelessWidget {
        const MyApp({super.key});

        @override
        Widget build(BuildContext context) {
            return MaterialApp(
            title: 'Flutter layout: Nama dan NIM Anda',
            home: Scaffold(
                appBar: AppBar(
                title: const Text('Flutter layout demo'),
                ),
                body: const Center(
                child: Text('Hello World'),
                ),
            ),
            );
        }
    }

<b>Jawab:</b>

![Alt text](images/image-1.png)

![Alt text](images/image-2.png)

### <b>Langkah 3: Identifikasi layout diagram</b>

Langkah pertama adalah memecah tata letak menjadi elemen dasarnya:

* Identifikasi baris dan kolom.
* Apakah tata letaknya menyertakan kisi-kisi (grid)?
* Apakah ada elemen yang tumpang tindih?
* Apakah UI memerlukan tab?
* Perhatikan area yang memerlukan alignment, padding, atau borders.

Pertama, identifikasi elemen yang lebih besar. Dalam contoh ini, empat elemen disusun menjadi sebuah kolom: sebuah gambar, dua baris, dan satu blok teks.

![Alt text](images/image-3.png)

Selanjutnya, buat diagram setiap baris. Baris pertama, disebut bagian Judul, memiliki 3 anak: kolom teks, ikon bintang, dan angka. Anak pertamanya, kolom, berisi 2 baris teks. Kolom pertama itu memakan banyak ruang, sehingga harus dibungkus dengan widget yang Diperluas.

![Alt text](images/image-4.png)

Baris kedua, disebut bagian Tombol, juga memiliki 3 anak: setiap anak merupakan kolom yang berisi ikon dan teks.

![Alt text](images/image-5.png)

Setelah tata letak telah dibuat diagramnya, cara termudah adalah dengan menerapkan pendekatan bottom-up. Untuk meminimalkan kebingungan visual dari kode tata letak yang banyak bertumpuk, tempatkan beberapa implementasi dalam variabel dan fungsi.

### <b>Langkah 4: Implementasi title row</b>

Pertama, Anda akan membuat kolom bagian kiri pada judul. Tambahkan kode berikut di bagian atas metode build() di dalam kelas MyApp:

    Widget titleSection = Container(
        padding: const EdgeInsets.all(...),
        child: Row(
            children: [
            Expanded(
                /* soal 1*/
                child: Column(
                crossAxisAlignment: ...,
                children: [
                    /* soal 2*/
                    Container(
                    padding: const EdgeInsets.only(bottom: ...),
                    child: const Text(
                        'Wisata Gunung di Batu',
                        style: TextStyle(
                        fontWeight: FontWeight.bold,
                        ),
                    ),
                    ),
                    Text(
                    'Batu, Malang, Indonesia',
                    style: TextStyle(...),
                    ),
                ],
                ),
            ),
            /* soal 3*/
            Icon(
            ...,
                color: ...,
            ),
            const Text(...),
            ],
        ),
    );

/* soal 1 */ Letakkan widget Column di dalam widget Expanded agar menyesuaikan ruang yang tersisa di dalam widget Row. Tambahkan properti crossAxisAlignment ke CrossAxisAlignment.start sehingga posisi kolom berada di awal baris.

/* soal 2 */ Letakkan baris pertama teks di dalam Container sehingga memungkinkan Anda untuk menambahkan padding = 8. Teks ‘Batu, Malang, Indonesia' di dalam Column, set warna menjadi abu-abu.

/* soal 3 */ Dua item terakhir di baris judul adalah ikon bintang, set dengan warna merah, dan teks "41". Seluruh baris ada di dalam Container dan beri padding di sepanjang setiap tepinya sebesar 32 piksel. Kemudian ganti isi body text ‘Hello World' dengan variabel titleSection seperti berikut:

![Alt text](images/image-6.png)

<b>Jawab:</b>

a. Soal 1

    /* soal 1*/
        child: Column(
            crossAxisAlignment: CrossAxisAlignment.start,

![Alt text](images/image-7.png)

b. Soal 2

    /* soal 2*/
    Container(
        padding: const EdgeInsets.only(bottom: 8.0),
        child: const Text(
            'Wisata Gunung di Batu',
            style: TextStyle(
                fontWeight: FontWeight.bold,
            ),
        ),
    ),
    Text(
        'Batu, Malang, Indonesia',
        style: TextStyle(color: Colors.grey[600]),
    ),

![Alt text](images/image-8.png)

c. Soal 3

    /* soal 3*/
        Icon(
            Icons.star,
            color: Colors.red,
        ),
        const Text('41'),

![Alt text](images/image-9.png)

<b>Hasil running:</b>

![Alt text](images/image-10.png)

## <b>Praktikum 2: Implementasi button row</b>

Selesaikan langkah-langkah praktikum berikut ini dengan melanjutkan dari praktikum sebelumnya.

### <b>Langkah 1: Buat method Column _buildButtonColumn</b>

Bagian tombol berisi 3 kolom yang menggunakan tata letak yang sama—sebuah ikon di atas baris teks. Kolom pada baris ini diberi jarak yang sama, dan teks serta ikon diberi warna primer.

Karena kode untuk membangun setiap kolom hampir sama, buatlah metode pembantu pribadi bernama buildButtonColumn(), yang mempunyai parameter warna, Icon dan Text, sehingga dapat mengembalikan kolom dengan widgetnya sesuai dengan warna tertentu.

<b>lib/main.dart (_buildButtonColumn)</b>

    class MyApp extends StatelessWidget {
        const MyApp({super.key});

        @override
        Widget build(BuildContext context) {
            // ···
        }

        Column _buildButtonColumn(Color color, IconData icon, String label) {
            return Column(
            mainAxisSize: MainAxisSize.min,
            mainAxisAlignment: MainAxisAlignment.center,
            children: [
                Icon(icon, color: color),
                Container(
                margin: const EdgeInsets.only(top: 8),
                child: Text(
                    label,
                    style: TextStyle(
                    fontSize: 12,
                    fontWeight: FontWeight.w400,
                    color: color,
                    ),
                ),
                ),
            ],
            );
        }
    }

<b>Jawab:</b>

![Alt text](images/image-11.png)

### <b>Langkah 2: Buat widget buttonSection</b>

Buat Fungsi untuk menambahkan ikon langsung ke kolom. Teks berada di dalam Container dengan margin hanya di bagian atas, yang memisahkan teks dari ikon.

Bangun baris yang berisi kolom-kolom ini dengan memanggil fungsi dan set warna, Icon, dan teks khusus melalui parameter ke kolom tersebut. Sejajarkan kolom di sepanjang sumbu utama menggunakan MainAxisAlignment.spaceEvenly untuk mengatur ruang kosong secara merata sebelum, di antara, dan setelah setiap kolom. Tambahkan kode berikut tepat di bawah deklarasi titleSection di dalam metode build():

<b>lib/main.dart (buttonSection)</b>

    Color color = Theme.of(context).primaryColor;

    Widget buttonSection = Row(
    mainAxisAlignment: MainAxisAlignment.spaceEvenly,
    children: [
        _buildButtonColumn(color, Icons.call, 'CALL'),
        _buildButtonColumn(color, Icons.near_me, 'ROUTE'),
        _buildButtonColumn(color, Icons.share, 'SHARE'),
    ],
    );

<b>Jawab:</b>

![Alt text](images/image-12.png)

### <b>Langkah 3: Tambah button section ke body</b>

Tambahkan variabel buttonSection ke dalam body seperti berikut:

![Alt text](images/image-13.png)

<b>Jawab:</b>

![Alt text](images/image-14.png)

<b>Hasil running:</b>

![Alt text](images/image-15.png)

## <b>Praktikum 3: Implementasi text section</b>

Selesaikan langkah-langkah praktikum berikut ini dengan melanjutkan dari praktikum sebelumnya.

### <b>Langkah 1: Buat widget textSection</b>

Tentukan bagian teks sebagai variabel. Masukkan teks ke dalam Container dan tambahkan padding di sepanjang setiap tepinya. Tambahkan kode berikut tepat di bawah deklarasi buttonSection:

    Widget textSection = Container(
        padding: const EdgeInsets.all(32),
        child: const Text(
            'Carilah teks di internet yang sesuai '
            'dengan foto atau tempat wisata yang ingin '
            'Anda tampilkan. '
            'Tambahkan nama dan NIM Anda sebagai '
            'identitas hasil pekerjaan Anda. '
            'Selamat mengerjakan 🙂.',
            softWrap: true,
        ),
    );

Dengan memberi nilai softWrap = true, baris teks akan memenuhi lebar kolom sebelum membungkusnya pada batas kata.

<b>Jawab:</b>

![Alt text](images/image-16.png)

<b>Langkah 2: Tambahkan variabel text section ke body</b>

Tambahkan widget variabel textSection ke dalam body seperti berikut:

![Alt text](images/image-17.png)

<b>Jawab:</b>

![Alt text](images/image-18.png)

<b>Hasil running:</b>

![Alt text](images/image-19.png)