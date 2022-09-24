## Strict Eşitliği

Uzun zamandır JavaScript yazıyorum ve öğrendiği başlıca şeylerden bir tane `==` (iki eşitlik) yerine `===` (üçlü eşitlik) kullanmaktır. Bu daha güvenli bir yöntemdir. Örneğin:

```javascript
[user.id](http://user.id) === user.id
```

yerine

```javascript
[user.id](http://user.id) == id
```

Kullandığımızda bir çok hatanın da kaynağı haline gelmektedir. Genellikle mantıksız görünen ifadelere denk geliyoruz. Örnek olarak boş bir string ile false boolen değerini karşılaştıralım:

```javascript
console.log("" == false); // true
```

Bu karşılaştırma `true`’dur çünkü == (çiftli eşitlik) kullandığımızda gevşek bir eşitlik kullanırız. Sonuç `true`’dur çünkü her iki değer de ortak bir type’a dönüştürülür ve öyle karşılaştırılır. Bu karşılaştırma daha çok şu eşitliğe benzer:

```javascript
console.log(Boolean("") === false); // true
```

JavaScript motoru type dönüşümünden sonra `===` (strirct equality)’yi kullanır. 

Yani üçlü eşitlik kodunuzu daha anlaşılır hale getirir ve sizi bazı garip hatalardan kurtarır.

---

## Virgül Operatörü (Comma Oparator)

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

## Spread Operatörü (Spread Operator)
![Spread Operator](https://50tips.dev/tip-assets/3/art.jpg)
Geçtiğimiz günlerde JavaScript'e gelen yeni özellikleri okumaya başladığımda beni en çok heyecanlandıran şey spread operatörü olmuştu. Çeşitli yerlerde genişletilebilir iterable (veya stringler) oluşturmamıza izin verir. Genellikle bu operatörü object oluştururken kullanırım. Örneğin: 

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

## Destructuring

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

## Opsiyonel Zincirleme (Optional Chaining)

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

## Referansa veya Değere Göre (By Reference or By Value)

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

## Reducing 

![Reducing](https://50tips.dev/tip-assets/7/art.jpg)

Genellike object'leri bir şekilde diğer şekle dönüştürmemiz gerekir. Bu tarz dönüşümler için benim favori aracım reduce fonksiyonlarıdır. Aşağıdaki gibi içerisinde object'leri içeren bir array'imizin olduğunu varsayalım:

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


