# Demistifying CSS Cascade

![My Tweet](/tweet_kukuh.png)

Halo, selamat datang di tulisan pertama saya. Saya akan mulai dengan _little bit bragging: I've been doing CSS for almost **5 years**_. Menurut saya, 5 tahun adalah waktu yang cukup lama untuk bisa _fluent_ dalam suatu hal. Namun dalam perjalanannya, pun hingga saat ini, saya masih sering mengalami kesulitan dan keresahan untuk memahami CSS yang terkadang tidak bekerja seperti yang saya inginkan. _Depicted [my tweet](https://twitter.com/suliskh/status/1068036449071587329) above_. Hingga pada suatu saat saya menemukan [_super intriguing tweet poll_](https://twitter.com/mxstbr/status/1038073603311448064) dari [Max Stoiber](https://twitter.com/mxstbr) seperti berikut:

![Max's Tweet](/tweet_max_stoiber.png)

> Yeah, pertanyaan yang simpel namun menarik bukan? Jawaban mana yang akan teman-teman pilih?

Saya pun termasuk ke dalam 44% orang yang menjawab **salah**. Ya, 44% dari 14.517 adalah angka yang cukup besar untuk sebuah kesalahpahaman mengenai konsep fundamental dari sesuatu (_red_: CSS) yang sering dianaktirikan dan dianggap remeh, _HAHA_.

> Lantas, jawaban mana yang benar? Mengapa demikian?

Dalam proses pencariannya, saya menemukan suatu hal yang sangat menarik yang harap bisa saya ketahui dan pahami sedari dulu kala, adalah CSS Cascade. MARKIBAH di tulisan ini ~

## The Cascading Nature of CSS

CSS adalah kependekan dari _Cascading Style Sheet_. Ya, kata _cascade_ adalah bagian dari CSS dan merupakan hal fundamental untuk bisa memahami bagaimana CSS bekerja.

Seringkali kita menemukan CSS _rules_ yang kita tulis untuk melakukan _styling_ pada suatu _element_ itu tidak bekerja sebagaimana yang kita inginkan. Hal tersebut bisa terjadi karena ada _rules_ lain yang teraplikasikan ke elemen yang kita tuju, baik dari _styling_ yang kita buat sendiri di bagian lain ataupun _third-party_ CSS yang kita pasang. _Cascading_ (dan konsep _specifity_) adalah mekanisme browser untuk menentukan manakah _rules_ yang akan teraplikasikan pada suatu _element_ apabila terdapat konflik seperti tersebut di atas. Banyak orang menganggap mekanisme ini sebagai _flaws_ dari CSS yang membuatnya sulit dipelajari dan banyak bug, namun menurut saya mekanisme tersebut merupakan fitur fundamental dari CSS yang sangat bermanfaat dan memudahkan pekerjaan apabila kita memahami bagaimana caranya bekerja.

Terdapat 3 faktor yang digunakan dalam mekanisme _cascading_ untuk menentukan _rules_ yang akan teraplikasikan pada suatu `element`. Berikut adalah daftarnya yang terurut naik berdasarkan prioritasnya (item di bawah akan meng-_overrule_ item yang ada di atasnya):

1. Source Order
2. Specifity
3. `!important`

## Source Order

Penentuan `cascading` paling awal ditentukan dari urutan pendeklarasian _rule_-nya. Secara sederhana, jika terdapat lebih dari satu _rules_ yang dituliskan pada elemen yang sama (dengan bobot _specifity_ yang sama), maka yang akan teraplikasi adalah _rule_ yang dideklarasi terakhir.

Tinjau CSS berikut

```css
h1 {
  color: red;
}
h1 {
  color: blue;
}
```

Maka `h1` akan memiliki `color: blue`

Selain itu, kita juga perlu ingat bahwa terdapat beberapa _styling_ dari sumber lain yang mempengaruhi _cascading_. Berikut adalah daftarnya dengan terurut naik berdasarkan prioritasnya.

1. _User-agent stylesheets_

Merupakan _basic/default styling_ yang diterapkan oleh browser poda beberapa komponen `html`. Beberapa browser menerapkan _styling_ yang berbeda-beda. Oleh karena itu kita sering memanfaatkan _third-party_ seperti [`reset.css`](https://meyerweb.com/eric/tools/css/reset/) atau [normalize.css](https://necolas.github.io/normalize.css/) untuk menyeragamkan _default styling_-nya.

2. _Author stylesheets_

Ini adalah tipe _styling_ yang paling umum, berasal dari _styling_ yang ditulis sendiri ataupun _third-party_ CSS. Hal yang perlu diperhatikan adalah _inline style_ akan meng-_overrule_ yang lainnya.

Tinjau kode berikut:

`style.css`:

```css
h1 {
  color: red;
}
```

`index.html`:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <link rel="stylesheet" href="style.css" />
    <style>
      h1 {
        color: green;
      }
    </style>

    <title>Document</title>
  </head>
  <body>
    <h1 style="color: blue">Indonesia</h1>
  </body>
</html>
```

Maka, `h1` akan memiliki `color: blue`

3. _User stylesheets_

Ini adalah `styling` yang dikustomasi oleh _user_ melalui pengaturan di _browser_ ataupun via _extension_. Jika memiliki bobot _specifity_ yang sama, maka _styling_ dari sumber ini akan selalu meng-_overrule_ yang lainnya.

## Specifity

_Specifity_ adalah sebuah bobot yang diberikan pada suatu _CSS declaration_. _Specifity_ yang dihitung dari jumlah dan bobot setiap tipe _selector_._Rules_ yang akan teraplikasi adalah _rules_ dari _CSS declaration_ yang memiliki bobot tertinggi.

[ESTELLE WEYL][estelle weyl](http://www.standardista.com/css3/css-specificity/) dalam tulisannya menjelaskan dengan jelas bagaimana penghitungan bobot _specifity_ bekerja.

- `X-0-0`: jumlah _id selectors_ (e.g. `#home`)
- `0-Y-0`: jumlah _class selectors_ (e.g. `.heading`), _attributes selector_ (e.g. `[type="checkbox"]`), dan _pseudo-classes_ (e.g. `:hover`)
- `0-0-Z`: jumlah _type selectors_ (e.g. `h1`, `body`, `nav`) dan _pseudo-elements_ (e.g. `::after`, `::before`) -`*`: _universal selector_ tidak memiliki bobot
- `+, >, ~`: _combinators_ tidak mempengaruhi bobot _specifity_ -`:not(x)`: _negation selector_ tidak memiliki bobot, tetapi argumen yang dimasukkan akan mempengaruhi bobot _specifity_

Bobot total dihitung dengan menggabungkan `X-Y-Z`

Misalnya, tinjau kode berikut

`HTML`:

```html
<p class="myClass" id="myDiv"></p>
```

`CSS 1`:

```css
#myDiv {
  color: red;
} /* 1-0-0 */
.myClass {
  color: blue;
} /* 0-1-0 */
p {
  color: yellow;
} /* 0-0-1 */
```

`CSS 2`:

```css
p.myClass {
  color: brown;
} /* 0-1-1 */
p[id^="my"] {
  color: purple;
} /* 0-1-1 */
p:nth-of-type(1n) {
  color: orange;
} /* 0-1-1 */
```

Pada `CSS 1`, `#myDiv { co redlor: red; }` memiliki bobot tertinggi (100 > 10 > 1), sehingga _element_ akan memiliki `color: red`

Pada `CSS 2`, ketiga _declaration_ memiliki bobot yang sama, maka yang diambil adalah yang terakhir _(source order)_, sehingga _element_ akan memiliki `color: orange`

## `!important`

`!important` adalah sebuah sintaks khusus yang bisa digunakan untuk meng-_overrule_ semua peraturan-peraturan di atas.

Tinjau kode berikut

`HTML`:

```html
<h1 id="head1" style="color: blue">Indonesia</h1>
```

`CSS`:
```css
.heading {
  color: red !important;
}

h1#heading {
  color: yellow;
}
```

Walaupun terdapat *declaration* yang memiliki bobot *specifity* lebih besar dan terdapat *inline style*, namun `!important` akan meng-*overrule* *styling* lainnya. Sehingga *element* memiliki `color: red`.
