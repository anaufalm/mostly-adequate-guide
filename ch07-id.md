# Bab 07: Saya dan Hindley-Milner

## Apa Tipe Anda?
Jika Anda baru mengenal dunia fungsional, itu tidak akan selama Anda menemukan tipe tanda tangan yang pas. Jenis adalah bahasa meta yang memungkinkan orang dari semua latar belakang yang berbeda untuk berkomunikasi secara ringkas dan efektif. Sebagian besar, mereka ditulis dengan sistem yang disebut "Hindley-Milner", yang akan kita periksa bersama di bab ini.

Saat bekerja dengan fungsi pure, tipe tanda tangan memiliki kekuatan ekspresif yang bahasa Inggrisnya tidak dapat menampung lilin. Tanda tangan ini membisikkan rahasia rahasia sebuah fungsi di telinga Anda. Secara tunggal, garis kompak, mereka mengekspos perilaku dan niat. Kita bisa mendapatkan "teorema bebas" dari mereka. Tipe ini dapat disimpulkan sehingga tidak perlu anotasi tipe eksplisit. Mereka dapat atur ke titik presisi atau kiri secara umum dan abstrak. Mereka tidak hanya berguna untuk pemeriksaan waktu kompilasi, namun juga menjadi dokumentasi terbaik yang tersedia. Tipe tanda tangan memainkan peran penting dalam pemrograman fungsional - jauh lebih banyak dari yang Anda harapkan.

JavaScript adalah bahasa yang dinamis, tapi bukan berarti kita menghindari semua tipe bersama. Kami masih bekerja dengan string, angka, boolean, dan sebagainya. Hanya saja tidak ada integrasi tingkat bahasa sehingga kita menyimpan informasi ini di kepala kita. Tidak perlu khawatir, karena kita menggunakan tanda tangan untuk dokumentasi, kita bisa menggunakan komentar untuk melayani tujuan kita.

Ada jenis alat pemeriksaan tipe yang tersedia untuk JavaScript seperti [Flow](http://flowtype.org/) atau logat yang diketik, [TypeScript](http://www.typescriptlang.org/). Tujuan dari buku ini adalah untuk melengkapi satu alat untuk menulis kode fungsional sehingga kita akan tetap menggunakan sistem tipe standar yang digunakan di seluruh bahasa FP.


## Kisah Cryptic

Dari halaman-halaman buku matematika yang berdebu, yang melintasi lautan kertas putih yang luas, di antara tulisan-tulisan blog Sabtu pagi yang santai, masuk ke dalam kode sumber itu sendiri, kami menemukan tipe tanda tangan Hindley-Milner. Sistemnya cukup sederhana, namun menjamin penjelasan singkat dan beberapa praktik untuk menyerap bahasa kecil itu sepenuhnya.


```js
// capitalize :: String -> String
const capitalize = s => toUpperCase(head(s)) + toLowerCase(tail(s));

capitalize('smurf'); // 'Smurf'
```

Di sini, `capitalize` mengambil` String` dan mengembalikan sebuah `String`. Lupakan implementasinya, itu adalah jenis tanda tangan yang kita minati.

Di HM, fungsi ditulis sebagai `a -> b` di mana `a` dan `b` adalah variabel untuk tipe apa saja. Jadi tanda tangan untuk `capitalize` dapat dibaca sebagai "fungsi dari `String` ke` String`". Dengan kata lain, dibutuhkan sebuah `String` sebagai input dan mengembalikan sebuah `String` sebagai outputnya.

Mari kita lihat beberapa tanda tangan fungsi lainnya:

```js
// strLength :: String -> Number
const strLength = s => s.length;

// join :: String -> [String] -> String
const join = curry((what, xs) => xs.join(what));

// match :: Regex -> String -> [String]
const match = curry((reg, s) => s.match(reg));

// replace :: Regex -> String -> String -> String
const replace = curry((reg, sub, s) => s.replace(reg, sub));
```

`strLength` adalah ide yang sama seperti sebelumnya: kita mengambil sebuah `String` dan mengembalikan sebuah `Number`.

Yang lain sekilas mungkin akan membingungkan Anda. Tanpa memahami detail sepenuhnya, Anda selalu bisa melihat tipe terakhir sebagai nilai pengembalian. Jadi untuk `match` Anda bisa menafsirkannya sebagai: Dibutuhkan `Regex` dan `String` dan mengembalikan `[String]` Anda. Tapi hal yang menarik terjadi di sini bahwa saya ingin meluangkan waktu untuk menjelaskan apakah saya boleh.

Untuk `match` kita bebas mengelompokkan tanda tangan seperti:

```js
// match :: Regex -> (String -> [String])
const match = curry((reg, s) => s.match(reg));
```

Ah iya, mengelompokkan bagian terakhir dalam kurung mengungkapkan lebih banyak informasi. Sekarang terlihat sebagai fungsi yang mengambil `Regex` dan mengembalikan kita fungsi dari `String` ke `[String]`. Karena currying, ini akan terjadi: berikan `Regex` dan kita akan mendapatkan sebuah fungsi kembali menunggu argumen `String` nya. Tentu saja, kita tidak perlu memikirkannya seperti ini, tapi bagus untuk mengerti mengapa tipe terakhir adalah yang kembali.

```js
// match :: Regex -> (String -> [String])
// onHoliday :: String -> [String]
const onHoliday = match(/holiday/ig);
```

Setiap argumen muncul satu jenis di bagian depan tanda tangan. `onHoliday` adalah `match` yang sudah memiliki `Regex`.

```js
// replace :: Regex -> (String -> (String -> String))
const replace = curry((reg, sub, s) => s.replace(reg, sub));
```

Seperti yang bisa Anda lihat dengan tanda kurung penuh pada `replace`, notasi tambahan bisa menjadi sedikit bising dan berlebihan sehingga kami sudah menghilangkannya. Kita bisa memberikan semua argumen sekaligus jika kita memilihnya menjadi lebih mudah untuk menganggapnya sebagai: `replace` mengambil `Regex`, `String` dan `String` lain dan mengembalikan sebuah `String`.

Beberapa hal terakhir di sini:


```js
// id :: a -> a
const id = x => x;

// map :: (a -> b) -> [a] -> [b]
const map = curry((f, xs) => xs.map(f));
```

Fungsi `id` mengambil tipe lama `a` dan mengembalikan sesuatu dengan tipe yang sama dengan `a`. Kami dapat menggunakan variabel dalam tipe seperti kode. Nama variabel seperti `a` dan` b` adalah konvensi, namun bersifat sesuka hati dan dapat diganti dengan nama apa pun yang Anda inginkan. Jika mereka adalah variabel yang sama, mereka harus tipe yang sama. Itu aturan yang penting jadi mari kita tegaskan: `a -> b` bisa menjadi tipe `a` untuk tipe `b`, tapi `a -> a` berarti harus tipe yang sama. Misalnya, `id` mungkin `String -> String` atau `Number -> Number`, tapi bukan `String -> Bool`.

`map` juga menggunakan variabel tipe, tapi kali ini kami mengenalkan `b` yang mungkin atau mungkin bukan tipe yang sama dengan `a`. Kita bisa membacanya sebagai: `map` mengambil sebuah fungsi dari tipe `a` ke tipe yang sama atau berbeda `b`, lalu mengambil sebuah array dari `a` dan menghasilkan array dari `b`.

Mudah-mudahan, Anda sudah diatasi dengan keindahan ekspresif dalam tipe signature ini. Ini secara harfiah memberi tahu kita apakah fungsinya hampir kata demi kata. Ini diberi fungsi dari `a` ke` b`, sebuah array dari `a`, dan ini memberi kita sebuah array dari` b`. Satu-satunya hal yang masuk akal untuk dilakukan adalah memanggil fungsi berdarah pada setiap `a`. Ada lagi wajah yang berani.

Mampu memberikan balasan tentang tipe dan implikasinya adalah keterampilan yang akan membawa Anda jauh ke dalam dunia fungsional. Tidak hanya akan kertas, blog, dokumen, dll, menjadi lebih mudah dicerna, namun tanda tangan itu sendiri praktis akan mengajarkan Anda tentang fungsinya. Dibutuhkan latihan untuk menjadi pembaca yang fasih, tapi jika Anda tetap menggunakannya, banyak informasi akan tersedia untuk Anda sans RTFMing.

Ini beberapa contoh lagi hanya untuk melihat apakah Anda bisa mengartikannya sendiri.

```js
// head :: [a] -> a
const head = xs => xs[0];

// filter :: (a -> Bool) -> [a] -> [a]
const filter = curry((f, xs) => xs.filter(f));

// reduce :: (b -> a -> b) -> b -> [a] -> b
const reduce = curry((f, x, xs) => xs.reduce(f, x));
```

`reduce` mungkin, yang paling ekspresif dari semuanya. Ini akan rumit, bagaimanapun, jangan merasa canggung jika Anda harus berjuang melawannya. Bagi yang penasaran, saya akan coba jelaskan dalam bahasa Indonesia meski menggarap tanda tangan sendiri jauh lebih instruktif.

Ahem, di sini tidak ada apa-apa.... melihat tanda tangan, kita melihat argumen pertama adalah fungsi yang mengharapkan `b`, `a`, dan menghasilkan `b`. Darimana `a` dan `b` ini didapatkan? Nah, argumen berikut dalam tanda tangan adalah `b` dan sebuah array `a` sehingga kita hanya bisa berasumsi bahwa `b` dan masing-masing `a` akan menjadi umpan masuk. Kita juga melihat hasil dari fungsinya adalah `b` sehingga pemikiran di sini adalah mantra terakhir dari fungsi yang berlalu akan menjadi nilai output kita. Untuk mengetahui kekurangannya, kita dapat menyatakan bahwa penyelidikannya akurat.


## Mempersempit Kemungkinan

Ketika variabel tipe diperkenalkan, muncul properti penasaran yang disebut *[parametricity](https://en.wikipedia.org/wiki/Parametricity)*. Properti ini menyatakan bahwa suatu fungsi akan *bertindak pada semua jenis dengan cara yang seragam*. Mari selidiki:

```js
// head :: [a] -> a
```

Lihatlah `head`, kita melihat bahwa `[a]` dibutuhkan ke `a`. Selain tipe beton `array`, tidak ada informasi lain yang tersedia dan, oleh karena itu, fungsinya terbatas untuk mengerjakan array saja. Apa yang mungkin bisa dilakukan dengan variabel `a` jika tidak tahu apa-apa tentang hal itu? Dengan kata lain, `a` mengatakan bahwa itu tidak bisa menjadi tipe *spesifik*, yang berarti dapat berupa tipe *apapun*, yang membuat kita memiliki fungsi yang harus bekerja secara seragam untuk *setiap* jenis yang mungkin ada. Inilah yang dimaksud *parametrikitas*. Menebak implementasi, satu-satunya asumsi yang masuk akal adalah bahwa dibutuhkan unsur pertama, terakhir, atau acak dari array itu. Nama `head` harus memberi tip pada kita.

Berikut yang lainnya:

```js
// reverse :: [a] -> [a]
```

Dari jenis tanda tangan saja, apa yang bisa di `reverse` sehingga bisa sampai? Sekali lagi, itu tidak dapat melakukan sesuatu yang spesifik untuk `a`. Ini tidak dapat mengubah `a` ke tipe yang berbeda atau memperkenalkan `b`. Bisakah diurutkan? Nah, tidak, tidak akan ada cukup informasi untuk menyortir setiap jenis yang mungkin. Bisakah menata ulang? Ya, saya kira itu bisa dilakukan, tapi harus dengan cara yang sama persis dengan yang bisa diprediksi. Kemungkinan lain adalah memutuskan untuk menghapus atau menduplikasi elemen. Bagaimanapun, intinya adalah, kemungkinan tingkah laku tersebut secara besar-besaran menyempit oleh jenis polimorfiknya.

Penyempitan kemungkinan ini memungkinkan kita untuk menggunakan mesin pencari jenis tanda tangan seperti [Hoogle](https://www.haskell.org/hoogle) untuk menemukan fungsi yang kita cari. Informasi yang dikemas erat menjadi tanda tangan memang cukup kuat.

## Bebas seperti dalam Teorema

Selain mengurangi kemungkinan implementasi, penalaran semacam ini bisa menghasilkan *teorema bebas*. Berikut adalah beberapa teorema contoh acak yang diangkat langsung dari [kertas Wadler pada subjek](http://ttic.uchicago.edu/~dreyer/course/papers/wadler.pdf).

```js
// head :: [a] -> a
compose(f, head) === compose(head, map(f));

// filter :: (a -> Bool) -> [a] -> [a]
compose(map(f), filter(compose(p, f))) === compose(filter(p), map(f));
```


Anda tidak memerlukan kode untuk mendapatkan teorema ini, karena mereka mengikuti langsung dari tipenya. Yang pertama mengatakan bahwa jika kita mendapatkan `head` dari array kita, maka jalankan beberapa fungsi `f` di atasnya, itu akan setara dengan, dan kebetulan, jauh lebih cepat daripada, jika kita pertama kali `melihat (f)` pada setiap elemen lalu mengambil hasil `head`.

Anda mungkin berpikir, itu hanya akal sehat. Tapi terakhir saya cek, komputer tidak memiliki akal sehat. Memang, mereka harus memiliki cara formal untuk mengotomatisasi pengoptimalan kode semacam ini. Matematika memiliki cara untuk meresmikan intuitif, yang sangat membantu di tengah medan logika komputer yang kaku.

Teorema `filter` juga serupa. Dikatakan bahwa jika kita menulis `f` dan `p` untuk memeriksa mana yang harus disaring, maka itu benar-benar menerapkan `f` melalui `map` (ingat, filter tidak akan mengubah elemen - tanda tangannya memaksa bahwa `a` tidak akan disentuh), akan selalu sama dengan pemetaan `f` kami kemudian memfilter hasilnya dengan predikat `p`.

Ini hanya dua contoh, tapi Anda bisa menerapkan penalaran ini pada tanda jenis polimorfik dan itu akan selalu berlaku. Dalam JavaScript, ada beberapa alat yang tersedia untuk mendeklarasikan aturan penulisan ulang. Seseorang mungkin juga melakukan ini melalui fungsi `compose` itu sendiri. Buahnya gantung rendah dan kemungkinannya tak ada habisnya.

## Kendala

Satu hal yang perlu diperhatikan adalah kita bisa membatasi tipe ke antarmuka.

```js
// sort :: Ord a => [a] -> [a]
```

Apa yang kita lihat di sisi kiri panah lemak kita di sini adalah pernyataan sebuah fakta: `a` harus menjadi `Ord`. Atau dengan kata lain, `a` harus mengimplementasikan antarmuka `Ord`. Apa itu `Ord` dan dari mana asalnya? Dalam bahasa yang diketik itu akan menjadi antarmuka yang didefinisikan yang mengatakan bahwa kita dapat memesan nilai-nilainya. Ini tidak hanya memberi tahu lebih banyak tentang fungsi `a` dan fungsi `sort` kami, namun juga membatasi domain. Kami memanggil deklarasi antarmuka ini *tipe kendala*.

```js
// assertEqual :: (Eq a, Show a) => a -> a -> Assertion
```

Di sini, kita memiliki dua kendala: `Eq` dan `Show`. Mereka akan memastikan bahwa kita dapat memeriksa persamaan `a` kita dan mencetak perbedaannya jika tidak sama.

Kita akan melihat lebih banyak contoh kendala dan idenya harus lebih banyak terbentuk di bab selanjutnya.


## Kesimpulan

Tanda tangan Hindley-Milner ada di mana-mana di dunia fungsional. Meski mudah dibaca dan ditulis, butuh waktu untuk menguasai teknik memahami program melalui tanda tangan saja. Kami akan menambahkan tanda tangan jenis ke setiap baris kode dari sini.

[Bab 08: Tupperware](ch08.md)
