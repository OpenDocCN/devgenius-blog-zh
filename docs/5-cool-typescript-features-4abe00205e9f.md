# 5 Cool TypeScript features

> 原文：<https://blog.devgenius.io/5-cool-typescript-features-4abe00205e9f?source=collection_archive---------25----------------------->

Becomes “TypeScript” Developer

![](img/43e8dadd90d569f613774b8314965984.png)

Edited from Photo by [Florian Klauer](https://unsplash.com/@florianklauer?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/cool-type?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 1\. Generic Type

Ini fitur yang sangat diperlukan saat kita sudah terbiasa dengan javascript yang naturalnya bahasa dinamis. Kita ingin sebuah *type* tidak hanya mendukung satu *type* saja tapi mendukung variasi *type* sesuai konteks *type* itu digunakan dan fleksibel untuk digunakan kembali. Bahasa *strong type* seperti Java, Dart dan C# mendukung fitur ini. Contoh kongkritnya misal kita ingin sebuah fungsi mengembalikan nilai dengan *type* tertentu namun kita tidak tentukan saat fungsi itu didefinisikan. Bukan berarti tidak diberi *type,* melainkan kita berikan *type* ketika fungsi dieksekusi . Generic bisa dibilang peubah *type,* ini seperti parameter di fungsi dan biasanya disimbolkan dengan **T** . Link [playground](https://bit.ly/2CnepUe) dan link [dokumentasi lengkapnya](https://www.typescriptlang.org/docs/handbook/generics.html#generic-types)

Contoh Generic type

# 2\. Overloading

Overloading atau overloads adalah fitur *infer types* pada fungsi yang menerima parameter / mengembalikan kemungkinan banyak *type* dan berbeda disetiap eksekusi fungsi dan kita tidak ingin memberikan ***any*** *type.* Umumnya berdasarkan jumlah parameter yang berbeda maka pengembalian nilai *type* juga berbeda. Di bahasa Java fungsi overloads umumnya ditulis dengan [*function body*](https://bit.ly/30tjSky) *.* Adapun di TypeScript penulisan overloads berbeda dengan Java. Mengingat TypeScript sebisa mungkin mengikuti natural JavaScript maka penulisan fungsi overloads hanya nama fungsi dengan parameter *type* dan kembalian nilai *type* .

Contoh overloading

Link [playground](https://bit.ly/30lsFVw) ,abaikan error lain. Dan Full codenya dibawah ini.

[](https://github.com/ikhsanalatsary/akarata/blob/master/src/lib/morphological-utility.ts#L375-L398) [## ikhsanalatsary/akarata

### You can't perform that action at this time. You signed in with another tab or window. You signed out in another tab o

github.com](https://github.com/ikhsanalatsary/akarata/blob/master/src/lib/morphological-utility.ts#L375-L398) 

# 3\. Type refinement

Bisa diartikan penyempitan type, kemampuan untuk membuktikan bahwa variabelnya memiliki tipe tertentu. Ini membantu untuk membangun lebih banyak program jenis aman ( *type safe* ) ketika bekerja dengan user input atau respons server tapi tidak mengesampingkan analisis tipe statis ( *static type analysis* ). Link [playground](https://bit.ly/3eAmyS9)

Contoh type refinement

# 4\. Utility types / Magic types

TypeScript menyediakan beberapa tipe utilitas yang umumnya untuk memfasilitasi transformasi tipe seperti mengekstraksi atau membuat *type* baru dari yang sudah ada. Utilitas ini tersedia secara global. Link [playground](https://bit.ly/2ZFilbI) dan [dokumentasi lengkapnya](https://www.typescriptlang.org/docs/handbook/utility-types.html) .

Contoh utility type omit

# 5\. Typecasting / coercion

*Typecasting* , atau [konversi tipe](https://en.wikipedia.org/wiki/Type_conversion) , adalah metode untuk mengubah entitas dari satu tipe data ke yang lain. Ini digunakan dalam pemrograman komputer untuk memastikan variabel diproses dengan benar oleh suatu fungsi. Tadi adalah definisi dari istilah *typecasting.* Di JavaScript *typecasting* dikenal dengan istilah *coercion.* Hanya saja perilaku *coercion* di JavaScript itu dimana dalam konteks tertentu *type* entitas / peubah( *variable* ) dikonversi secara implisit. Di TypeScript, konversi itu harus di lakukan secara eksplisit. Contohnya, ini bisa dilakukan di JavaScript maupun TypeScript.

Contoh explicit coercion

Cara lain yang hanya valid di TypeScript, yaitu dengan memberikan kata kunci `as **`** walaupun ini hanya memberi tahu *compiler* untuk memberikan *type* tertentu ke peubah atau entitas di depannya.

Contoh menggunakan as keyword

Link [playground](https://bit.ly/2BdQJBa) dan [dokumentasi untuk as](https://www.typescriptlang.org/docs/handbook/basic-types.html#type-assertions) keyword.

Akhir cerita, sebagai JavaScript developer yang awalnya aneh dan takut ketika pertama kali diperkenalkan TypeScript. Wow, ternyata tidak semengerikan yang dibayangkan. Kita bisa menggunakan kemampuan type system dan JavaScript bersamaan. Mantap 👍 !!!

Well, Banyak feature di TypeScript yang ga disebutkan, seperti *union* , *intersection* dan seterusnya. Cukup mampir ke dokumentasi resminya, semua lengkap disana.

Ada kalanya kita mesti coba-coba bahasa yang memang *fully static types* seperti Dart atau Java, disini kita bisa pelajari lebih banyak, terutama benefit dari type system. Bahkan ada fitur yang ga ada di bahasa tersebut, contoh overloads tidak ada di Dart. Jadi agak menyusahkan konversi *library* JavaScript ke Dart. **Keep learning guys 💻 !!!**

**PS: tulisan di sini saya khususkan bahasa indonesia buat pembaca sedulur. Dan juga sebagai catatan pribadi saya seperti apa yang pernah saya pelajari dan alami saya tuangkan disini.**