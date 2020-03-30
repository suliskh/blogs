Demistifying CSS Cascade
========================

![My Tweet](/tweet_kukuh.png)

Halo, selamat datang di tulisan pertama saya. Saya akan mulai dengan *little bit bragging: I've been doing CSS for almost __5 years__*. Menurut saya, 5 tahun adalah waktu yang cukup lama untuk bisa *fluent* dalam suatu hal. Namun dalam perjalanannya, pun hingga saat ini, saya masih sering mengalami kesulitan dan keresahan untuk memahami CSS yang terkadang tidak bekerja seperti yang saya inginkan. *Depicted [my tweet](https://twitter.com/suliskh/status/1068036449071587329) above*. Hingga pada suatu saat saya menemukan [*super intriguing tweet poll*](https://twitter.com/mxstbr/status/1038073603311448064) dari [Max Stoiber](https://twitter.com/mxstbr) seperti berikut:

![Max's Tweet](/tweet_max_stoiber.png)

> Yeah, pertanyaan yang simpel namun menarik bukan? Jawaban mana yang akan teman-teman pilih?

Saya pun termasuk ke dalam 44% orang yang menjawab **salah**. Ya, 44% dari 14.517 adalah angka yang cukup besar untuk sebuah kesalahpahaman mengenai konsep fundamental dari sesuatu (*red*: CSS) yang sering dianaktirikan dan dianggap remeh, *HAHA*. 

> Lantas, jawaban mana yang benar? Mengapa demikian?

Dalam proses pencariannya, saya menemukan suatu hal yang sangat menarik yang harap bisa saya ketahui dan pahami sedari dulu kala, adalah CSS Cascade. MARKIBAH di tulisan ini ~

## The Cascading Nature of CSS
CSS adalah kependekan dari *Cascading Style Sheet*. Ya, kata *cascade* adalah bagian dari CSS dan merupakan hal fundamental untuk bisa memahami bagaimana CSS bekerja. 

Seringkali kita menemukan CSS *rules* yang kita tulis untuk melakukan *styling* pada suatu *element* itu tidak bekerja sebagaimana yang kita inginkan. Hal tersebut bisa terjadi karena ada *rules* lain yang teraplikasikan ke elemen yang kita tuju, baik dari *styling* yang kita buat sendiri di bagian lain ataupun *third-party* CSS yang kita pasang. *Cascading* (dan konsep *specifity*) adalah mekanisme browser untuk menentukan manakah *rules* yang akan teraplikasikan pada suatu *element* apabila terdapat konflik seperti tersebut di atas. Banyak orang menganggap mekanisme ini sebagai *flaws* dari CSS yang membuatnya sulit dipelajari dan banyak bug, namun menurut saya mekanisme tersebut merupakan fitur fundamental dari CSS yang sangat bermanfaat dan memudahkan pekerjaan apabila kita memahami bagaimana caranya bekerja.

Terdapat 3 faktor yang digunakan dalam mekanisme *cascading* untuk menentukan *rules* yang akan teraplikasikan pada suatu `element`. Berikut adalah daftarnya yang terurut naik berdasarkan prioritasnya (item di bawah akan meng-*overrule* item yang ada di atasnya):

1. Source Order
2. Specifity
3. `!important`

## Source Order

Penentuan `cascading` paling awal ditentukan dari urutan pendeklarasian *rule*-nya. Secara sederhana, jika terdapat lebih dari satu *rules* yang dituliskan pada elemen yang sama, maka yang akan teraplikasi adalah *rule* yang dideklarasi terakhir.

Tinjau CSS berikut
```css
h1 { color: red; }
h1 { color: blue; }
```
Maka `h1` akan memiliki `color: blue`

Selain itu, kita juga perlu ingat bahwa terdapat beberapa *styling* dari sumber lain yang mempengaruhi  *cascading*. Berikut adalah daftarnya dengan terurut naik berdasarkan prioritasnya.

1. *User-agent stylesheets*

Merupakan *basic/default styling* yang diterapkan oleh browser poda beberapa komponen `html`. Beberapa browser menerapkan *styling* yang berbeda-beda. Oleh karena itu kita sering memanfaatkan *third-party* seperti [`reset.css`](https://meyerweb.com/eric/tools/css/reset/)  atau [normalize.css](https://necolas.github.io/normalize.css/) untuk menyeragamkan *default styling*-nya.

2. *Author stylesheets*

Ini adalah tipe *styling* yang paling umum, berasal dari *styling* yang ditulis sendiri ataupun *third-party* CSS. Hal yang perlu diperhatikan adalah *inline style* akan meng-*overrule* yang lainnya (tanpa memperhatikan *specifity*).

Tinjau kode berikut:

`style.css`
```
h1 {
    color: red;
}
```
`index.html`
```
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

3. *User stylesheets*

Ini adalah `styling` yang dikustomasi oleh *user* melalui pengaturan di *browser* ataupun via *extension*. Tanpa memperhatikan *specifity*, maka *styling* dari sumber ini akan selalu meng-*overrule* yang lainnya.

## Specifity


## Importance (`!important`)

