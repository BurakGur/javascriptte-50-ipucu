# Strict Eşitliği

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
