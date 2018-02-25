[![cover](images/cover.png)](SUMMARY.md)

## Tentang buku ini

This is a book on the functional paradigm in general. We'll use the world's most popular functional programming language: JavaScript. Some may feel this is a poor choice as it's against the grain of the current culture which, at the moment, feels predominately imperative. However, I believe it is the best way to learn FP for several reasons:
Ini adalah buku tentang paradigma fungsional pada umumnya. Kami akan menggunakan bahasa pemrograman fungsional terpopuler di dunia: JavaScript. Beberapa diantaranya mungkin merasa ini adalah pilihan yang buruk karena bertentangan dengan selera budaya saat ini, saat ini, terasa begitu penting. Namun, saya percaya ini adalah cara terbaik untuk belajar FP karena beberapa alasan berikut:

 * **You likely use it every day at work.**
 * **Anda mungkin menggunakannya setiap hari di tempat Anda bekerja.**

    This makes it possible to practice and apply your acquired knowledge each day on real world programs rather than pet projects on nights and weekends in an esoteric FP language.
    Hal ini memungkinkan untuk berlatih dan menerapkan pengetahuan yang Anda dapatkan setiap hari di program dunia nyata daripada project pet pada malam hari dan akhir pekan dalam bahasa esoteris FP.


 * **We don't have to learn everything up front to start writing programs.**
 * **Kita tidak mesti harus mempelajari semuanya secara terdepan untuk memulai menulis program.**

    In a pure functional language, you cannot log a variable or read a DOM node without using monads. Here we can cheat a little as we learn to purify our codebase. It's also easier to get started in this language since it's mixed paradigm and you can fall back on your current practices while there are gaps in your knowledge.
    Dalam bahasa fungsional murni, Anda tidak bisa me-_log_ variabel atau membaca _node_ DOM tanpa menggunakan _monads_. Di sini kita bisa sedikit mengecoh saat kita belajar memurnikan basis kode kita. Ini juga akan lebih mudah untuk memulai dalam bahasa ini karena ini adalah paradigma campuran dan Anda dapat mengikuti praktik Anda saat ini ketika ada kesenjangan dalam pengetahuan Anda.


 * **The language is fully capable of writing top notch functional code.**
 * **Bahasa memiliki kapasitas untuk menulis kode fungsional dengan hasil terbaik.**

    We have all the features we need to mimic a language like Scala or Haskell with the help of a tiny library or two. Object-oriented programming currently dominates the industry, but it's clearly awkward in JavaScript. It's akin to camping off of a highway or tap dancing in galoshes. We have to `bind` all over the place lest `this` change out from under us, we don't have classes (yet), we have various work arounds for the quirky behavior when the `new` keyword is forgotten, private members are only available via closures. To a lot of us, FP feels more natural anyways.
    
    Kami memiliki semua fitur yang kami butuhkan untuk meniru bahasa seperti Scala atau Haskell dengan bantuan satu atau dua perpustakaan kecil. Pemrograman yang berorientasi pada objek saat ini mendominasi perindustrian, Tetapi ini jelas canggung untuk JavaScript. Ini mirip dengan berkemah di jalan raya atau menari dengan sepatu _galoshes_. Kita harus `mengikat` di semua tempat agar `ini` tidak berubah dibawah pengawasan kita, kita (belum) memiliki kelas. Kita memiliki banyak pekerjaan
    
That said, typed functional languages will, without a doubt, be the best place to code in the style presented by this book. JavaScript will be our means of learning a paradigm, where you apply it is up to you. Luckily, the interfaces are mathematical and, as such, ubiquitous. You'll find yourself at home with swiftz, scalaz, haskell, purescript, and other mathematically inclined environments.


## Read it Online

For a best reading experience, [read it online via Gitbook](https://mostly-adequate.gitbooks.io/mostly-adequate-guide/).

- Quick-access side-bar
- In-browser exercises
- In-depth examples


## Download it

* [Download PDF](https://www.gitbook.com/download/pdf/book/mostly-adequate/mostly-adequate-guide)
* [Download EPUB](https://www.gitbook.com/download/epub/book/mostly-adequate/mostly-adequate-guide)
* [Download Mobi (Kindle)](https://www.gitbook.com/download/mobi/book/mostly-adequate/mostly-adequate-guide)


## Do it yourself

```
git clone https://github.com/MostlyAdequate/mostly-adequate-guide.git
cd mostly-adequate-guide/
npm install
npm run setup
gitbook pdf
```


# Table of Contents

See [SUMMARY.md](SUMMARY.md)

### Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md)

### Translations

See [TRANSLATIONS.md](TRANSLATIONS.md)

### FAQ

See [FAQ.md](FAQ.md)



# Plans for the future

* **Part 1** (chapters 1-7) is a guide to the basics. I'm updating as I find errors since this is the initial draft. Feel free to help!
* **Part 2** (chapters 8-10) will address type classes like functors and monads all the way through to traversable. I hope to squeeze in transformers and a pure application.
* **Part 3** (chapters 11+) will start to dance the fine line between practical programming and academic absurdity. We'll look at comonads, f-algebras, free monads, yoneda, and other categorical constructs.


---


<p align="center">
  <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">
    <img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" />
  </a>
  <br />
  This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.
</p>
