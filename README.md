# JavaScript'te 50 İpucu (50 Tips on JavaScript)

[Krasimir Tsonev](https://github.com/krasimir)'in [50 Tips on JavaScript](https://50tips.dev) kitabının çevirisidir. Çeviriyi kendi yorumlarımla birlikte yaptım ve anlaşılır olması için kod örneklerine de kendi yorumlarımı ekledim. Herhangi bir hata veya geliştirme için lütfen PR açınız. Ayrıca tüm kelimeleri Türkçe'ye çevirmedim. Daha anlaşılır ve teknik isimlendirmeleri genellikle aynı şekilde bıraktım.

Bu kitap JavaScript'teki ufak ipucuları, JavaScript'te geçmişten günümüze değişen yapılar, yeni API'lar gibi konularda bilgi vermektedir.

![50 Tips on JavaScript](https://50tips.dev/img/cover.jpg)

### İçindekiler

| #  | İçerik İsmi                                                                                                                |
|:---|----------------------------------------------------------------------------------------------------------------------------|
| 1  | [Strict Eşitliği](#1-strict-eşitliği)                                                                                      |
| 2  | [Virgül Operatörü (Comma Oparator)](#2-virgül-operatörü-comma-oparator)                                                    |
| 3  | [Spread Operatörü (Spread Operator)](#3-spread-operatörü-spread-operator)                                                  |
| 4  | [Destructuring](#4-destructuring)                                                                                          |
| 5  | [Opsiyonel Zincirleme (Optional Chaining)](#5-opsiyonel-zincirleme-optional-chaining)                                      |
| 6  | [Referansa veya Değere Göre (By Reference or By Value)](#6-referansa-veya-değere-göre-by-reference-or-by-value)            |
| 7  | [Reducing](#7-reducing)                                                                                                    |
| 8  | [Async/await](#8-asyncawait)                                                                                               |
| 9  | [Iterable Protocol](#9-iterable-protocol)                                                                                  |
| 10 | [Generators](#10-generators)                                                                                               |
| 11 | [Priting JSON](#11-priting-json)                                                                                           |
| 12 | [Object.assign](#12-objectassign)                                                                                          |
| 13 | [Capture Groups](#13-capture-groups)                                                                                       |
| 14 | [Etiketli Template Literal](#14-etiketli-template-literal)                                                                 |
| 15 | [Media Query List](#15-media-query-list)                                                                                   |
| 16 | [Event Delegation](#16-event-delegation)                                                                                   |
| 17 | [Error Handling](#17-error-handling)                                                                                       |
| 18 | [Eski Zamanlardan Bir Anı](#18-eski-zamanlardan-bir-anı)                                                                   |
| 19 | [Asenkron Derhal Çağrılan Fonksiyon İfadesi](#19-asenkron-derhal-çağrılan-fonksiyon-i̇fadesi)                              |
| 20 | [Asenkron Kuyruk](#20-asenkron-kuyruk)                                                                                     |
| 21 | [JavaScript Modül Sistemi Olarak Bir Singleton](#21-javascript-modül-sistemi-olarak-bir-singleton)                         |
| 22 | [Call-to-action Eklentilerini Script Etiketiyle Değiştirme](#22-call-to-action-eklentilerini-script-etiketiyle-değiştirme) |
| 23 | [Nesnelerden Alanları Çıkarma](#23-nesnelerden-alanları-çıkarma)                                                           |

------

## 1. Strict Eşitliği

Uzun zamandır JavaScript yazıyorum ve öğrendiğim başlıca şeylerden birisi `==` (iki eşitlik) yerine `===` (üçlü eşitlik) kullanmaktır. Bu daha güvenli bir yöntemdir. Örneğin:

```javascript
[user.id](http://user.id) === user.id
```

yerine

```javascript
[user.id](http://user.id) == id
```

kullandığımızda bir çok hataya da davet çıkarmış oluruz. Bunun sebebi genellikle mantıksız görünen ifadelere denk gelmemizdir. Örnek olarak boş bir string ile false olan bir boolean değerini karşılaştıralım:

```javascript
console.log("" == false); // true
```

Bu karşılaştırma `true`’dur çünkü == (çiftli eşitlik) kullandığımızda basit bir eşitlik kullanırız. Sonuç `true`’dur çünkü her iki değer de ortak bir type’a dönüştürülür ve öyle karşılaştırılır. Bu karşılaştırma daha çok şu eşitliğe benzer:

```javascript
console.log(Boolean("") === false); // true
```

JavaScript motoru type dönüşümünden sonra `===` (strirct equality)’yi kullanır. 

Yani üçlü eşitlik kodunuzu daha anlaşılır hale getirir ve sizi bazı garip hatalardan kurtarır.

---

## 2. Virgül Operatörü (Comma Oparator)

Virgül operatörünün JavaScript'te hafife alındığını düşünüyorum. Popüler bakış açısı virgülü fonksiyondaki parametreleri ve array içindeki elemanları ayırmamız için kullanıldığımız yönünde. Ancak başka bir kullanımı daha var.

```javascript
const page = {
  visits: 42
};
const newPageView = () => {
  return page.visits += 1, page.visits;
};

console.log(newPageView()); // 43;
console.log(newPageView()); // 44;
```

`newPageView` `visits` değerini arttıran ve geri return eden bir fonksiyondur. Virgül operatörü her bir işleneni soldan sağa doğru değerlendirir ve en sonuncuyu `return` eder. `newPageView` fonksiyonunun yeni değeri döndürmesinin sebebi budur. 

Birisi çıkıp bu bir yazım tarzının ne zaman faydalı olduğunu soracaktır. Gerçek şu ki virgül operatörü kodu tek satırda yazdığınızda mantıklı olacaktır. Aşağıdaki üçlü operatör örneği gibi.

```javascript
const a = () => 'a';
const b = () => 'b';
const isValid = true;
const result = isValid ? (a(), b()) : 'Nope';
console.log(result); // b
```

---

## 3. Spread Operatörü (Spread Operator)

![Spread Operator](https://50tips.dev/tip-assets/3/art.jpg)
Geçtiğimiz günlerde JavaScript'e gelen yeni özellikleri okumaya başladığımda beni en çok heyecanlandıran şey spread operatörü olmuştu. Çeşitli yerlerde genişletilebilir iterable (veya stringler) oluşturmamıza izin vermektedir. Genellikle bu operatörü object oluştururken kullanırım. Örneğin: 

```javascript
const name = {
  firstName: "Krasimir",
  lastName: "Tsonev"
};

// Adding fields to an object
const profile = { age: 36, ...name };
console.log(profile); // age, first, last name
```

Bu yöntem property'leri overwrite etmek için de kullanılabilir. Diyelim ki yukarıdaki profil alanında yaş alanını güncellemek istiyoruz. Şu şekilde yapabiliriz:

```javascript
const updates = { age: 37 };
const updatedProfile = { ...profile, ...updates };
console.log(updatedProfile); // age=37
```

Bir başka popüler kullanım da birden çok argümanı fonksiyona iletmek:

```javascript
const values = [10, 33, 42, 2, 9];
console.log(Math.max(...values));
// instead of
console.log(Math.max(10, 33, 42, 2, 9));
```

Son olarak spread operatör array'leri klonlama konusunda iyi iş çıkarır. Deep klon değil de orijinal object'in mutasyonunu önlemek için yapar. Fonksiyonlara, object'leri ve array'leri referansa göre (bu tamamen doğru değil) nasıl ilettiğimizi hatırlayın. Bu gibi benzer durumlarda array'lerle çalışıp onları değiştirmemek isteyebiliriz. Aşağıdaki örneğe bakabilirsiniz:

```javascript
const values = [10, 33, 42, 2, 9];
function findBiggest(arr) {
  return arr.sort((a, b) => (a > b ? -1 : 1)).shift();
}
const biggest = findBiggest(values);
console.log(biggest), // 42
console.log(values); // [33, 10, 9, 2]
```

`findBiggest` fonksiyonu array içerisindeki elemanları sıralar ve en büyük olanı geri döner. Buradaki problem bu işlemi array'i mutasyona uğratan `shift` metoduyla yapmasıdır. Sonucunda ilk array'den daha az elemana sahip bir array kalır elimizde.

Bu sorunu çözmek için spread operatörünü kullanıyoruz.

```javascript
const values = [10, 33, 42, 2, 9];
function findBiggest(arr) {
  return [...arr].sort((a, b) => (a > b ? -1 : 1)).shift();
}
const biggest = findBiggest(values);
console.log(biggest), // 42
console.log(values); // [10, 33, 42, 2, 9]
```

---

## 4. Destructuring

![Destructing](https://50tips.dev/tip-assets/4/art.jpg)

İlk olarak bazı programlama dillerde destructuring gördüm. Bunu anlamam biraz zamanımı aldı ama sonunda mantığını anladım. Artık JavaScript'te ne kadar güzel olabileceğini düşünmeden duramıyordum. Bundan bir kaç ay sonra destructuring tanıtıldı. Destructuring array ve object elemanlarını tek tek değişkenlere çıkaran bir JavaScript ifadesidir. Şu şekilde bir object'i destruct ederiz: 

```javascript
const user = { name: "Krasimir", job: "dev" };
const { name, job } = user;
console.log(`${name}, position: ${job}`); // Krasimir, position: dev
```

Bu tanımlamanın diğer bir adı da `unpacking`'dir. Burada da gördüğümüz gibi `name` ve `job` alanları `user`object'ini içerisinden gelmektedir.

Array'lerde de benzer şekilde çalışır:

```javascript
// destructuring array
const arr = ["oranges", "apples", "bananas"];
const [a, b, c] = arr;
console.log(`I like ${a}`); // I like oranges
```

Sonunda eğer tanımlamaları beğenmediysek alias(takma ad) da oluşturabiliriz. Aşağıdaki örnekte `name` `who` olurken `job` da `position` olur:

```javascript
// destructuring with renaming
const user = { name: "Krasimir", job: "dev" };
const { name: who, job: position } = user;
console.log(`${who}, position: ${position}`); // Krasimir, pos
```

---

## 5. Opsiyonel Zincirleme (Optional Chaining)

![Optional Chaining](https://50tips.dev/tip-assets/5/art.jpg)

JavaScript'le çalışırken favori hatam `Cannot read properties of undefined`'dır. İşte bu hataya neden olan kod örneği:

```javascript
const user = { name: 'Krasimir' };
console.log(user.skills[0]);
// Uncaught TypeError: Cannot read properties of undefined (reading '0')
```

Bu durumu önlemek için kontroller koyup helper'lar (yardımcılar) kullanırız. Deeply nested property'lerindeki (iç içe geçmiş property'ler yani object içerisindeki object veya array içerisindeki object vs. gibi) değeri okumak her zaman hataların kaynağı olmuştur. Bugün JavaScript'in bu sorunu çözen bir özelliği var. Bu özelliğe optional chaining (opsiyonel zincirleme) denir. İç içe geçmiş alanlarda güvenli erişim sunuyor. Eğer iç içe geçmiş alanlardan herhang birisi `undefined` veya `null` olduğunda tüm ifade `undefined` olur yani hata vermez.

```javascript
const user = { name: 'Krasimir' };
console.log(user?.skills?.[0]); // undefined
```

---

## 6. Referansa veya Değere Göre (By Reference or By Value)

![By Reference or By Value](https://50tips.dev/tip-assets/6/art.jpg)

Programlamada değeri iletmek (passing a variable) ifadesi vardır. Bu bir fonksiyonu parametre ile çağırdığınız anda gerçekleşir. Bazı programlama dillerinde bu parametreler değere göre bazı dillerde de referansa göre iletilir. JavaScript'te ise biraz karma bir yapı var. 

Primitive (ilkel) tipler -number, string vb. gibi- değeri iletirken non-primitive (ilkel olmayan) tipler ise -object, array vb. gibi- referansın bir kopyasını iletir. Bunu bir kaç örnekle açıklayalım.

```javascript
var numOfUsers = 24;
function doSomething(num) {
  num += 1;
}

doSomething(numOfUsers);

console.log(numOfUsers); // 24
```

`numOfUsers`'ı var ile tanımlasak da aslından onun değerini iletiyoruz. Yani fonksiyonun içindeki değeri alıyoruz ama onun orijinal değerini değiştiremiyoruz.

İşte başka bir örnek: 

```javascript
const user = { score: 78 }
function mutates(obj) {
  obj.score += 12;
}

mutates(user);
console.log(user.score); // 90
```

Burada `user`'ın referansının bir kopyası iletilir. Object'in içerisindeki alanları değiştirebiliyoruz fakat orijinal tanımı değiştiremiyoruz. Aşağıdaki kod örneği bunu kanıtlıyor: 

```javascript
const user = { score: 78 }
function doesntMutate(obj) {
  obj = { score: 120 }
}

doesntMutate(user);
console.log(user.score); // 78
```

Bazı programlama dillerinde ise `obj = { score: 120 }` tanımlamasının `user` object'inin değerini değiştirebileceğini de not etmeliyiz.

---

## 7. Reducing 

![Reducing](https://50tips.dev/tip-assets/7/art.jpg)

Genellike object'leri bir şekilden diğer bir şekle dönüştürmemiz gerekir. Bu tarz dönüşümler için benim favori aracım reduce fonksiyonlarıdır. Aşağıdaki gibi içerisinde object'leri içeren bir array'imizin olduğunu varsayalım:

```javascript
const positions = [
  { languages: ["HTML", "JavaScript", "PHP"], years: 4 },
  { languages: ["JavaScript", "PHP"], years: 2 },
  { languages: ["C", "PHP"], years: 3 },
];
```

Ve her bir dil için toplam `years` değerine ihtiyacımız var. Çözüm döngü oluşturup içerisindeki bazı değerleri almayı içerir. Tam olarak `reduce` işlemi şunun için yapılır: 

```javascript
const yearsOfWriting = positions.reduce((res, { languages, years }) => {
  languages.forEach((lang) => {
    if (!res[lang]) res[lang] = 0;
    res[lang] += years;
  });
  return res;
}, {});

console.log(yearsOfWriting); // { HTML: 4, JavaScript: 6, PHP: 9, C: 3 }

```

`reduce` Array prototipinin bir metodudur. Array içerisindeki elemanları döngüye sokarak değer toplamamızı sağlar. 

Ayrıca bu metod object'ler için de kullanılabilir. Doğrudan değil ama object'i `Object.keys` ile iletebiliriz. `Object.keys ` ile array'in tüm property'lerini alacağız ve bunları yineleyerek işlemimizi yapacağız. Örneğe devam edelim ve başka bir soru soralım: Toplamda kaç yıl(`years`) var?

```javascript
const yearsOfWriting = { HTML: 4, JavaScript: 6, PHP: 9, C: 3 };
const totalYears = Object.keys(yearsOfWriting).reduce((total, key) => {
  return total + yearsOfWriting[key];
}, 0);

console.log(totalYears); // 22
```

---

## 8. Async/await

![Async/await](https://50tips.dev/tip-assets/8/art.jpg)

JavaScript'te eşzamansızlık (asenkron) konusunu çok geniş bir alan. Çünkü JavaScript dili asenkron işlemleri çözmek için bir çok mekanizma sunmaktadır. Yıllar geçtikçe bu mekanizmalar da değişiyor.  Tarihsel olarak, native API'lar bu sorunu çözmek için tanıtılan ilk  yöntemdi. Bu yöntemde fonksiyon belirsiz bir gelecekte çağırılan `callback` isimli bir fonksiyonu kabul eder.

```javascript
const fs = require('fs');

fs.readFile('./content.md', function callback(error, result) {
  if (error) {
    // eğer hata varsa
  } else {
    // datayı kullan
  }
})
```

`callback` fonksiyonu bir işlem yapılmasına göre tetiklenir. Ya dosyayı başarılı bir şekilde okuduk ya da burada bir hata var. Potansiyel hatayı ilk argüman olarak kabul etmek güzel bir yöntemdi. Bu düzen hata çözmeyi de teşvik eder. Fakat çok fazla `callback` kullanmak derinlemesine içe içe geçmiş fonksiyonlara yol açar ve `callback hell` (callback cehennemi) olarak isimlendirilir.

```javascript
functionA(param, function (err, result) {
  functionB(param, function (err, result) {
    functionC(param, function (err, result) {
      // böyle devam eder
    });
  });
});
```

`callback hell` 'i düzeltmesi gereken API ise `promise` olacaktır. `promise` gelecekteki işlemlerimizi üstlenecek bir object'tir. Tabi başarılı veya başarısız da olabilir. Bu object'ler üç durumu vardır: `resolved (çözüldü) `,  `rejected(reddedildi)` ve `pending(bekleniyor) `'dur. 

İşte promise'leri kullanarak dönüştürülmüş aynı örnek: 

```javascript
import { readFile } from 'fs/promises';
readFile('./content.md')
  .then(data => { ... })
  .catch(err => { ... });
```

Promise'lerin callback'ler gibi problemleri yoktu fakat mükemmel de değildir. İç içe veya birbirine bağlı promise'ler yazdığımızda yeniden karışık kod blocklarıyla karşılaşabiliriz.

Günümüzde ise herkes tamamiyle başka bir API'yi kullanıyor: `async/await` . Bu API, bize fonksiyonun önüne eklenen `async` kelimesiyle asenkron fonksiyon tanımlamıza izin veriyor. Ardından fonksiyonun içerisinde promise işlemlerin öncesinde `await` kelimesini ekleyebiliriz. Bu fonksiyonu promise işlemi çözülene (resolved) kadar duraklatacaktır. İşte aynı örneğin `async/await` kullanılarak yeniden yazılmış hali: 

```javascript
import { readFile } from 'fs/promises';

async function getContent() {
  try {
    const data = await readFile('./content.md');
    // datayı kullan
  } catch(err) {
    // hata var
  }
}

getContent();
```

Önemli bir gerçek de her `async` kullanılan tüm fonksiyonların promise ile sonuçlanmasıdır. Örnek olarak aşağıdaki `getResult` fonksiyonu 10 döndürmez, onun yerine 10 değeriyle çözülen (resolved) bir promise döndürür.

```javascript
async function getResult() {
  return 10;
}
console.log(getResult());
// Promise<fulfilled: 10>
```



---

## 9. Iterable Protocol 

![Iterable Protokol](https://50tips.dev/tip-assets/9/art.jpg)

Iterable protokol hafife alınıyor. İnsanların kullanmadığını görüyorum ve bu beni çok üzüyor. Çünkü aslında muhteşem bir araç. Spesifik object'lerde nasıl döngü yapacağımızı belirler. Başka bir deyişle özelleştirilmiş iterating davranışı oluşturmamıza imkan verir. `@@iterator` (`Sysbol.iterator`'un kısa yazılışı) isimli bir property oluşturmamız gerekiyor. Bu property `iterable protocol` ile eşleşen, bir object dönen ve herhangi bir argüman almayan fonksiyona eşit olması gerekiyor. İşte örnek:

```javascript
const user = {
  data: ["JavaScript", "HTML", "CSS"],
  [Symbol.iterator]: function () {
    let i = 0;
    return {
      next: () => ({
        value: this.data[i++],
        done: i > this.data.length,
      }),
    };
  },
};
for (const skill of user) { console.log(skill); }
```

Bu protokol `next` metodunu dönen bir object almak zorundadır. Bu metod  `value` ve `done` alanlarını alan bir object ile sonuçlanmalıdır. `value` her şey olabilir ve `done` ise iteration işleminin bitip bitmediğini gösteren bir boolean değeridir. 

Yukarıdaki örnekte fark ederseniz `user` değeri array değildir ama array gibi kullanıyoruz. Bunun nedeni özelleştirilmiş iterator tanımı yapmamızdandır. Bu tanımlama `"JavaScript"`, `"HTML"` ve `"CSS"` (sırasıyla) çıktısını verir.

Bu özellikle karmaşık datalar ve iç içe geçmiş property'lere erişmek istediğimizde kullanışlı olur. Object'lerimi destroy edilmesini kolaylaştırmak için iterable protokol kullanmak hoşuma gidiyor.

```javascript
const user = {
  name: { first: 'Krasimir', last: 'Tsonev' },
  position: 'engineer',
  [Symbol.iterator]: function () {
    let i = 0;
    return {
      next: () => {
        i++;
        if (i === 1) {
          return { value: `${this.name.first} ${this.name.last}`, done: false };
        } else if (i === 2) {
          return {value: this.position, done: false };
        }
        return { done: true }
      }
    }
  }
}
const [name, position] = user;
console.log(`${name}: ${position}`); // Krasimir Tsonev: engineer

```

Tanım olarak, tüm iterable object'lerini destory edebiliriz. Eğer object'lerimiz iterable değilse de yukarıdaki gibi iterable protocol tanımlayarak destroy işlemlerimizi yapabiliriz.

---

## 10. Generators

![Generators](https://50tips.dev/tip-assets/10/art.jpg)

Aynı `iterable protocol` gibi, JavaScript'te `generator` kullanmak da popüler değil. Topluluk bu API üzerinde çok fazla çalışmıyor. Fakat bence  potansiyeli var. Özellikle de asenksron süreçleri çözmede. 

Generator fonksiyonlar çağrıldığında iterable generator döndüren özel tipli bir fonksiyondur. Bu, fonksiyonun içindeki kodun hemen yürütülmediği anlamına gelir. Generator'da `next` metodunu çağırmamız gerekiyor. Ardından yürütme işlemi `yield` ifadesine kadar devam eder. Aşağıdaki kod örneği bu yapıyı gösteriyor:

```javascript
function* calculate() {
  yield 10;
  const result = yield 5;
  console.log(`Result = ${result}`);
}

const g = calculate();
const res1 = g.next(); // {value: 10, done: false}
const res2 = g.next(); // {value: 5, done: false}
g.next(res1.value + res2.value); // Result = 15
```

Bu örneği satır satır okuyup ne olduğunda bakalım:

- `const generator = calculate()` - bu noktada fonksiyonun içerisi yürütülmüyor yani çalışmıyor. Biz sadece generator üretiyoruz. Aslında `calculate` istediği kadar generator üretebilir. Farklı iterable'lar olacaktır böylece.
- `const res1 = generator.next()` - burada fonksiyon çalışıyor ve yürütme ilk `yield` ifadesinde duruyor. `yield` değeri ne olursa olsun `value` alanı objede döndürülür.
- `const res2 = generator.next()` - yukarıdaki satırla value değerinin 5 olması dışında aynı. Ve `result` değerini almadan önce fonksiyonun duraklatıldığını söyleyebiliriz. Bu noktada, `results` değeri hala tanımlanmamıştır. Sadece tanımdan sonraki `next` metodu dışardan çağrıldığında işlem tamamlanır.
- `generator.next(res1.value + res2.value)` - bu satır daha önce dışa çıkarılan (export edilen) tam sayıların toplamını göndererek generator işlemini devam ettirir.

Generator'ın datayı gönderme ve alma yeteneği onu benzersiz kılar ve bazı ilginç implementasyonlara olanak sağlar. Bunlardan en yaygın olanı da komut deseni (command pattern) implementasyonudur. Bir kaç adımdan oluşan bir şeyi nasıl oluşturacağımızı düşünün. Örneğin, uzak sunucudan bir resim URL'i almak ve `<img>` etiketi oluşturmak istiyoruz. Generator'le nasıl görüneceği şu şekildedir:

```javascript
commander(robot());

function* robot() {
  const catURL = yield ["get-cat"];
  const imgTag = yield ["format", catURL];
  console.log(imgTag);
}
async function commander(gen, passBack) {
  let state = gen.next(passBack);
  switch (state.value ? state.value[0] : null) {
    case "get-cat":
      const res = await fetch("https://api.thecatapi.com/v1/images/search");
      const data = await res.json();
      return commander(gen, data[0].url);
    case "format":
      return commander(gen, `<img src="${state.value[1]}" />`);
  }
  if (!state.done) {
    commander(gen);
  }
}
```

```javascript
// Bir iki saniye sonra şu sonucu alacağız:
// <img src="https://cdn2.thecatapi.com/images/<random id here>.jpg" />
```

Generator fonksiyonumuz `robot` , komutları gönderiyor ve asenkron olarak sonuçları alıyor. `get-cat` [thecatapi.com](thecatapi.com) adresine HTTP isteği gönderen ve rastgele resim URL'i döndüren bir komuttur. `format` başka bir komuttur ve resim etiketi döndürür. Ayrıca generator'ler, async/await'e benzer şekilde fonksiyonumuzun eşzamanlı (synchronous) olarak görünmesini sağlar. 

---

## 11. Priting JSON

![Printings JSON](https://50tips.dev/tip-assets/11/art.jpg)

Bu bölüm, her developer'ın sıklıkla kullandığındığı debugging tool olan konsola yazdırmayı ele almaktadır. JavaScript'te her zaman veri yapılarıyla (data structures) çalışırız. Sıklıkla object'lerin içine bakma ihtiyacımız olur. Objeyi yazdırmadaki favorilerimden bir tanesi de `JSON.stringify` API'ını kullanmaktır.

```javascript
const user = {
  name: "Molecule Man",
  age: 29,
  secretIdentity: "Dan Jukes",
  powers: ["Radiation resistance", "Radiation blast"],
};

console.log(JSON.stringify(user, null, 2));
```

`null` ve `2` parametrelerine dikkat edin. Bunlarsız, her şey tek bir satırda görünecektir. İkinci parametre olan `null` bir yenileyici fonksiyondur. Özelleştirilmiş geçişleri yapmak istediğimizde işe yarar. Bazen kendi replacer fonksiyonumu yazmak zorunda kaldım. Bunun nedeni de bazen object property'lere spesifik bir şekilde ulaşmak istememdi. Ayrıca daha popüler olan kullanım senaryosu ise circular structure problemini çözmektir. DOM elementi bu tarz structure için mükemmel bir örnektir. Her DOM elementi, alt öğesini (children) işaret eden üst elementini işaret eder ve bu şekilde devam eder.

`JSON.stringify` ve `JSON.parse` ile yapılacak bir başka trick de obje klonlamaktır. Örnek olarak:

```javascript
const user1 = {
  name: "Molecule Man",
  age: 29,
  secretIdentity: "Dan Jukes",
  powers: ["Radiation resistance", "Radiation blast"],
};
const user2 = JSON.parse(JSON.stringify(user1));

console.log(user1 === user2); // false
```

Bu işlemin her zaman başarıya ulaştığını söyleyemeyiz. Eğer objenin yukarıda da belirttiğimiz gibi circular bağımlılıkları varsa işlem başarısız olur. 

---

## 12. Object.assign

![Object.assign](https://50tips.dev/tip-assets/12/art.jpg)

Sık kullandığım bazı API'lar var. `Object.assign` bunlardan biri. Benim için genellikle kullanım durumu çeşitli property'ler ile birlikte yeni bir obje oluşturmaktır. İşte temel bilgiler: 

```javascript
const skills = { JavaScript: true, GraphQL: false };
const user = { name: 'Krasimir' };

const profile = Object.assign({}, user, skills);
// { "name":"Krasimir", "JavaScript":true, "GraphQL":false }
```

Bu metodun yararlı olduğu bir kaç yön var. Örnek olarak bir field'a default değer atayıp ardından onu overwrite edebiliriz:

```javascript
const defaults = { JavaScript: false, GraphQL: false, name: 'unknown' };
const skills = { JavaScript: true };

const profile = Object.assign(defaults, skills);
// { "JavaScript":true, "GraphQL":false, "name":"unknown" }
```

`assign` metodu falsy argümanları yok sayar, böylece bir field'ı yalnızca eğer tek satırlıysa ekleyebiliriz.

```javascript
function createUser(accessToken) {
  const user = Object.assign(
    { name: "Krasimir" },
    accessToken && { accessToken }
  );
  return user;
}

createUser('xxx'); // { name:"Krasimir", accessToken:"xxx" }
createUser(); // { name:"Krasimir" }
```

Bu API'yi genel olarak güvenlik data işlemleri için özellikle de data eksik veya tamamlanmış olduğunda kullanıyorum.

---

## 13. Capture Groups

![Capture Groups](https://50tips.dev/tip-assets/13/art.jpg)

Çoğu programlama dilinde olduğunda JavaScript de regular expression'ları desteklemektedir. 15 yıldan uzun süredir yazılım geliştiriyorum ve regular expression alanında hiç bir zaman iyi olmadım fakat benim favori özelliğimi paylaşmak istiyorum: capture groups. Capture groups, bir çok problemi tek bir satır kodla çözebilir.

Geçenlerde bir dosyanın ismini `script.js`'ten `script.prod.js`'e çevirdim. Capture groups bunun için mükemmel bir örnek: 

```javascript
function renameFile(file, postfix) {
  return file.replace(
    /(\.(js|ts|jsx|tsx))/i,
    `.${postfix}$1`
  );
}

console.log(renameFile("public/js/script.js", "prod"));
// public/js/script.prod.js
```

`(\.(js|ts|jsx|tsx))` bu kod `.js` ile eşleşen capture group'u ifade eder. Daha sonra `replace` metodunun ikinci argümanı `$1` olarak görüyoruz. `$2` ise ikinci capture group'ı içerecektir ve böyle devam edecektir.

---

## 14. Etiketli Template Literal

![Tagged template literal](https://50tips.dev/tip-assets/14/art.jpg)

Önceden JavaScript'te çok satırlı string ifadeler yazamıyorduk. Tek satırlı ifadeler birleştirmek zorunda kaldık. Daha sonra ise template literal tanıtıldı ve birdenbire hayatımız daha kolaylaştı.

```javascript
const what = "dummy text";
const bigString = `
  Lorem Ipsum is simply ${what} of the
  printing and typesetting industry.
`;
```

String manipülasyonlarını açısından, template literal büyük ihtimalle JavaScript'te son bir kaç yıldaki en iyi şeydir.

Template literal'ler yardımcıdır fakat etiketli template literal'ler (`tagged template literals`) daha da iyidir.  String'i işleyecek bir fonksiyon tanımlamamıza izin vermektedir. Bu fonksiyon yer tutucudaki (`placeholder`) tüm metni ve ifadeleri kabul eder. Aşağıdaki örneği kontrol edin. Yer tutucuda, primitive (ilkel tip) yerine bir fonksiyon iletiyoruz. Bu fonksiyon bir `theme` objesiyle tetiklenir.

```javascript
const theme = {
  brandColor1: "#BE2525",
  brandColor2: "#BE0000",
};
function css(strings, ...values) {
  return strings.reduce((res, str, i) => {
    return res + str + (values[i] ? values[i](theme) : "");
  }, "");
}
const styles = css`
  font-size: 1.2em;
  color: ${(theme) => theme.brandColor2};
`;
console.log(styles);
// font-size: 1.2em;
// color: #BE0000;
```

JavaScript topluluğu bu özelliği karmaşık ayrıştırma (`parse`) işlemlerinin gerekli olduğu yerde kullanır. Etiketli template kullanan bir çok kütüphane vardır. En popülerleri ise CSS-in-JS çözümleridir. CSS kodumuz JavaScript içerisinde tagged template literal olarak tutulur. Aynı yukarıdaki örnekte olduğu gibi. 

---

## 15. Media Query List

![Media Query List](https://50tips.dev/tip-assets/15/art.jpg)

Responsive tasarım ile ilgili ilk makaleyi 2010 yılında (Ethan Marcotte tarafından) gördüm. O zamandan beri Web gelişmeye ve geliştiriciler de web siteleri yapmaya devam etti. Bu günlerde ise responsive mimari bir opsiyon değil, zorunluluktur. 

Responsive tasarım geliştirmek için birden fazla araç var. Büyük ihtimalle de en başındaki media query'lerdir. Bu, elementlerimizin altında farklı davranan veya/ve görünen bir tanımlama yapabildiğimiz CSS özelliğidir. Örnek olarak:

```css
body {
  background-color: blue;
}
@media screen and (max-width: 800px) {
  body {
    background-color: red;
  }
}
```

`<body>` etiketinin varsayılan arkaplan rengi mavidir. Ancak eğer tarayıcının genişliği 800px'den düşük olursa bu sefer arkaplan rengi kırmızı olacaktır.

Uzun bir zaman, bu özellik yalnızca CSS'te vardı. Bu durum artık devam etmiyor. Artık `MediaQueryList` API'a sahibiz:

```javascript
const mediaQueryList = window.matchMedia(
  "(max-width: 800px)"
);
const handle = (mql) => {
  if (mql.matches) {
    console.log("width <= 800px");
  } else {
    console.log("width > 800px");
  }
};

mediaQueryList.addEventListener("change", handle);
handle(mediaQueryList);
```

Kendi kriterimize göre bir media query listesi objesi oluşturuyoruz. Ardından, kriterimiz karşılandığında bize söyleyecek bir dinleyici (`listener`) ekleyebiliriz.

Bu API ile birlikte, tamamen responsive uygulamalar yapabiliriz. Sadece nasıl göründüğüne değil, nasıl çalıştığına da odaklanabiliriz.

---

## 16. Event Delegation

![Event Delegation](https://50tips.dev/tip-assets/16/art.jpg)

Teknoloji geliştikçe bazı şeyleri nasıl unutmaya başladığımızı çok ilginç buluyorum. Unuttuğumuz şeylerden bir tanesi de event delegation. Günümüzde genellikle framework kullandığımız için event bubbling ve event capturing bir problem değil. Bu güvendiğimiz araçlar bu süreçleri kendimiz yönetmektedir. Frontend developer pozisyonları için yapılan mülakatlarda sorulan başlıca sorulardan biri olduğunu hatırlıyorum. Ve bu konuya bir bölüm ayırmayı karar verdim. 

Aşağıdaki örneğe bakın:

```html
<div id="container">
  <header>Hello</header>
  <button id="button">call to action</button>
</div>

<script>
document
  .querySelector("#button")
  .addEventListener("click", () => {
  // ...
});
</script>
```

Sayfa yüklendiğinde, `<button>` elementine bir listener ekliyoruz. DOM'a değiştirmediğimiz sürece iyi çalışır. `container`'ın içeriğini değiştir değiştirmez, listener'ı  yeniden eklemeliyiz. Şu anda aptalca bir problem görülebilir fakat bir kaç sene önce büyük bir problemdi. 

Eğer saf JavaScript yazıyorsak ve DOM elementlerini manipüle ediyorsak, listener'ların hazır bulunduğuna emin olmamız gerekir. Çözümlerden bir tanesi ise değişmeyen bir üst elemente listener eklemektir. Bizim senaryomuzda bu `<div id="container">`'dır. Ardından `event.target` ile event'in nereden geldiğini güvenli bir şekilde öğrenebiliriz. 

```javascript
document
  .querySelector("#container")
  .addEventListener('click', (event) => {
  // event.target....
});
```

Bu event delegation sayesinde çalışır. Eğer capture yoksa, her elementten gelen event bubble işlemi gerçekleştirir ve biz de onu yakalarız.

---

## 17. Error Handling

![Error Handling](https://50tips.dev/tip-assets/17/art.jpg)

Developer'ların sıklıkla unuttuğu bir şey var: error handling. Benim için düzgün error handling'in iki kuralı var. Birincisi error'u doğru yerde işlemek. Try-catch bloğu ve log yazdırma mantıklı değil. İkinci kural ise hatanın gerekli bilgiyi tutup hemen tepki vermesidir. 

İlk kuralı göstermek için bir `button`'a tıklandığında fetch isteği tetikleyen bir yapımız olduğunu varsayalım.

```jsx
// somewhere in our services layer
async function getAllProducts() {
  try {
    const res = await fetch('/api/products');
    return res.json();
  } catch(error) {
    console.log('Ops! Something happen.');
  }
}
// in a React Component
<button onClick={async () => {
  const data = await getAllProducts();
  // render the data
}}>
  All products
</button>
```

Çalışır ama kullanışlı değil. Eğer istek başarısız olursa ve hatayı yakalarsak, servisimizde çok bir şey yapmamız olacağız. Diğer bir yandan eğer try-catch bloğu React component'inin içerisinde olursa, kullanıcıya bir mesaj gösterebileceğiz.

```jsx
// somewhere in our services layer
async function getAllProducts() {
  const res = await fetch('/api/products');
  return res.json();
}
// in a React Component
<button onClick={async () => {
  try {
    const data = await getAllProducts();
    // render the data
  } catch(error) {
    // render error message
  }
}}>
  All products
</button>
```

İkinci kuralı yerine getirmek için özelleştirilmiş hata tipi oluşturmayı seviyorum. Hatayı işlerken mesaja güvenmek yerine tipini kullanmak çok daha iyidir. Örnek olarak:

```javascript
class NoEmailError extends Error {
  constructor() {
    super("Missing email");
    this.name = "NoEmailError";
  }
}
function validatePayload(data) {
  if (!data.email) throw new NoEmailError();
  return true;
}
(function handler() {
  try {
    validatePayload({ name: "Jules" });
  } catch (err) {
    if (err instanceof NoEmailError) {
      console.log(`Error: ${err.message}`);
    }
  }
})();
```

Hatayı işleme, uygulamamızda işler yolunda gitmeğinde bile çalışmasını garanti eder. Geliştirme sürecinin bu bölümünü hafife almamak gerekir. Tekrardan, hatayı doğru yerde ve doğru bağlamda ele almalıyız. 

---

## 18. Eski Zamanlardan Bir Anı

![Blast From the Past](https://50tips.dev/tip-assets/18/art.jpg)

Bir zamanlar panoya kopyalama işlemi küçük bir Flash uygulaması ile yapılırdı. Günümüzde ise bunun için bir API bulunmaktadır:

```javascript
// Panoya yazı kopyalama
async function copyToClipboard(text) {
  return navigator.clipboard.writeText(text);
}
// Kopyalanan yazıyı getirme
async function pasteFromClipboard() {
  return navigator.clipboard.readText();
}
```

Flash ile karşılaştığımız bir diğer sorun da yönlendirme (routing) işlemleriydi. Flash oynatıcısı yalnızca bir eklenti olduğu için adres çubuğuna doğrudan erişim sağlayamıyorduk. Kullanıcı uygulamamızda ilerlediğinde ve sayfaları değiştirdiğinde, o an bulunduğu yere veya sayfaya paylaşılabilir bir bağlantı vermenin hiçbir yolu yoktu. Ardından **"derin bağlantı"** (deep link) terimi icat edildi. Bu, tarayıcının URL'sini düzenleme işlemiydi ve böylece her sayfa için benzersiz adreslere sahip olurduk. Günümüzde artık Flash kullanmıyoruz ve **History API**'sine sahibiz:

```javascript
window.onpopstate = function(event) {
  console.log(`State: ${JSON.stringify(event.state)}`) // Değişikliği dinleyip console'a yazdırıyoruz.
}

history.pushState({foo: "bar"}, "my title", "/foo/bar"); // URL'i değiştiriyoruz.
history.back(); // Yaptığımız işlemi geri alıyoruz.
```

Ayrıca depolama API'larına da sahip değildik. Genellikle çerezleri kullanabiliyorduk. Günümüzde kullanıcının tarayıcısında veri depolamanın birkaç farklı yolu bulunmaktadır. En popüler olanlardan bir tanesi **local storage(yerel depolama)**'dır.

```javascript
localStorage.setItem("foo", "bar"); // Key-value şeklinde kaydediyoruz.
// ...
const value = localStorage.getItem("foo"); // Key değeriyle value'yu alabiliyoruz.
console.log(value); // bar
```

Ayrıca, localStorage'a benzer olan ancak verileri yalnızca mevcut oturum için saklayan **sessionStorage** da bulunmaktadır. Kullanıcı sekmesini veya tarayıcıyı kapatırsa, verileri silinir. Buna karşılık localStorage, JavaScript veya kullanıcı tarafından manuel olarak silinene kadar verileri saklar.

Daha fazla depolama alanına ihtiyaç duyduğumuz ise IndexedDB API'ı karşımıza çıkıyor. Bu, büyük miktarda veri saklamak için düşük seviyeli bir API'dir. IndexedDB, işlem tabanlı bir veritabanı sistemidir ve SQL'ye benzerdir. İşte hiç de kısa olmayan bir örnek:

```javascript
const indexedDB = window.indexedDB || window.mozIndexedDB || window.webkitIndexedDB || window.msIndexedDB || window.shimIndexedDB;

const request = indexedDB.open("UserProfiles", 1);

request.onupgradeneeded = function() {
  const db = request.result;
  const store = db.createObjectStore("User", { keyPath: "id" });
  const index = store.createIndex("NameIdx", ["name", "age"]);
};

request.onsuccess = function() {
  const db = request.result;
  const tx = db.transaction("User", "readwrite");
  const store = tx.objectStore("User");
  const index = store.index("NameIdx");
  store.put({id: 1234, name: "Steve", age: 32});
  store.put({id: 1235, name: "Peter", age: 34});
  
  const getUser = store.get(1234);
  getUser.onsuccess = function() {
    console.log(getUser.result.name);  // => "Steve"
  };

  tx.oncomplete = function() {
    db.close();
  };
}
```

---

### 19. Asenkron Derhal Çağrılan Fonksiyon İfadesi

![Asynchronous Immediately Invoked Function Expression](https://50tips.dev/tip-assets/19/art.jpg)

Bu kitabın başında, async/await sözdizini üzerine değinmiştik. Bu API'ın JavaScript diline geldiği zamanı hatırlıyorum. Kodumuz biraz daha okunaklı ve yönetilebilir hale geldi. Ancak bir istisna var - global kapsamda await kullanamayız. Bu tür durumlarda, kendiliğinden çağrılan bir async fonksiyon kullanmamız gerekmektedir.

```javascript
const url = "https://api.thecatapi.com/v1/images/search";
(async function getData() {
  const res = await fetch(url);
  const data = await res.json();
  console.log(data[0].url); // cat image url
})();
```

Bu desen aynı zamanda IIFE (Immediately Invoked Function Expression - Hemen Çağrılan İşlev İfadesi) olarak da bilinir. Bu desen, çalıştırılacak kodu kendi özel kapsamına almak istediğimiz durumlar için faydalıdır. Yukarıdaki örnekte, res ve data sabitleri sadece getData işlevi içinde erişilebilirdir.

*(Kullandığınız JavaScript motoru üst düzey await'i destekliyor olabilir. Bu nedenle kendiliğinden çağrılan bir fonksiyona ihtiyaç duymayabilirsiniz.)*

---

### 20. Asenkron Kuyruk

![Asynchronous queue](https://50tips.dev/tip-assets/20/art.jpg)

Bazen kütüphanelerin zaten çözdüğü sorunları çözmeyi severim. Bu sorunlardan biri, ardışık olarak birden fazla asenkron (eşzamanlı) işlemi ele almak. Diğer bir deyişle, rastgele süreleri olan istekleri kabul edip, tüm sonuçları döndüren bir API'ye sahip olmaktır. Örneğin:

```javascript
const queue = createQueue();

function nationality(name) {
  return () => fetch(`https://api.nationalize.io/?name=${name}`)
      .then((res) => res.json())
      .then((data) => `${name}: ${data.country[0].country_id}`);
}

queue.add(nationality("Krasimir"));
queue.add(nationality("Natalie"));
setTimeout(() => {
  queue.add(nationality("Hans"));
}, 10);
queue.done.then(console.log);

queue.execute();
// Bir süre sonra: [ 'Krasimir: BG', 'Natalie: GB', 'Hans: FO' ]
```

`nationality` isimli bir fonksiyon, bir isim alır ve bu ismin kaynağını sorgulamak için `api.nationalize.io`'ya başvurur. Bu, bir promise döndürür. Tüm promise'leri bir kuyruğa ekliyoruz. Tüm promise'ler yerine getirildiğinde sonucu alacağız.

İşte bu asenkron kuyruk için benim entegrasyonum:

```javascript
function createQueue() {
  let tasks = [], results = [], processing = false, done = () => {};
  const handle = (res) => {
    results.push(res);
    processing = false;
    execute();
  };
  function execute() {
    if (tasks.length > 0 && !processing) {
      processing = true;
      tasks.shift()().then(handle).catch(handle);
    } else if (tasks.length === 0 && !processing) {
      done(results);
    }
  }
  function add(task) { tasks.push(task); }
  return {
    add,
    execute,
    done: new Promise((cb) => (done = cb))
  }
}
```

Bu fonksiyon, görev listesini tanımlar ve bu görevlerin ne zaman tamamlandığını izler. Bir işlem bittiğinde, bekleyen promise'lerin olup olmadığını kontrol ederiz. Yoksa, işin tamamlandığını kabul ederiz. Şu anda birbirini bekledikleri için istekleri paralel olarak çalıştırmak daha iyi bir geliştirme olabilir.

---

### 21. JavaScript Modül Sistemi Olarak Bir Singleton

![JavaScript module system as a singleton](https://50tips.dev/tip-assets/21/art.jpg)

JavaScript'te farklı kapsam (scope) türleri bulunmaktadır. Bunlardan biri modül kapsamıdır. Bir dosyanın en üstündeki bir değişken bu kapsama dahil olur. Node'da böyle bir değişken; fonksiyon, sınıf veya blok kapsamının bir parçası değildir. Bu anlamda, kodumuzu paketlediğimizde client tarafında yeniden oluşturulur. Bu değişkenlere dosya/modül dışından erişilemez. Onları dışa aktarmamız (export) gerekir. Bir diğer özellikleri ise önbelleğe (cache) alınmış olmalarıdır. Yani modülü ne kadar çok import/require işlemi yaparsak yapalım, en üst düzey kod yalnızca bir kez çalıştırılır. Bu, singleton desenini uygulamamıza olanak tanır (kitabın ilerleyen bölümlerinde bu konuyu daha derinlemesine ele alacağız).

Diyelim ki aşağıdaki içeriğe sahip `registry.js` adlı bir dosyamız var.

```javascript
const users = [];
module.exports = {
  register(name) {
    users.push({ name });
  },
  total() {
    return users.length;
  },
};
```

Kullanıcıları en üst düzeyde tanımladık, bu nedenle önbelleğe alındı. Sadece bir kez başlatılacak. Şimdi `A.js` ve `B.js` adlı iki başka dosya/modül oluşturalım ve bunlar `registry.js` dosyasını içe aktarsın.

```javascript
// A.js
const R = require("./registry");
R.register("Dave");
module.exports = R;

// B.js
const R = require("./registry");
R.register("Alex");
module.exports = R;
```

Her iki dosyadaki `R` sabiti aynı nesnedir. İşte kanıtı: Üç dosyayı da içe aktaracak şekilde `index.js` oluşturacağız. Bu, `registry.js` modülünden `total` fonksiyonunu çağıracak.

```javascript
const A = require("./A");
const B = require("./B");
const { total } = require("./registry");

console.log(total()); // 2
console.log(A === B); // true
```

İlk log, `users` dizisini yalnızca bir kez oluşturduğumuzu ve `register` fonksiyonunu çağırdığımızda aynı örneği kullandığımızı kanıtlar. İkinci log, `registery` içindeki dışa aktarılan (export) edilen değerin önbelleğe alındığını gösterir.

---

### 22. Call-to-action Eklentilerini Script Etiketiyle Değiştirme

![Call-to-action widgets script tag replacement](https://50tips.dev/tip-assets/22/art.jpg)

Hiç call-to-action eklentilerinin nasıl çalıştığını merak ettiniz mi? Bilirsiniz, bu küçük butonlar sosyal ağlarda içeriği paylaşmak içindir. Genellikle `iframe` olarak entegre edilirler, ancak bazen script etiketini kopyala/yapıştır yapmamız gerekir. Ve dökümanda şöyle der: "Butonu nerede görmek istiyorsanız aşağıdaki kodu yerleştirin.". Ancak eğer bu bir script etiketi ise, butonu nereye yerleştireceğini nasıl bilir? Bu bölümün amacı, bu sorunun cevabını size vermek.

Aşağıdaki işaretlemeyle başlayalım:

```javascript
<script src="./widget.js" data-color="#ff0000"></script>
<section>
  İçerik burada
</section>
<script src="./widget.js" data-color="#00ff00"></script>
```

Görmek istediğimiz şey; bir bağlantı, ardından "İçerik burada" ve başka bir bağlantıdır. Sadece bu script etiketlerini değiştirmek istemiyoruz, aynı zamanda her biri için farklı sonuçlar elde etmek istiyoruz. Butonun rengi farklı olmalıdır.

`widget.js` dosyasının arkasındaki kodun oldukça kısa olabileceğini öğrenmek beni şaşırttı. Sadece sekiz satır:

```javascript
(async function loadContent() {
  const options = document.currentScript.dataset;
  const node = document.createElement('div');
  const style = options.color ? `color:${options.color}` : '';

  node.innerHTML = `<a href="http://mysite.com" style="${style}">click me</a>`;
  document.currentScript.parentNode.replaceChild(node, document.currentScript);
})();
```

Burada kullanılan API'lar `document.currentScript` ve `element.dataset`'tir. İlk API, şu anda işlediğimiz scripte erişim sağlar. `dataset` özelliği ise öğenin data attribute'larına hızlı erişim sağlar.

Yukarıdaki kod kesiti yeni bir `div` oluşturur ve içine bir bağlantı ekler. Ardından `replaceChild` kullanarak script etiketini bu yeni oluşturulan öğeyle değiştirir. Sonuç şu şekilde:

```javascript
<div><a href="http://mysite.com" style="color:#ff0000">click me</a></div>
<section>
  Content here
</section>
<div><a href="http://mysite.com" style="color:#00ff00">click me</a></div>
```

---

### 23. Nesnelerden Alanları Çıkarma

![Removing fields from objects](https://50tips.dev/tip-assets/23/art.jpg)

Bu kitabın daha önceki bölümlerinde, destructing hakkında bilgi edindik. Bu özelliğin ilginç bir kullanım durumu, bir alanı nesneden kaldırmamıza izin vermesidir.

```javascript
function allButPoints(obj) {
  const { points, ...rest } = obj;
  return rest;
}

const user = {
  firstName: "David",
  lastName: "Bird",
  points: 231,
};
console.log(allButPoints(user));
// { firstName: 'David', lastName: 'Bird' }
```

React dünyasında bunu çokça kullanırım, burada bir component bir dizi props alır, ancak benim sadece birkaçını iletmem gerekebilir.





