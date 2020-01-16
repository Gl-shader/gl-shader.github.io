---
title:  "Hello World!"
date:   2020-03-26 17:22:33 +0800
categories: glsl
classes:
  - landing
header:
  teaser: https://upload.wikimedia.org/wikipedia/commons/thumb/9/99/Linux_kernel_and_OpenGL_video_games.svg/1024px-Linux_kernel_and_OpenGL_video_games.svg.png
---

## Hello World!

 Biasanya "Hello World!"  Contohnya adalah langkah pertama untuk belajar bahasa baru.  Ini adalah program satu-baris sederhana yang menampilkan pesan sambutan yang antusias dan menyatakan peluang di depan.

 Dalam teks rendering GPU-land adalah tugas yang terlalu rumit untuk langkah pertama, alih-alih kami akan memilih warna sambutan yang cerah untuk meneriakkan antusiasme kami!

 <div class = "codeAndCanvas" data = "#ifdef GL_ES
precision mediump float;
#endif

uniform float u_time;

void main() {
	gl_FragColor = vec4(1.0,0.0,1.0,1.0);
}
"> </div>

 Jika Anda membaca buku ini di browser, blok kode sebelumnya bersifat interaktif.  Itu berarti Anda dapat mengklik dan mengubah bagian mana pun dari kode yang ingin Anda jelajahi.  Perubahan akan segera diperbarui berkat arsitektur GPU yang mengkompilasi dan mengganti shader * dengan cepat *.  Cobalah dengan mengubah nilai pada baris 8.

 Meskipun baris-baris kode sederhana ini tidak terlihat banyak, kita dapat menyimpulkan pengetahuan substansial dari mereka:

 1. Bahasa Shader memiliki fungsi `main` tunggal yang mengembalikan warna di akhir.  Ini mirip dengan C.

 2. Warna piksel terakhir ditetapkan ke variabel global yang dilindungi undang-undang `gl_FragColor`.

 3. Bahasa C-flavored ini telah dibangun di * variabel * (seperti `gl_FragColor`), * fungsi * dan * jenis *.  Dalam hal ini kami baru saja diperkenalkan ke `vec4` yang merupakan singkatan dari vektor empat dimensi presisi titik mengambang.  Nanti kita akan melihat lebih banyak tipe seperti `vec3` dan` vec2` bersama dengan yang populer: `float`,` int` dan `bool`.

 4. Jika kita melihat lebih dekat ke tipe `vec4` kita dapat menyimpulkan bahwa empat argumen merespon saluran RED, GREEN, BLUE dan ALPHA.  Kita juga dapat melihat bahwa nilai-nilai ini * dinormalisasi *, yang berarti mereka beralih dari `0,0` ke` 1,0`.  Kemudian, kita akan belajar bagaimana nilai-nilai normalisasi membuatnya lebih mudah untuk * memetakan nilai-nilai antar variabel.

 5. Fitur * C * penting lainnya yang dapat kita lihat dalam contoh ini adalah keberadaan macro preprocessor.  Macro adalah bagian dari langkah pra-kompilasi.  Dengan mereka dimungkinkan untuk `# define` variabel global dan melakukan beberapa operasi kondisional dasar (dengan` # ifdef` dan `# endif`).  Semua perintah makro dimulai dengan tagar (`#`).  Pra-kompilasi terjadi tepat sebelum kompilasi dan menyalin semua panggilan ke `# mendefinisikan` dan memeriksa` # ifdef` (didefinisikan) dan kondisi `# ifndef` (tidak ditentukan).  Di dunia "halo kami!"  contoh di atas, kami hanya menyisipkan baris 2 jika `GL_ES` didefinisikan, yang sebagian besar terjadi ketika kode dikompilasi pada perangkat seluler dan browser.

 6. Jenis float sangat penting dalam shader, sehingga tingkat * presisi * sangat penting.  Presisi yang lebih rendah berarti rendering yang lebih cepat, tetapi dengan biaya kualitas.  Anda bisa pilih-pilih dan tentukan presisi setiap variabel yang menggunakan floating point.  Pada baris pertama (`float mediump presisi;`) kami mengatur semua float ke presisi sedang.  Tetapi kita dapat memilih untuk mengaturnya menjadi rendah (`float presisi lowp;`) atau tinggi (`float presisi tinggi;`).

 7. Detail terakhir, dan mungkin yang paling penting, adalah bahwa spesifikasi GLSL tidak menjamin bahwa variabel akan dicor secara otomatis.  Apa artinya?  Pabrikan memiliki pendekatan berbeda untuk mempercepat proses kartu grafis tetapi mereka dipaksa untuk menjamin spesifikasi minimum.  Pengecoran otomatis bukan salah satunya.  Dalam contoh "hello world!" Kami, `vec4` memiliki presisi titik apung dan untuk itu ia diharapkan akan ditetapkan dengan` floats`.  Jika Anda ingin membuat kode konsisten yang baik dan tidak menghabiskan berjam-jam men-debug layar putih, biasakan menempatkan titik (`.`) di float Anda.  Kode semacam ini tidak akan selalu berfungsi:

 `` `glsl
 membatalkan main () {
     gl_FragColor = vec4 (1,0,0,1);  // GALAT
 }
 `` `

 Sekarang kita telah menggambarkan elemen yang paling relevan dari "hello world!"  program, saatnya untuk mengklik pada blok kode dan mulai menantang semua yang telah kita pelajari.  Anda akan mencatat bahwa pada kesalahan, program akan gagal dikompilasi, menampilkan layar putih.  Ada beberapa hal menarik untuk dicoba, misalnya:

 * Cobalah mengganti pelampung dengan bilangan bulat, kartu grafis Anda mungkin atau mungkin tidak mentolerir perilaku ini.

 * Cobalah mengomentari baris 8 dan tidak menetapkan nilai piksel apa pun ke fungsi tersebut.

 * Cobalah membuat fungsi terpisah yang mengembalikan warna tertentu dan menggunakannya di dalam `main ()`.  Sebagai petunjuk, berikut adalah kode untuk fungsi yang mengembalikan warna merah:

 `` `glsl
 vec4 red () {
     return vec4 (1.0.0.0.0.0.1.0);
 }
 `` `

 * Ada beberapa cara untuk membangun tipe `vec4`, coba temukan cara lain.  Berikut ini adalah salah satunya:

 `` `glsl
 vec4 color = vec4 (vec3 (1.0.0.0.1.0), 1.0);
 `` `

 Meskipun contoh ini tidak terlalu menarik, ini adalah contoh paling mendasar - kami mengubah semua piksel di dalam kanvas ke warna yang sama persis.  Dalam bab berikut kita akan melihat bagaimana mengubah warna piksel dengan menggunakan dua jenis input: spasi (tempat piksel pada layar) dan waktu (jumlah detik sejak halaman dimuat).
