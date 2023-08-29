# Bölüm 7. JavaScript Tasarım Desenleri

Önceki bölüm, farklı tasarım deseni kategorilerinin üç farklı örneğini verdi. Bu tasarım desenlerinin bazıları web geliştirme bağlamında ilgili veya gereklidir. JavaScript uygulamalarında kullanıldığında yardımcı olabilecek bazı zamansız desenleri belirledim. Bu bölüm, farklı klasik ve modern tasarım deseni uygulamalarını JavaScript bağlamında keşfeder. Her bölüm, üç kategoriden birine - yaratıcı, yapısal ve davranışsal desenler - adanmıştır. Creational desenlerle başlayalım.

## Bir Desen Seçimi
Geliştiriciler genellikle çalışma akışlarında hangi desenin veya desenlerin ideal olduğunu merak ederler. Bu soruya tek bir doğru cevap yoktur; üzerinde çalıştığımız her betik ve web uygulamasının muhtemelen farklı bireysel ihtiyaçları olacaktır. Bir desenin gerçek değer sunup sunamayacağını düşünmeliyiz.
Örneğin, bazı projeler, Gözlemci deseninin sunduğu bağımsızlaşma faydalarından (uygulamanın bağımlı parçalarının birbirine olan bağımlılığını azaltır) yararlanabilir. Aynı zamanda, diğerleri bağımsızlaşmanın bir endişe olmadığı kadar küçük olabilir.
Ancak, tasarım desenlerinin ne işe yaradığını ve hangi özel problemlere en uygun olduğunu iyice anladığımızda, bu desenleri uygulama mimarilerimize entegre etmek çok daha kolay hale gelir.

## Yaratıcı Desenler
Yaratıcı desenler, nesneleri oluşturmak için mekanizmalar sağlar. Aşağıdaki desenleri ele alacağız.
- "Constructor Deseni"
- "Modül (Module) Deseni"
- "Açıklayan Modül Deseni"
- "Singleton Deseni"
- "Prototip Deseni"
- "Fabrika (Factory) Deseni"

Constructor Deseni
Bir yapıcı (constructor), bellekte önceden ayrılan bir bellek bölgesi için yeni oluşturulmuş bir nesneyi başlatmak için kullanılan özel bir yöntemdir. ES2015+ ile, yapıcıları kullanarak sınıfları oluşturmak için JavaScript'e tanıtıldı. Bu, sınıfın varsayılan yapıcısı kullanılarak bir nesnenin sınıf örneği olarak oluşturulmasını sağlar.
JavaScript'de neredeyse her şey bir nesnedir ve sınıflar, JavaScript'in prototip tabanlı miras yaklaşımı için sentaks şeklidir. Klasik JavaScript ile, genellikle nesne yapıcılarıyla ilgilenirdik. 

Nesne yapıcıları, belirli türde nesneler oluşturmak için kullanılır - nesneyi kullanıma hazırlarken ve nesne ilk oluşturulduğunda üye özelliklerinin ve yöntemlerinin değerlerini ayarlamak için argümanları kabul eder.

XX ŞEKİL

### Nesne Oluşturma
JavaScript'te yeni nesneler oluşturmanın üç yaygın yolu aşağıdaki gibidir:
```javascript
// Aşağıdaki seçeneklerin her biri yeni boş bir nesne oluşturur
const newObject = {};
// veya
const newObject = Object.create(Object.prototype);
// veya
const newObject = new Object();
```
Burada her nesneyi bir sabit olarak tanımladık, bu da salt okunur blok kapsamında bir değişken oluşturur. Son örnekte, Object yapıcısı belirli bir değer için bir nesne sarmalayıcısı oluşturur veya değer geçirilmediyse boş bir nesne oluşturur ve bunu döndürür.

Şimdi nesnelere aşağıdaki yöntemlerle anahtarlar ve değerler atayabilirsiniz:

```javascript
// ECMAScript 3 uyumlu yaklaşımlar
// 1. Nokta sözdizimi
// Özellikleri ayarla
newObject.someKey = "Merhaba Dünya";
// Özellikleri al
var key = newObject.someKey;

// 2. Kare parantez sözdizimi
// Özellikleri ayarla
newObject["someKey"] = "Merhaba Dünya";
// Özellikleri al
var key = newObject["someKey"];
// Yalnızca ECMAScript 5 uyumlu yaklaşımlar
// Daha fazla bilgi için: http://kangax.github.com/es5-
compat-table/


// 3. Object.defineProperty
// Özellikleri ayarla
Object.defineProperty( newObject, "someKey", {
value: "özellik davranışının daha fazla kontrolü için",
writable: true,
enumerable: true,
configurable: true
});

// 4. Yukarıdaki yöntem biraz okunması zorsa, kısa bir yol
şu şekilde yazılabilir:
var defineProp = function ( obj, key, value ){
config.value = value;
Object.defineProperty( obj, key, config );
};

// Kullanmak için, ardından yeni boş bir "kişi" nesnesi oluştururuz
var person = Object.create( null );

// Nesneyi özelliklerle doldur
defineProp( person, "car", "Delorean" );
defineProp( person, "dateOfBirth", "1981" );
defineProp( person, "hasBeard", false );

// 5. Object.defineProperties
// Özellikleri ayarla
Object.defineProperties( newObject, {
"someKey": {
value: "Merhaba Dünya",
writable: true
},
"anotherKey": {
value: "Foo bar",
writable: false
}
});
// 3. ve 4. için özellikleri almak, 1. ve 2. seçeneklerinden
// herhangi birini kullanarak yapılabilir.
```

Bu yöntemleri aşağıdaki gibi miras almak için bile kullanabilirsiniz:

```javascript
// ES2015+ anahtar kelimeleri/sözdizimi kullanıldı: const
// Kullanım:
// Kişiden miras alan bir yarış arabası sürücüsü oluştur
const driver = Object.create(person);
// Sürücü için bazı özellikleri ayarla
defineProp(driver, 'topSpeed', '100mph');
// Miras alınmış bir özelliği al (1981)
console.log(driver.dateOfBirth);
// Ayarladığımız özelliği al (100mph)
console.log(driver.topSpeed);
```

### Temel Yapıcılar
Daha önce Tartışıldığı gibi, Bölüm 5'te ele alındığı gibi, JavaScript sınıfları ES2015'te tanıtıldı, bu da JavaScript nesneleri için şablonlar tanımlamamıza ve JavaScript'i kullanarak kapsüllemeyi ve kalıtımı uygulamamıza olanak tanıdı.

Özetlemek gerekirse, sınıflar bir constructor() adında bir yöntem içermeli ve bildirmelidir, bu yöntem yeni bir nesne oluşturmak için kullanılacaktır. new anahtar kelimesi, kurucuyu çağırabilmemizi sağlar. Kurucu içindeki this anahtar kelimesi, oluşturulan yeni nesneyi işaret eder. Aşağıdaki örnek temel bir yapıcıyı göstermektedir.
```javascript
class Car {
  constructor(model, year, miles) {
    this.model = model;
    this.year = year;
    this.miles = miles;
  }
  
  toString() {
    return `${this.model} ${this.year} model has done ${this.miles} miles`;
  }
}

// Kullanım:
// Araba örneklerini oluşturabiliriz
let civic = new Car('Honda Civic', 2009, 20000);
let mondeo = new Car('Ford Mondeo', 2010, 5000);

// ve ardından tarayıcı konsolunu açarak bu nesneler üzerinde çağrılan toString() yönteminin çıktısını görüntüleyebiliriz
console.log(civic.toString());
console.log(mondeo.toString());
```
Yukarıdaki örnek basit bir yapıcı deseninin bir sürümüdür, ancak bazı sorunlarla karşılaşır. Birincisi, kalıtımı zorlaştırır ve diğeri, toString() gibi işlevlerin Car yapıcısını kullanarak oluşturulan her yeni nesne için yeniden tanımlanmasıdır. Bu, tüm Car türü örneklerinin ideal olarak aynı işlevi paylaşması gerektiği anlamına gelmez.

### Prototipli Yapıcılar (Constructor)

JavaScript'de prototipler, belirli bir nesnenin, işlevin veya sınıfın tüm örnekleri için kolayca yöntemler tanımlamanıza olanak tanır. Bir JavaScript yapıcısını bir nesne oluşturmak için çağırdığımızda, yapıcının prototipinin tüm özellikleri yeni nesneye kullanılabilir hale gelir. Bu şekilde aynı prototipe erişen birden fazla Car nesnesi oluşturabiliriz. Bu nedenle, özgün örneği aşağıdaki gibi genişletebiliriz:
```javascript
class Car {
  constructor(model, year, miles) {
    this.model = model;
    this.year = year;
    this.miles = miles;
  }
}

// Burada, prototip nesnesini yeniden tanımlamamak için Object.prototype.newMethod yerine Object.prototype kullanıyoruz
// Yeni yöntemler eklemek için hala Object.prototype kullanabiliriz,
// çünkü içeride aynı yapısı kullanılır
Car.prototype.toString = function() {
  return `${this.model} has done ${this.miles} miles`;
};

// Kullanım:
let civic = new Car('Honda Civic', 2009, 20000);
let mondeo = new Car('Ford Mondeo', 2010, 5000);
console.log(civic.toString());
console.log(mondeo.toString());
```
Yukarıda, tüm Car nesneleri artık toString() yönteminin tek bir örneğini paylaşacaklardır.

## Modül (Module) Deseni
Module deseni, güçlü bir uygulamanın mimarisinin ayrılmış ve düzenli olarak tutulmasına yardımcı olan önemli bir bileşendir.
Klasik JavaScript, modülleri uygulamanın birkaç seçeneği vardı, örneğin:
- Nesne anlık gösterimi (Object literal notation)
- Modül deseni (Module pattern)
- AMD modülleri
- CommonJS modülleri

Daha önce 5. Bölümde modern JavaScript modüllerini (aynı zamanda "ES modülleri" veya "ECMAScript modülleri" olarak da bilinir) tartıştık. Bu bölümdeki örneklerde özellikle ES modüllerini kullanacağız.
ES2015'ten önce CommonJS modülleri veya AMD modülleri, bir modülün içeriğini dışa aktarmanıza izin veren popüler alternatiflerdi. Kitabın ilerleyen bölümlerinde AMD, CommonJS ve UMD modüllerini ele alacağız (Bölüm 10). İlk olarak, Modül Deseni ve kökenlerini anlayalım.
Modül deseni, kısmen nesne anlık gösterimine dayandığı için önce onunla ilgili bilgilerimizi tazelemek mantıklı olacaktır.

### Nesne Değişmez Değerleri
Nesne anlık gösteriminde, bir nesne küme içerisinde ({} süslü parantezler içinde) virgülle ayrılmış ad/değer çiftleri olarak açıklanır. Nesnenin içindeki adlar ya dize ya da kimlikleri takiben bir iki nokta üst üste olabilir. Nesnenin son ad/değer çiftinden sonra virgül kullanmamaya dikkat etmelisiniz, çünkü bu hatalara neden olabilir.
```javascript
const myObjectLiteral = {
    variableKey: variableValue,
    functionKey() {
        // ...
    }
};
```
Nesne anlık gösterimleri, new operatörü kullanılarak oluşturulmayı gerektirmez, ancak { açılış parantezi yeni bir bloğun başlangıcı olarak yorumlanabileceği için bir ifadenin başında kullanılmamalıdır. Bir nesnenin dışında, yeni üyeler şu şekilde atama kullanılarak eklenir:
```javascript
myModule.property = "someValue";
```

### Modül (Module) Deseni

Module deseni başlangıçta geleneksel yazılım mühendisliğinde sınıflar için özel ve genel kapsüllemeyi sağlamak amacıyla tanımlandı. 

Bir noktada, makul bir boyuttaki herhangi bir JavaScript uygulamasını düzenlemek zor bir görevdi. Geliştiriciler, yeniden kullanılabilir mantık parçalarını bölmek ve yönetmek için ayrı betiklere güvenirdi ve mantıklı bir düzeni korumak için bir HTML dosyasına manuel olarak 10-20 betik eklemek şaşırtıcı değildi. Nesneleri kullanarak, modül deseni mantığı hem genel hem de "özel" yöntemlerle bir dosyada kapsüle etmenin bir yoluydu. Zaman içinde, bunu daha sorunsuz hale getiren birkaç özel modül sistem ortaya çıktı. Şimdi geliştiriciler, JavaScript modüllerini kullanarak nesneleri, işlevleri, sınıfları veya değişkenleri düzenleyebilir, böylece bunlar kolayca diğer dosyalara aktarılabilir veya içe aktarılabilir. Bu, farklı modüllerde bulunan sınıf veya işlev adları arasındaki çakışmaları önlemeye yardımcı olur. Aşağıdaki şekil modül desenini göstermektedir.

XX ŞEKİL

#### Gizlilik
Modül deseni, kapanışları kullanarak "gizlilik" durumunu ve organizasyonu kapsüller. Bu, genel kapsamda sızmasını ve başka bir geliştiricinin arayüzü ile yanlışlıkla çakışmasını önlemek için genel ve özel yöntemleri ve değişkenleri karıştırma yolunu sağlar. Bu desenle sadece genel API'yi açığa çıkarırken, geri kalan her şeyi sarmalayan bir kapanış içinde özel tutarsınız.

Bu, koruyucu mantığı ağır kaldırma işlemi yaparken sadece uygulamanın diğer kısımlarının kullanmasını istediğimiz bir arayüzü açığa çıkardığımız temiz bir çözüm sağlar. Desen, hemen çağrılan bir işlev ifadesi kullanır, böylece bir nesne döndürülür.
Unutulmamalıdır ki JavaScript içinde tam anlamıyla "gizlilik" kavramı bulunmaz, çünkü bazı geleneksel dillerin aksine erişim belirleyicileri yoktur. Teknik olarak değişkenleri genel veya özel olarak bildiremezsiniz, bu yüzden bu kavramı taklit etmek için işlev kapsamını kullanırız. Modül deseni içinde bildirilen değişkenler veya yöntemler, kapanış sayesinde sadece modül içinde kullanılabilir. Ancak, dönen nesnenin içinde tanımlanan değişkenler veya yöntemler herkese açıktır.

Dönen nesnede değişkenlerin gizliliğini uygulamak için WeakMap() kullanmak, bu bölümde Modern Modül Deseni ile daha sonra ele alınacaktır. WeakMap() sadece nesneleri anahtar olarak kabul eder ve yinelemesi mümkün değildir. Bu nedenle, bir modül içindeki nesneye yalnızca referansı aracılığıyla erişilebilir. Modülün dışında, yalnızca içinde tanımlanan bir genel yöntemi aracılığıyla erişilebilir. Böylece nesne için gizliliği sağlar.

#### Tarihçe
Tarihsel bir perspektiften bakıldığında, Modül deseni ilk olarak 2003 yılında Richard Cornford ve diğer birkaç kişi tarafından geliştirildi. Douglas Crockford daha sonra bu deseni derslerinde popülerleştirdi. İlginç bir diğer ayrıntı ise, Yahoo'nun YUI (Yahoo User Interface) kütüphanesiyle oynamışsanız bazı özelliklerinin oldukça tanıdık gelebileceğidir. Bunun nedeni, Modül deseninin YUI'nin bileşenlerini oluştururken güçlü bir etki yaratmış olmasıdır.

#### Örnekler
Modül desenini uygulamaya başlayarak bir kendi kendine yeten modül oluşturmayı incelemeye başlayalım. Uygulamamızda import ve export anahtar kelimelerini kullanıyoruz. Önceki tartışmamızı tekrar hatırlayacak olursak, export, modül özelliklerine modül dışından erişim sağlamanızı sağlar. Aynı zamanda import, bir modül tarafından dışa aktarılan bağlamaları betiğimize içe aktarmamıza olanak tanır.
```javascript
let counter = 0;
const testModule = {
  incrementCounter() {
    return counter++;
  },
  resetCounter() {
    console.log(`reset öncesindeki sayacın değeri: ${counter}`);
    counter = 0;
  },
};

// İsimsiz varsayılan dışa aktarma modülü
export default testModule;

// Kullanım:
// Yolu belirterek modülü içe aktarın
import testModule from './testModule';

// Sayacımızı artırın
testModule.incrementCounter();

// Sayacın değerini kontrol edin ve sıfırlayın
// Çıktı: reset öncesindeki sayacın değeri: 1
testModule.resetCounter();
```
Burada kodun diğer bölümleri, incrementCounter() veya resetCounter() değerini doğrudan okuyamaz. counter değişkeni tamamen küresel kapsamımızdan korunur, böylece adeta özel bir değişken gibi davranır. Varlığı yalnızca modülün kapanışı içerisine sınırlıdır, böylece bu iki fonksiyonun kapsamına erişebilen tek kod onların kendileridir. Yöntemlerimiz etkin bir şekilde ad alanlarıdır, bu nedenle kodumuzun test bölümünde herhangi bir çağrıyı modülün adıyla (örneğin, testModule) öne eklememiz gerekmektedir.

Modül deseniyle çalışırken, onunla başlamak için kullanabileceğimiz basit bir şablon tanımlamanın faydalı olabileceğini görebiliriz. İşte ad alanlarını, genel ve özel değişkenleri kapsayan bir örnek:
```javascript
// Özel bir sayaç değişkeni
let myPrivateVar = 0;

// Herhangi bir argümanı günlüğe kaydeden özel bir işlev
const myPrivateMethod = foo => {
  console.log(foo);
};

const myNamespace = {
  // Genel bir değişken
  myPublicVar: 'foo',

  // Privateleri kullanan genel bir işlev
  myPublicFunction(bar) {
    // Özel sayaç değişkenimizi arttırın
    myPrivateVar++;

    // bar'ı kullanarak özel metodumuzu çağırın
    myPrivateMethod(bar);
  },
};

export default myNamespace;
```

Aşağıda başka bir örneği incelediğimizde, bu deseni kullanarak uygulanan bir alışveriş sepetini görebiliriz. Modül kendisi basketModule adlı global bir değişkende tamamen kapsayıcı olarak yer alır. Modüldeki sepet dizisi gizlidir, bu nedenle uygulamanın diğer parçaları doğrudan okuyamaz. Yalnızca modülün kapanışı içinde var olur ve bu nedenle sadece addItem(), getItem(), vb. gibi kapsama erişimi olan yöntemler tarafından erişilebilir.

```javascript
// Gizli değişkenler
const sepet = [];
const gizliBirSeyYap = () => {
  //...
};
const baskaBirGizliSeyYap = () => {
  //...
};

// Kamuya açık bir nesne oluştur
const sepetModulu = {
  // Sepete öğeler ekleyin
  ozelikEkle(degerler) {
    sepet.push(degerler);
  },
  
  // Sepetteki öğelerin sayısını alın
  ozelikSayisiAl() {
    return sepet.length;
  },
  
  // Kamuya açık bir işlev olarak gizli bir işlemin takma adı
  birSeyYap() {
    gizliBirSeyYap();
  },
  
  // Sepetteki öğelerin toplam değerini alın
  // reduce() yöntemi, bir birikimleyiciye ve dizideki
  // her öğeye (soldan sağa doğru) bir işlev uygular
  // ve bu değeri tek bir değere indirir.
  toplamAl() {
    return sepet.reduce((gecerliToplam, ozelik) => ozelik.fiyat + gecerliToplam, 0);
  },
};

export default sepetModulu;

```

Modül içinde, bir nesne döndürdüğümüzü fark etmiş olabilirsiniz. Bu otomatik olarak `basketModule`'a atanır, böylece aşağıdaki gibi etkileşime girebiliriz:
```javascript
// Yolu üzerinden modülü içe aktar
import basketModule from './basketModule';
// basketModule, kullanabileceğimiz bir kamuya açık API'ye sahip bir nesne döndürür
basketModule.addItem({
  item: 'ekmek',
  price: 0.5,
});
basketModule.addItem({
  item: 'tereyağı',
  price: 0.3,
});
// Çıktı: 2
console.log(basketModule.getItemCount());
// Çıktı: 0.8
console.log(basketModule.getTotal());
// Ancak aşağıdaki çalışmayacaktır:
// Çıktı: tanımlanmamış
// Bu, sepetin kendisinin kamuya açık bir bölüm olarak
// dışa aktarılmadığı anlamına gelir
console.log(basketModule.basket);
// Aynı şekilde bu da çalışmayacaktır çünkü bu, yalnızca
// basketModule kapsamının içinde mevcuttur, ancak
// döndürülen kamuya açık nesnede değil
console.log(basket);
```
Yukarıdaki yöntemler etkili bir şekilde `basketModule` içinde isim alanında bulunur. Tüm işlevlerimiz bu modülün içine sarılır, bize çeşitli avantajlar sağlar, örneğin:

- Yalnızca modülümüz tarafından tüketilebilen özel işlevlere sahip olma özgürlüğü. Bu işlevler sayfa geri kalanına (sadece ihracat yaptığımız API) açık değildir, bu yüzden gerçekten özeldirler.

- İşlevler genellikle bildirildiği ve adlandırıldığı için hata ayıklamada hangi işlev(ler)in istisna attığını bulmaya çalışırken çağrı yığınlarını daha kolay göstermek mümkün olabilir.

- Daha önce T.J. Crowder tarafından belirtildiği gibi, bu aynı zamanda çevreye bağlı olarak farklı işlevleri döndürmemize olanak tanır. Geçmişte geliştiricilerin, IE için özel bir kod yolu sağlamak için bu yöntemi kullandığını gördüm, ancak günümüzde benzer bir amaca ulaşmak için özellik tespiti seçebiliriz.

## Modül Deseni Varyasyonları
Zaman içinde tasarımcılar ihtiyaçlarına uygun farklı modül deseni varyasyonları tanıttılar.

**Import karıştırıcıları (Import Mixins)**

Bu desen varyasyonu, modülün anonim fonksiyonuna global değişkenleri (örneğin, jQuery, Underscore) argüman olarak ileterek nasıl kullanabileceğimizi gösterir. Bu, bunları istediğimiz gibi yerel olarak içe aktarabilir ve takma adlandırabiliriz.

```javascript
const privateMethod1 = () => {
  $(".container").html("test");
};

const privateMethod2 = () => {
  console.log(_.min([10, 5, 100, 2, 1000]));
};

const myModule = {
  publicMethod() {
    privateMethod1();
  },
};

// Default export module, without name
export default myModule;
```

```javascript
// Yol üzerinden modülü içe aktar
import myModule from './MyModule';

myModule.publicMethod();
```

Bu örnekte, jQuery'yi ve Underscore'ı modülün içine almak için argümanlar olarak geçirdik ve fonksiyonlar içinde bu kütüphaneleri kullanabiliriz. Böylece, bu kütüphaneleri modülün içinde lokal olarak kullanabiliriz.

**Exportlar**

Bu son varyasyon, global değişkenleri tüketmeden bildirmemize ve son örnekte görülen global içe aktarma kavramını benzer şekilde desteklememize olanak tanır.

```javascript
// Modül nesnesi
const module = {};

const privateVariable = 'Merhaba Dünya';

const privateMethod = () => {
  // ...
};

module.publicProperty = 'Foobar';

module.publicMethod = () => {
  console.log(privateVariable);
};

// Default export module, without name
export default module;
```

Bu örnekte, `module` adlı bir nesne kullanarak hem özel hem de genel değişkenleri ve yöntemleri tanımladık. Bu nesneyi modül olarak dışa aktardık. Bu şekilde, modül içindeki tüm özelliklere ve yöntemlere dışarıdan erişilebilir. Aynı zamanda, `privateVariable` gibi özel değişkenler yalnızca modül içindeki yöntemler tarafından erişilebilir ve dışarıya sızdırılmaz.

**Avantajlar**
Constructor deseninin neden faydalı olabileceğini gördük, ancak Modül deseni neden iyi bir seçenek? İlk olarak, bu desen, nesne tabanlı bir arka plana sahip geliştiriciler için gerçek kapsülleme fikrinden daha temiz bir yaklaşımdır, en azından bir JavaScript bakış açısıyla.

İkinci olarak, Modül deseni özel verileri destekler. Yani, Modül deseninde sadece `export` anahtar kelimesini kullanarak açıkça dışa aktardığımız değerlere erişim sağlarız. Açıkça dışa aktarmadığımız değerler özeldir ve yalnızca modül içinde kullanılabilir. Bu, küresel kapsülleme riskini azaltır. Modülünüzü kullanan geliştiricilerin aynı adı taşıyan özel değerleri kazara üzerine yazma riskinden korkmanıza gerek yoktur. Bu, ad çakışmalarını ve küresel kapsam kirliliğini önler.

Modül deseni ile, halka açık olmaması gereken kod parçalarını kapsülleyebiliriz. Birden fazla bağımlılık ve ad alanı ile çalışırken riski azaltırlar. Ancak belirtmek gerekir ki, ES2015 modüllerini tüm JavaScript çalışma zamanlarında kullanabilmek için Babel gibi bir transpile aracına ihtiyaç vardır.

**Dezavantajlar**
Modül deseninin dezavantajları arasında hem halka açık hem de özel üyelere farklı şekillerde erişiyor olmamız yer alır. Bir üyenin görünürlüğünü değiştirmek istediğimizde, üyeyi kullandığımız her yerde değişiklik yapmamız gerekmektedir.

Ayrıca, nesneye sonradan eklediğimiz metodlarda özel üyelere erişemeyiz. Bununla birlikte, birçok durumda Modül deseni oldukça yardımcı olmaktadır ve doğru kullanıldığında uygulamanın yapısını kesinlikle geliştirmenin potansiyeline sahiptir.

Diğer dezavantajlar, özel üyeler için otomatik birim testleri oluşturamama ve hatalar düzeltme gerektiğinde ek karmaşıklık içermektedir. Basitçe söylemek gerekirse, özel üyeleri yamalamak mümkün değildir. Bunun yerine, hatalı özel üyelerle etkileşimde bulunan tüm halka açık metodları geçersiz kılmak gerekmektedir. Geliştiriciler özel üyeleri kolayca genişletemezler ve bu nedenle özel üyelerin başlangıçta göründüğü kadar esnek olmadığını hatırlamakta fayda vardır.

Modül deseni hakkında daha fazla okuma için, Ben Cherry'nin mükemmel derinlemesine makalesine bakabilirsiniz.

## Modern Module Pattern with WeakMap
ES6 ile JavaScript'e tanıtılan WeakMap nesnesi, anahtar/değer çiftlerinin bir koleksiyonudur ve anahtarlar zayıf bir referansa sahiptir. Anahtarlar nesneler olmalıdır ve değerler keyfi olabilir. Nesne aslında anahtarların zayıf tutulduğu bir haritadır. Bu, anahtarların nesneye aktif bir referans olmadığı durumda hafıza toplama işlemine (GC) hedef olacağı anlamına gelir. WeakMap nesnesini kullanan modül deseninin bir uygulamasına bakalım.

***Örnek 7-2. Temel Modül Tanımı***
```javascript
let _counter = new WeakMap();
class Module {
constructor() {
_counter.set(this, 0);
}
incrementCounter() {
let counter = _counter.get(this);
counter++;
_counter.set(this, counter);
return _counter.get(this);
}
resetCounter() {
console.log(`Sıfırlamadan önceki sayac değeri: ${_counter.get(this)}`);
_counter.set(this, 0);
}
}
const testModule = new Module();
// Kullanım:
// Sayacımızı artıralım
testModule.incrementCounter();
// Sayacın değerini kontrol edelim ve sıfırlayalım
// Çıktı: Sıfırlamadan önceki sayac değeri: 1
testModule.resetCounter();
```

***Örnek 7-3. Ad Alanları ve Halka Açık/Özel Değişkenler***
```javascript
const myPrivateVar = new WeakMap();
const myPrivateMethod = new WeakMap();
class MyNamespace {
constructor() {
// Özel sayaç değişkeni
myPrivateVar.set(this, 0);
// Herhangi bir argümanı kaydeden özel bir işlev
myPrivateMethod.set(this, foo => console.log(foo));
// Halka açık bir değişken
this.myPublicVar = 'foo';
}
// Özel üyeleri kullanan halka açık bir işlev
myPublicFunction(bar) {
let privateVar = myPrivateVar.get(this);
const privateMethod = myPrivateMethod.get(this);
// Özel sayaç değişkenimizi artıralım
privateVar++;
myPrivateVar.set(this, privateVar);
// Argümanı kullanarak özel işlevimizi çağıralım
privateMethod(bar);
}
}

```
***Örnek 7-4. Alışveriş Sepeti Uygulaması***
```javascript
const basket = new WeakMap();
const doSomethingPrivate = new WeakMap();
const doSomethingElsePrivate = new WeakMap();
class BasketModule {
constructor() {
// Özel üyeler
basket.set(this, []);
doSomethingPrivate.set(this, () => {
//...
});
doSomethingElsePrivate.set(this, () => {
//...
});
}
// Halka açık işlevler için özel işlevleri takma adlarla kullan
doSomething() {
doSomethingPrivate.get(this)();
}
doSomethingElse() {
doSomethingElsePrivate.get(this)();
}
// Sepetimize öğeler ekleyelim
addItem(values) {
const basketData = basket.get(this);
basketData.push(values);
basket.set(this, basketData);
}
// Sepetteki öğe sayısını alalım
getItemCount() {
return basket.get(this).length;
}
// Sepetteki öğelerin toplam değerini alalım
getTotal() {
return basket
.get(this)
.reduce((currentSum, item) => item.price +
currentSum, 0);
}
}
```
## Modern kütüphanelerle Modül Deseni
React gibi JavaScript kütüphaneleriyle uygulama oluştururken Modül desenini kullanabilirsiniz. Diyelim ki takımınız tarafından oluşturulan büyük bir sayıda özel bileşeniniz var. Bu durumda her bileşeni kendi dosyasında ayırarak esasen her bileşen için bir modül oluşturabilirsiniz.
İşte material-ui düğme bileşenini özelleştirerek oluşturulan ve bir modül olarak dışa aktarılan bir düğme bileşeninin örneği:

```javascript
import React from "react";
import Button from "@material-ui/core/Button";
const style = {
root: {
borderRadius: 3,
border: 0,
color: "white",
margin: "0 20px"
},
primary: {
background: "linear-gradient(45deg, #FE6B8B 30%,
#FF8E53 90%)"
},
secondary: {
background: "linear-gradient(45deg, #2196f3 30%,
#21cbf3 90%)"
}
};
export default function CustomButton(props) {
return (
<Button {...props} style={{ ...style.root,
...style[props.color] }}>
{props.children}
</Button>
);
}
```

### Açığa Çıkan Modül Deseni (The Revealing Module Pattern)
Şimdi Modül deseni hakkında biraz daha bilgi sahibiyiz, bu yüzden biraz geliştirilmiş bir versiyonuna bakalım: Christian Heilmann'in Açığa Çıkan Modül deseni.
Açığa Çıkan Modül deseni, Heilmann'ın bir halka açık yöntemi başka bir halka açık yöntemden çağırmak veya halka açık değişkenlere erişmek istediğinde ana nesnenin adını tekrar etmek zorunda kalmasından dolayı hayal kırıklığına uğramasıyla ortaya çıktı. Ayrıca halka açık yapmak istediği şeyler için nesne literali gösterimine geçiş yapmayı sevmedi.
Bu çabalar, tüm fonksiyonları ve değişkenleri özel kapsamda tanımlayabileceğimiz ve halka açık yapmak istediğimiz özel işlevlere işaret eden anonim bir nesne döndürebileceğimiz güncellenmiş bir desenle sonuçlandı.
ES2015+ ile modern modül uygulama şeklinde, modülde tanımlanan fonksiyonların ve değişkenlerin kapsamı zaten özeldir. Ayrıca, açığa çıkarılması gereken her şeyi açığa çıkarmak için export ve import kullanırız.
Aşağıda, ES2015+ ile Açığa Çıkan Modül deseninin kullanımına dair bir örnek bulunmaktadır:

```javascript
let privateVar = 'Ben Cherry';
const publicVar = 'Merhaba!';
const privateFunction = () => {
console.log(`Ad:${privateVar}`);
};
const publicSetName = strName => {
privateVar = strName;
};
const publicGetName = () => {
privateFunction();
};
// Özel işlevleri ve özellikleri halka açık işlevlere
// ve özelliklere işaret edecek şekilde açığa çıkar
const myRevealingModule = {
setName: publicSetName,
greeting: publicVar,
getName: publicGetName,
};
export default myRevealingModule;
// Kullanım:
import myRevealingModule from './myRevealingModule';
myRevealingModule.setName('Paul Kinlan');

```
Yukarıdaki örnekte, privateVar özel değişkenini publicSetName ve publicGetName halka açık get ve set yöntemleriyle açığa çıkarıyoruz.
Bu deseni daha spesifik bir adlandırma şemasıyla özel işlevleri ve özellikleri açığa çıkarmak için de kullanabilirsiniz.

```javascript
let privateCounter = 0;
const privateFunction = () => {
privateCounter++;
}
const publicFunction = () => {
publicIncrement();
}
const publicIncrement = () => {
privateFunction();
}
const publicGetCount = () => privateCounter;
// Özel işlevleri ve özellikleri halka açık işlevlere
// ve özelliklere işaret edecek şekilde açığa çıkar
const myRevealingModule = {
start: publicFunction,
increment: publicIncrement,
count: publicGetCount
};
export default myRevealingModule;
// Kullanım:
import myRevealingModule from './myRevealingModule';
myRevealingModule.start();
```

## Avantajları
Bu desen, betiklerimizin sözdiziminin daha tutarlı olmasına olanak tanır. Ayrıca modülün sonunda hangi işlevlerin ve değişkenlerin halka açık olarak erişilebileceğini anlamak daha kolay hale gelir, bu da okunabilirliği artırır.
## Dezavantajları
Bu desenin dezavantajlarından biri, özel bir işlevin bir halka açık işleve başvurması durumunda, bir yamaya ihtiyaç duyulursa halka açık işlevin geçersiz kılınamamasıdır. Çünkü özel işlev, özel uygulamaya devam edecek ve desen yalnızca halka açık üyelere değil, yalnızca işlevlere uygulanır. Özel değişkenlere başvuran halka açık nesne üyeleri de yama kurallarına tabidir.

Sonuç olarak, Açığa Çıkan Modül deseniyle oluşturulan modüller, orijinal Modül deseniyle oluşturulanlara göre daha kırılgan olabilir ve kullanırken dikkatli olmanız gerekir.

### Singleton Deseni
Singleton deseni, bir sınıfın örneklemesini bir nesne ile sınırlayan bir tasarım desenidir. Bu, sistem genelindeki işlemleri koordine etmek için tam olarak bir nesnenin gerektiği durumlarda faydalıdır. Klasik olarak, Singleton desenini, yalnızca bir örnek zaten mevcut değilse yeni bir örnek oluşturan bir yöntemle uygulayabilirsiniz. Bir örnek zaten varsa, sadece o nesneye bir referans döndürür.

Singletonlar, statik sınıflardan (veya nesnelerden) farklıdır, çünkü onların başlatılmasını geciktirebiliriz, çünkü başlatılma zamanında mevcut olmayabilecek bazı bilgilere ihtiyaç duyarlar. Daha önceki bir Singleton sınıfına dair bilgisi olmayan herhangi bir kod, onu kolayca alamaz. Çünkü Singleton'ın döndürdüğü ne nesne ne de "sınıf"dır; bir yapıdır. Bir kapanmış değişkenin gerçekte kapanmış bir değişken olmadığı gibi—kapanmışlığı sağlayan işlev kapsamı kapanmıştır.

ES2015+, Singleton desenini uygulamak için bir JavaScript sınıfının bir kez örneklenen küresel bir örneğini oluşturmak için kullanabiliriz. Singleton örneğini bir modül ihracatı ile açığa çıkarabilirsiniz. Bu, erişimi daha açık ve kontrollü hale getirir ve diğer küresel değişkenlerden ayırır. Yeni bir sınıf örneği oluşturamazsınız, ancak sınıfta tanımlanan halka açık get ve set yöntemleri aracılığıyla örneği okuyabilir/değiştirebilirsiniz.
Bir Singleton'ı aşağıdaki gibi uygulayabiliriz:

```javascript
// Örnek, Singleton'a bir referansı saklar
let instance;
// Özel yöntemler ve değişkenler
const privateMethod = () => {
console.log('Ben özelim');
};
const privateVariable = 'Ben de özelim';
const randomNumber = Math.random();
// Singleton
class MySingleton {
// Singleton örneğini alın eğer zaten bir varsa
// veya varsa bir tane oluşturun
constructor() {
if (!instance) {
// Halka açık özellik
this.publicProperty = 'Ben de halka açığım';
instance = this;
}
return instance;
}
// Public yöntemler
publicMethod() {
console.log('Halk da beni görebilir!');
}
getRandomNumber() {
return randomNumber;
}
}
// [ES2015+] İsimsiz export, ad olmadan modülü varsayılan olarak açığa çıkar
export default MySingleton;
// Örnek, Singleton'a bir referansı saklar
let instance;
// Singleton
class MyBadSingleton {
// Her zaman yeni bir Singleton örneği oluştur
constructor() {
this.randomNumber = Math.random();
instance = this;
return instance;
}
getRandomNumber() {
return this.randomNumber;
}
}
export default MyBadSingleton;
// Kullanım:
import MySingleton from './MySingleton';
import MyBadSingleton from './MyBadSingleton';
const singleA = new MySingleton();
const singleB = new MySingleton();
console.log(singleA.getRandomNumber() ===
singleB.getRandomNumber());
// true
const badSingleA = new MyBadSingleton();
const badSingleB = new MyBadSingleton();
console.log(badSingleA.getRandomNumber() !==
badSingleB.getRandomNumber());
// true
// Not: Rastgele sayılarla çalıştığımız için, her iki sayının da aynı olma
matematiksel bir
// olasılığı vardır, ancak olasılıkla düşük bir ihtimaldir.
// Yukarıdaki örnek aksi belirtilmedikçe hala geçerli olmalıdır.
```
Singleton'ı özel kılan, örneğe küresel erişimdir. GoF kitabı, Singleton deseninin uygulanabilirliğini şu şekilde açıklar:

Bir sınıfın yalnızca bir örneği olmalıdır ve bu örneğe iyi bilinen bir erişim noktasından müşteriler tarafından erişilebilmelidir.
Tek örnek alt sınıflandırma ile genişletilebilir olmalıdır ve müşteriler kodlarını değiştirmeden genişletilmiş bir örneği kullanabilmelidir.
İkinci nokta, böyle bir durumda kod ihtiyacımız olabileceği duruma atıfta bulunur, örneğin:
```javascript
constructor() {
if (this._instance == null) {
if (isFoo()) {
this._instance = new FooSingleton();
} else {
this._instance = new BasicSingleton();
}
}
return this._instance;
}
```
Burada constructor biraz bir Fabrika (Factory) yöntem gibi olur ve ona erişen kodun her noktasını güncelleme ihtiyacımız olmaz. Yukarıdaki FooSingleton (yukarıda) BasicSingleton'ın alt sınıfı olacaktır ve aynı arayüzü uygular.

Neden yürütmenin ertelenmesi Singleton için önemli olarak kabul edilir? C++'da, bu, dinamik başlatma sırasının öngörülemeyenliğinden izolasyon sağlar ve kontrolü programcıya geri verir.

Bir sınıfın (nesnenin) statik bir örneği ile Singleton arasındaki farkı belirtmek önemlidir. Bir Singleton'ı statik bir örnek olarak uygulayabilirsiniz, ancak aynı zamanda kaynaklar veya bellek gerektirmeden ve ihtiyaç duyulana kadar tembelce oluşturulabilir.

Doğrudan başlatabileceğimiz bir statik nesnemiz olsaydı, kodun her zaman aynı sırayla yürütüldüğünden emin olmamız gerekirdi (örneğin, objCar objWheel'in başlatılması sırasında objWheel'e ihtiyaç duyarsa), ve bu, çok sayıda kaynak dosyasına sahip olduğunuzda ölçeklenmez.

Her ikisi de kullanışlı olan Singletonlar ve statik nesneler kullanışlıdır, ancak aynı şekilde diğer desenleri de aşırı kullanmamalıyız.

Uygulamada, Singleton desenini, sistem genelinde diğerleri arasında koordinasyon gerektiğinde tam olarak bir nesne gerektiğinde kullanmak faydalı olur. İşte bu bağlamda deseni kullanan bir örnek:

```javascript
// options: Singleton için yapılandırma seçeneklerini içeren bir nesne
// örn. const options = { name: "test", pointX: 5};
class Singleton {
constructor(options = {}) {
// Singleton için bazı özellikleri ayarlayın
this.name = 'SingletonTester';
this.pointX = options.pointX || 6;
this.pointY = options.pointY || 10;
}
}
// örnek tutucumuz
let instance;
// statik değişkenlerin ve yöntemlerin bir taklidi
const SingletonTester = {
name: 'SingletonTester',
// Bir örneğin alınması için yöntem. Singleton'ın bir örneğini döndürür
// bir Singleton nesnesinin tek bir örneği
getInstance(options) {
if (instance === undefined) {
instance = new Singleton(options);
}
return instance;
},
};
const singletonTest = SingletonTester.getInstance({
pointX: 5,
});
// pointX çıktısını sadece doğrulamak için kaydedin
// Çıktı: 5
console.log(singletonTest.pointX);
```
Singleton'ın geçerli kullanımları olsa da, genellikle JavaScript'te bunu ihtiyaç duyduğumuzda, tasarımımızı tekrar gözden geçirmemiz gerektiğinin bir işareti olabilir. C++ veya Java'dan farklı olarak, bir nesne oluşturmak için bir sınıfı tanımlamak zorunda değilsiniz. Bu nedenle, bir Singleton sınıfı tanımlamak yerine doğrudan böyle bir nesne oluşturabilirsiniz. Öte yandan, JavaScript'te singleton sınıflarını kullanmanın bazı dezavantajları vardır.

- ***Singleton'ların tanımlanması zor olabilir:***  Büyük bir modülü içe aktarıyorsanız, belirli bir sınıfın Singleton olduğunu tanıyamayabilirsiniz. Sonuç olarak, bu sınıfı yanlışlıkla çoklu nesneleri örneklendiren normal bir sınıf gibi kullanabilir ve yanlışlıkla güncelleyebilirsiniz.

- ***Test etmek zor olabilir:*** Singletonları test etmek daha zor olabilir, gizli bağımlılıklardan, birden fazla örnek oluşturmanın zorluğundan, bağımlılıkların sahtelenmesinin zorluğundan ve benzeri sorunlardan kaynaklanan sorunlar nedeniyle.

- ***Dikkatli düzenlemeye ihtiyaç duyar:*** Singleton'ların günlük kullanım durumu, global kapsam boyunca gerekecek verileri depolamak olabilir, örneğin kullanıcı kimlik bilgileri veya bir kez ayarlanabilecek ve birden çok bileşen tarafından tüketilebilecek çerez verileri gibi.

Verinin her zaman kullanılabilir hale gelmesinden sonra tüketilmesi ve tersi durumda olmaması için doğru yürütme sırasını uygulamak önemlidir. Bu, uygulama boyutu ve karmaşıklığı arttıkça zor bir hal alabilir.

Miller Medeiros daha önce Singleton ve onun çeşitli sorunları hakkında bu mükemmel makaleyi önerdi ve bu makalenin yorumları Singleton'ların sıkı bağlamayı artırabileceğini tartışıyor. Bu önerileri önemli birçok noktayı ele alır ve bu desenle ilgili dikkate değerdir.


## React'ta Durum (State) Yönetimi
React'i web geliştirmede kullanan geliştiriciler, Singletonlar yerine Redux veya React Context gibi durum yönetimi araçları aracılığıyla global duruma güvenebilirler. Bu araçlar, değiştirilemez durumu değiştirilebilir durum yerine sunarlar.

Global bir duruma sahip olmanın dezavantajları bu araçları kullanarak sihirli bir şekilde kaybolmaz. Ancak bileşenler doğrudan güncelleme yapamadığı için en azından global durumun niyetimize uygun şekilde değiştirildiğinden emin olabiliriz.

### Prototip Deseni 
GoF, Prototip desenini mevcut bir nesnenin bir şablonuna dayalı olarak nesneler oluşturan bir desen olarak adlandırır.

Prototip desenini, prototip mirası üzerine kurulu olarak düşünebiliriz, burada diğer nesneler için prototip görevi gören nesneler oluştururuz. Prototip nesnesi, yapılandırıcı her nesne için bir şablondur. Örneğin, kullanılan yapılandırıcı işlevin prototipi name adında bir özellik içeriyorsa (aşağıdaki kod örneğinde olduğu gibi), o yapılandırıcı tarafından oluşturulan her nesne de bu aynı özelliğe sahip olur.

XX ŞEKİL

Mevcut literatürde Prototype deseni hakkında yapılan tanımlamalarda sıkça sınıflara atıfta bulunulabilir, ancak gerçekte JavaScript'teki prototip temelli mirasa sınıflarla ilgisi yoktur. JavaScript'teki prototip temelli miras, varolan fonksiyonel nesnelerin kopyalarını oluşturarak gerçekleşir; "tanım" nesneleri veya çekirdek nesneleri tanımlamak yerine varolan nesnelerin kopyalarını oluştururuz.

Prototip deseni kullanmanın avantajlarından biri, JavaScript'in yerel prototip yeteneklerini tam anlamıyla kullanmaktır. Diğer dillerin özelliklerini taklit etmeye çalışmak yerine, JavaScript'in temel özelliklerini kullanma fırsatı sağlar.

Prototip deseni, kalıtımı basit bir şekilde sağlamakla kalmaz, aynı zamanda performans artışı da sunabilir. Bir nesnenin içinde işlevleri tanımlarken, bu işlevler referans yoluyla oluşturulur; bu sayede alt nesneler aynı işlevleri paylaşır ve kopyalamak yerine referans kullanılır.

ES2015+ ile sınıflar ve yapıcılar kullanarak nesne oluşturmak mümkündür. Bu, kodun okunabilirliğini artırır ve nesne yönelimli tasarım prensiplerini takip etmemizi sağlar. Ancak sınıflar ve yapıcılar, dahili olarak işlevlere ve prototiplere derlenir, böylece JavaScript'in prototip yeteneklerini ve buna bağlı performans artışını sürdürür.

ECMAScript 5 standardında tanımlanan gerçek prototip temelli miras için `Object.create` kullanımı gereklidir. Bu yöntem belirtilen bir prototip ve isteğe bağlı özelliklere sahip bir nesne oluşturur. Örneğin:

```javascript
const myCar = {
  name: 'Ford Escort',
  drive() {
    console.log("Weeee. I'm driving!");
  },
  panic() {
    console.log('Wait. How do you stop this thing?');
  },
};

const yourCar = Object.create(myCar);
console.log(yourCar.name);
```

`Object.create`, nesnelerin doğrudan diğer nesnelerden miras alabileceği diferansiyel miras gibi gelişmiş kavramları da destekler ve ikinci argümanı kullanarak nesne özelliklerini başlatmayı sağlar. Örneğin:

```javascript
const vehicle = {
  getModel() {
    console.log(`The model of this vehicle is ${this.model}`);
  },
};

const car = Object.create(vehicle, {
  id: {
    value: MY_GLOBAL.nextId(),
    enumerable: true,
  },
  model: {
    value: 'Ford',
    enumerable: true,
  },
});
```

`Object.create` kullanmadan Prototip desenini uygulamak isterseniz, sınıfları ve kalıtımı kullanarak simüle edebilirsiniz. İşte bir örnek:

```javascript
class VehiclePrototype {
  constructor(model) {
    this.model = model;
  }

  getModel() {
    console.log(`The model of this vehicle is ${this.model}`);
  }

  Clone() {}
}

class Vehicle extends VehiclePrototype {
  constructor(model) {
    super(model);
  }

  Clone() {
    return new Vehicle(this.model);
  }
}

const car = new Vehicle('Ford Escort');
const car2 = car.Clone();
car2.getModel();
```

**Not:** Prototip ilişkileri nesnelerin özelliklerini sıralarken sorunlara neden olabilir ve döngü içeriğini `hasOwnProperty()` ile kontrol etmek önerilir.

Prototip deseninin alternatif uygulamaları, `beget` gibi fonksiyonları kullanarak prototiplere dayalı yeni nesneler oluşturmayı içerebilir. Ancak bu yaklaşımlar, `Object.create` kullanmaktan elde edilen esneklik ve işlevselliği sağlamaz.


## Factory Deseni

Factory deseni, nesneleri oluşturmak için kullanılan başka bir yaratıcı desendir. Bu kategorideki diğer desenlerden farklı olarak, açıkça bir kurucu kullanma gerekliliği olmadan da çalışabilir.

Bir "UI Factory" hayal edin, burada belirli bir türde bir UI bileşeni oluşturmak istiyoruz. Bu bileşeni doğrudan `new` operatörünü veya başka bir yaratıcı kurucuyu kullanarak oluşturmak yerine, bir Factory nesnesine yeni bir bileşen istediğimizi sorarız. Factory'ye hangi türde bir nesne gerektiğini bildiririz (örneğin, "Button" veya "Panel"), o da bu nesneyi oluşturur ve kullanımımız için bize geri döndürür.

Bu yaklaşım, nesne oluşturma süreci karmaşıksa özellikle değerlidir. Örneğin, dinamik faktörlere veya uygulama yapılandırmasına güçlü bir şekilde bağımlıysa faydalıdır. Bu deseni, ExtJS gibi UI kütüphanelerinde nesneleri veya bileşenleri oluşturan yöntemlerin daha da alt sınıflandırılabileceği yerlerde bulabilirsiniz.

Aşağıdaki örnek, araçları tanımlamak için Kurucu deseni mantığını kullanan önceki kod parçalarını temel alarak, Factory desenini nasıl uygulayabileceğimizi göstermektedir:

```javascript
// Types.js - Arka planda kullanılan yapılandırıcılar (constructors)
class Car {
  constructor({
    doors,
    state,
    color
  }) {
    this.doors = doors || 4;
    this.state = state || 'yeni';
    this.color = color || 'gümüş';
  }
}
// Yeni kamyonları tanımlamak için yapılandırıcı
class Truck {
  constructor({
    state,
    wheelSize,
    color
  }) {
    this.state = state || 'kullanılmış';
    this.wheelSize = wheelSize || 'büyük';
    this.color = color || 'mavi';
  }
}
// FactoryExample.js
// Bir araç fabrikası tanımla
class VehicleFactory {
  // Bu fabrika için prototipleri ve yardımcıları tanımla
  // Varsayılan araç sınıfımız Car
  constructor() {
    this.vehicleClass = Car;
  }
  // Yeni Araç örnekleri oluşturmak için fabrika yöntemimiz
  createVehicle(options) {
    switch (options.vehicleType) {
      case 'car':
        this.vehicleClass = Car;
        break;
      case 'truck':
        this.vehicleClass = Truck;
        break;
      // varsayılan olarak VehicleFactory.prototype.vehicleClass (Car)
    }
    return new this.vehicleClass(options);
  }
}
// Araba üreten bir fabrika örneği oluştur
const carFactory = new VehicleFactory();
const car = carFactory.createVehicle({
  vehicleType: 'car',
  color: 'sarı',
  doors: 6,
});
// Araç nesnemizin vehicleClass/prototype Car kullanılarak oluşturulduğunu doğrulamak için test et
// Çıktı: true
console.log(car instanceof Car);
// Çıktı: Renk "sarı", kapılar: 6 olan bir "yeni" durumlu Car nesnesi
console.log(car);
```

Car ve Truck sınıflarını ilgili taşıt özelliklerini ayarlayan yapılandırıcılarla tanımladık. VehicleFactory, vehicleType'a dayalı olarak yeni bir araç nesnesi, Car veya Truck, oluşturabilen bir nesne oluşturur.

VehicleFactory sınıfını kullanarak kamyonlar oluşturma konusunda iki olası yaklaşım vardır.

Yaklaşım 1'de, bir VehicleFactory örneğini Truck sınıfını kullanacak şekilde değiştiriyoruz:

```javascript
const movingTruck = carFactory.createVehicle({
  vehicleType: 'truck',
  state: 'yeni gibi',
  color: 'kırmızı',
  wheelSize: 'küçük',
});
// Kamyonumuzun vehicleClass/prototype Truck kullanılarak oluşturulduğunu doğrulamak için test et
// Çıktı: true
console.log(movingTruck instanceof Truck);
// Çıktı: Renk "kırmızı", "yeni gibi" durumlu ve "küçük" tekerlek boyutlu Truck nesnesi
console.log(movingTruck);
```

Yaklaşım 2'de ise, VehicleFactory'yi alt sınıf olarak kullanarak Truck oluşturan bir fabrika sınıfı oluşturuyoruz:

```javascript
class TruckFactory extends VehicleFactory {
  constructor() {
    super();
    this.vehicleClass = Truck;
  }
}
const truckFactory = new TruckFactory();
const myBigTruck = truckFactory.createVehicle({
  state: 'vahşi kötü',
  color: 'pembe',
  wheelSize: 'çok büyük',
});
// myBigTruck'ın Truck prototipi ile oluşturulduğunu doğrula
// Çıktı: true
console.log(myBigTruck instanceof Truck);
// Çıktı: Renk "pembe", tekerlek boyutu "çok büyük" ve durum "vahşi kötü" olan Truck nesnesi
console.log(myBigTruck);
```

### **Fabrika Deseni Ne Zaman Kullanılır**

Fabrika deseni aşağıdaki durumlarda uygulandığında faydalı olabilir:

- Nesnelerin veya bileşenlerin kurulumu yüksek bir karmaşıklık içeriyorsa.
- Farklı nesne örneklerini ortama bağlı olarak kolayca oluşturmanın gerektiği durumlarda.
- Aynı özellikleri paylaşan birçok küçük nesne veya bileşenle çalışıyorsak.
- API sözleşmesini (örneğin, duck typing) sadece karşılamaları gereken diğer nesne örnekleri ile nesneleri birleştirirken. Bu, bağımlılıkları azaltmak için kullanışlıdır.

### **Fabrika Deseni Ne Zaman Kullanılmamalıdır**

Bu desen, yanlış türdeki bir probleme uygulandığında uygulamaya gereksiz karmaşıklık ekleyebilir. Nesne oluşturmanın bir arayüz sağlama, yazdığımız kütüphane veya çerçevenin bir tasarım amacı olmadığı sürece, gereksiz yükten kaçınmak için açıkça belirtilen yapıcıları kullanmanızı öneririm.

Çünkü nesne oluşturma süreci etkili bir şekilde bir arayüzün arkasına gizlendiğinde, bu sürecin ne kadar karmaşık olabileceğine bağlı olarak birim testi ile ilgili sorunlar da ortaya çıkabilir.

## Soyut Fabrika (Abstract Factories) Deseni 

Aynı zamanda Soyut Fabrika desenini de bilmek faydalı olabilir, çünkü bu desen bir grup benzer fabrikayı tek bir amaç altında kapsamayı amaçlar. Bu desen, nesnelerin uygulanmasının ayrıntılarını onların genel kullanımından ayırır.

Soyut Fabrika desenini, bir sistemin oluşturduğu nesnelerin nasıl üretildiğinden bağımsız olması gerektiğinde veya birden fazla nesne türü ile çalışması gerektiğinde kullanabilirsiniz. 

Basit ve anlaşılması daha kolay bir örnek, araç fabrikasıdır. Bu desende AbstractVehicleFactory adında bir soyut fabrika tanımlanabilir. Soyut fabrika, araç türlerini tanımlamayı sağlar, örneğin araba veya kamyon, ve somut fabrikalar sadece araç kontratını (örneğin Vehicle.prototype.drive ve Vehicle.prototype.breakDown) yerine getiren sınıfları uygular.

```javascript
class AbstractVehicleFactory {
  constructor() {
    // Araç türlerimizi saklamak için
    this.types = {};
  }

  static getVehicle(type, customizations) {
    const Vehicle = this.types[type];
    return Vehicle ? new Vehicle(customizations) : null;
  }

  static registerVehicle(type, Vehicle) {
    const proto = Vehicle.prototype;
    // Sadece araç kontratını yerine getiren sınıfları kaydet
    if (proto.drive && proto.breakDown) {
      this.types[type] = Vehicle;
    }
    return AbstractVehicleFactory;
  }
}

// Kullanım:
AbstractVehicleFactory.registerVehicle('car', Car);
AbstractVehicleFactory.registerVehicle('truck', Truck);

// Soyut araç türüne dayalı yeni bir araç örneği oluştur
const car = AbstractVehicleFactory.getVehicle('car', {
  color: 'lime green',
  state: 'like new',
});

// Benzer şekilde yeni bir kamyon örneği oluştur
const truck = AbstractVehicleFactory.getVehicle('truck', {
  wheelSize: 'medium',
  color: 'neon yellow',
});
```

Bu örnek, Soyut Fabrika desenini kullanarak araç türlerini tanımlamayı, kaydetmeyi ve istemci tarafından araç örneklerini oluşturmayı göstermektedir. Bu desen, nesne oluşturma sürecinin daha soyutlanmış bir şekilde ele alınmasını sağlar ve farklı türlerdeki nesnelerle çalışmanın kolaylaştırılmasına yardımcı olabilir.

## Yapısal Desenler**

Yapısal desenler, sınıf ve nesne kompozisyonu ile ilgilenir. Örneğin, miras kavramı, arayüzleri ve nesneleri bir araya getirerek yeni işlevsellik elde etmemizi sağlar. Yapısal desenler, sınıfları ve nesneleri düzenlemek için en iyi yöntemleri ve uygulamaları sunar.

Bu bölümde ele alacağımız JavaScript yapısal desenler şunlardır:
- "Facade Deseni"
- "Mixin Deseni"
- "Decorator Deseni"
- "Flyweight Deseni"

### Facade Deseni**

Bir cephe oluşturduğumuzda, dünyaya dışa doğru bir görünüm sunarız, ki bu görünüm altında çok farklı bir gerçeklik olabilir. Bu durum, inceleyeceğimiz sonraki desen olan Facade deseninin adını ilham verir. Bu desen, daha büyük bir kod kümesine daha uygun bir yüksek seviye arabirim sağlayarak, onun gerçek altta yatan karmaşıklığını gizler.

Bunu, diğer geliştiricilere sunulan API'yi basitleştirmek olarak düşünebilirsiniz, bu neredeyse her zaman kullanılabilirliği artıran bir kalitedir. Cepheler, genellikle jQuery gibi JavaScript kütüphanelerinde görebileceğiniz yapısal bir desendir. Bu tür bir kütüphane, geniş bir davranış yelpazesi sunabilecek yöntemleri desteklemesine rağmen, genellikle yalnızca bir "yüzey", yani yalınlaştırılmış bir soyutlama, dışa sunulur.

Bu şekilde, alt sistemle doğrudan etkileşim yerine Cephelerle doğrudan etkileşime geçeriz. jQuery'in $(el).css() veya $(el).animate() yöntemlerini kullandığımızda, bir Cephe kullanıyoruz: daha basit ve halka açık bir arayüz, böylece bir davranışı çalıştırmak için jQuery çekirdeğindeki birçok dahili yöntemi manuel olarak çağırmaktan kaçınılabilir. Bu ayrıca DOM API'leriyle manuel olarak etkileşim kurma ve durum değişkenlerini yönetme ihtiyacını da ortadan kaldırır. jQuery çekirdek yöntemleri ara bir soyutlama olarak kabul edilmelidir. Geliştiricilere daha yakın bir yük bindiren, DOM API'si ve yüzeylerin jQuery kütüphanesini kullanmayı kolaylaştıran bir yapıdır.

Öğrendiklerimizi temel alarak, Facade deseni bir sınıfın arayüzünü basitleştirir ve sınıfı kullanan kodu bağımsız hale getirir. Bu, alt sistemlerle dolaylı olarak etkileşimde bulunmamızı sağlar ve bazen alt sistemlere doğrudan erişimden daha az hata eğilimli olabilir. Bir Cephenin avantajları, kullanım kolaylığı ve genellikle deseni uygularken küçük bir iz bırakmasıdır.

Deseni aksiyonda görmek için aşağıdaki örneğe bakalım. Bu optimize edilmemiş bir kod örneğidir, ancak burada bir Cephe kullanarak tarayıcılar arası olayları dinlemek için bir arayüzü basitleştiriyoruz. Bunun için, özelliklerin varlığını kontrol etme görevini yerine getiren ortak bir yöntem oluşturarak güvenli ve tarayıcılar arası uyumlu bir çözüm sağlayabiliriz.
```javascript
const addMyEvent = (el, ev, fn) => {
  if (el.addEventListener) {
    el.addEventListener(ev, fn, false);
  } else if (el.attachEvent) {
    el.attachEvent(`on${ev}`, fn);
  } else {
    el[`on${ev}`] = fn;
  }
};
```
Benzer bir şekilde, hepimiz jQuery'in $(document).ready(..) işlevini biliyoruz. İçeride, bu işlevi çalıştıran bindReady() adlı bir yöntem bulunur, bu da şunu yapar:
```javascript
function bindReady() {
  // ...
  if (document.addEventListener) {
    document.addEventListener('DOMContentLoaded',
    DOMContentLoaded, false);
    window.addEventListener('load', jQuery.ready,
    false);
  } else if (document.attachEvent) {
    document.attachEvent('onreadystatechange',
    DOMContentLoaded);
    window.attachEvent('onload', jQuery.ready);
  }
}
```
Bu, Cephe'nin başka bir örneğidir. Dünya genelinde, $(document).ready(..) tarafından sunulan sınırlı arabirim kullanılır ve arkasındaki daha karmaşık uygulama görünmez tutulur. Ancak Cepheler yalnızca tek başına kullanılmak zorunda değildir. Aynı zamanda Modül deseni gibi diğer desenlerle de entegre edebilirsiniz. Aşağıda gördüğümüz gibi, Modül deseninin örneği bir dizi özelliği özel olarak tanımlamış bir d

#### **Soyutlama Üzerine Notlar**

Facades genellikle birkaç dezavantaja sahip olabilir, ancak özellikle dikkate alınması gereken bir endişe performanstır. Özellikle, bir Cephenin uygulamamıza sunduğu soyutlamanın zımni bir maliyeti olup olmadığını ve bu maliyetin haklı çıkarılıp çıkarılamayacağını belirlemek gereklidir. jQuery kütüphanesine geri dönersek, çoğumuzun hem getElementById("kimlik") hem de $("#kimlik") kullanılarak bir sayfadaki bir öğeyi sorgulayabileceğini biliyoruz.

Ancak, getElementById() işlevinin tek başına, büyük bir büyüklük sırasıyla daha hızlı olduğunu biliyor muydunuz? Tarayıcı düzeyinde sonuçları görmek için bu jsPerf testine göz atın. Elbette, jQuery (ve seçici motoru Sizzle) sorgumuzu optimize etmek için sahnenin arkasında daha fazla iş yapmaktadır (ve yalnızca bir DOM düğümü değil, bir jQuery nesnesi döndürülmektedir).

Bu özel Cephe ile ilgili zorluk, birden çok türde sorguları kabul edebilen ve ayrıştırabilen zarif bir seçici işlevi sağlamak için soyutlamanın zımni bir maliyetine sahip olmasıdır. Kullanıcının jQuery.getById("kimlik") veya jQuery.getbyClass("kimlik") vb. gibi erişmesi gerekmez. Ancak, performansta yapılan fedakarlık yıllar içinde pratikte test edilmiştir ve jQuery'nin geçmişteki başarısı göz önüne alındığında, basit bir Cephe takım için çok iyi çalışmıştır.

Cephe desenini kullanırken, içeren performans maliyetlerinin farkında olmaya çalışın ve bu maliyetlerin sunulan soyutlama düzeyine değip değmeyeceğine karar verin.

## **Mixin Deseni**

Geleneksel programlama dilleri olan C++ ve Lisp gibi dillerde, Mixin'ler, bir alt sınıfın veya alt sınıf grubunun işlevselliği sunan ve işlevleri tekrar kullanım için kolayca devralabileceği sınıflardır.

### Alt Sınıflandırma
Zaten bir üst veya üst sınıfı genişletmemize ve üst sınıftaki yöntemleri çağırabilmemize olanak tanıyan ES2015+ özelliklerini tanıttık. Üst sınıfı genişleten alt sınıfa alt sınıf denir.
Alt sınıflandırma, yeni bir nesne için temel veya üst sınıf nesnesinden özelliklerin miras alınması anlamına gelir. Bir alt sınıf hala kendi yöntemlerini tanımlayabilir, başlangıçta üst sınıfta tanımlanan yöntemleri de içeren. Alt sınıftaki yöntem, üst sınıfta ezilen bir yöntemi çağırabilir, bu yöntem zincirleme olarak adlandırılır. Benzer şekilde, üst sınıfın yapıcı yöntemini çağırabilir, bu da yapılandırıcı zincirleme olarak adlandırılır.
Alt sınıflandırmayı göstermek için önce kendisi için yeni örnekler oluşturabilecek bir temel sınıfımız olması gerekmektedir. Bunu bir kişi kavramı etrafında modelleyelim.
```javascript
class Person {
    constructor(firstName, lastName) {
        this.firstName = firstName;
        this.lastName = lastName;
        this.gender = "male";
    }
}
// Person'un yeni bir örneği şu şekilde kolayca oluşturulabilir:
const clark = new Person('Clark', 'Kent');
```
Sonraki adımda, mevcut Person sınıfının alt sınıfı olan yeni bir sınıf belirtmek isteyeceğiz. Bir Süper Kahramanı bir Kişiden ayırt etmek için özgün özellikler eklemek istediğimizi hayal edelim ve aynı zamanda Kişi üst sınıfının özelliklerini devralalım. Süper kahramanlar, normal insanlarla birçok ortak özellik paylaştığından (örneğin, ad, cinsiyet), bu, alt sınıflandırma nasıl çalışırı anlamamıza yardımcı olacaktır.
```javascript
class Superhero extends Person {
    constructor(firstName, lastName, powers) {
        // Üst sınıf yapıcısını çağır
        super(firstName, lastName);
        this.powers = powers;
    }
}
// Süper Kahramanın yeni bir örneği aşağıdaki gibi oluşturulabilir
const SuperMan = new Superhero('Clark', 'Kent', ['uçuş', 'ısı görme']);
console.log(SuperMan);
```
Superhero yapıcısı, Person sınıfının bir genişlemesi olan Superhero sınıfının bir örneğini oluşturur. Bu tür nesneler zincirde üstünde bulunan sınıfların özelliklerine sahiptir. Eğer Person sınıfında varsayılan değerler belirlemiş olsaydık, Süper Kahraman, miras alınan değerleri sınıfına özgü değerlerle geçersiz kılabilirdi.

### Mixins

JavaScript'te, işlevselliği uzantı yoluyla toplamak için Mixin'lerden miras almayı inceleyebiliriz. Tanımladığımız her yeni sınıfın, yöntemleri ve özellikleri miras alabileceği bir üst sınıfı olabilir. Sınıflar ayrıca kendi özelliklerini ve yöntemlerini tanımlayabilirler. Bu gerçeği işlev tekrarı için kullanabiliriz. Mixin'ler nesnelerin bu işlevselliği ödünç almasına (veya miras almasına) olanak tanır ve bunu minimum karmaşıklıkla yapar. Bu nedenle, Mixin'ler, birden fazla başka sınıf arasında kolayca paylaşılabilen niteliklere ve yöntemlere sahip sınıflardır.

JavaScript sınıfları birden fazla üst sınıftan miras alamasa da, çeşitli sınıflardan işlevselliği karıştırabiliriz. Bir JavaScript sınıfı ifade olarak da kullanılabilir. Bir ifade olarak kullanıldığında, her değerlendirildiğinde yeni bir sınıf döndürür. extends ifadesi, ayrıca sınıflar veya yapıcılar döndüren keyfi ifadeleri kabul edebilir. Bu özellikler bize bir Mixin'i kabul eden bir işlev olarak tanımlama olanağı verir ve bu işlev bir üst sınıf kabul eder ve ondan yeni bir alt sınıf oluşturur.

Aşağıda, bir standart JavaScript sınıfında yardımcı işlevleri içeren bir Mixin tanımladık:
```javascript
const MyMixins = superclass =>
class extends superclass {
    moveUp() {
        console.log('move up');
    }
    moveDown() {
        console.log('move down');
    }
    stop() {
        console.log('stop! in the name of love!');
    }
};
```
Yukarıda, MyMixins adlı bir işlev oluşturduk, bu işlev dinamik bir üst sınıfı genişletebilir. Şimdi MyMixins tarafından tanımlanan yöntemlerle ve genişletilen sınıftaki yöntemlerle alt sınıf döndürebilen CarAnimator ve PersonAnimator adlı iki sınıf oluşturacağız.
```javascript
// Bir iskelet carAnimator yapıcısı
class CarAnimator {
    moveLeft() {
        console.log('move left');
    }
}
// Bir iskelet personAnimator yapıcısı
class PersonAnimator {
    moveRandomly() {
        /*..*/
    }
}
// CarAnimator'ü MyMixins kullanarak genişlet
class MyAnimator extends MyMixins(CarAnimator) {}
// Yeni bir carAnimator örneği oluştur
const myAnimator = new MyAnimator();
myAnimator.moveLeft();
myAnimator.moveDown();
myAnimator.stop();
// Çıktı:
// move left
// move down
// stop! in the name of love!
```
Görüldüğü gibi, bu benzer davranışları sınıflara karıştırmak oldukça basit hale getirir.
Aşağıdaki örnekte iki sınıf var: Car ve Mixin. Yapacağımız şey, Car'ı artırmaktır (başka bir deyişle genişletmek), böylece Mixin'de tanımlanan belirli yöntemleri, yani driveForward() ve driveBackward() yöntemlerini devralabilir.
Bu örnek, her yapıcı işlev için bu işlemi tekrarlamak zorunda kalmadan işlevselliği içerecek şekilde bir yapıyı nasıl genişletebileceğimizi gösterir.
```javascript
// Basit bir Car yapıcısını tanımla
class Car {
    constructor({ model, color }) {
        this.model = model || 'no model provided';
        this.color = color || 'no colour provided';
    }
}
// Mixin
const Mixin = superclass =>
class extends superclass {
    driveForward() {
        console.log('drive forward');
    }
    driveBackward() {
        console.log('drive backward');
    }
    driveSideways() {
        console.log('drive sideways');
    }
};
class MyCar extends Mixin(Car) {}
// Yeni bir Car oluştur
const myCar = new MyCar({
    model: 'Ford Escort',
    color: 'blue',
});
// Yöntemlere erişimin olduğundan emin olmak için test yap
myCar.driveForward();
myCar.driveBackward();
// Çıktı:
// drive forward
// drive backward
const mySportsCar = new MyCar({
    model: 'Porsche',
    color: 'red',
});
mySportsCar.driveSideways();
// Çıktı:
// drive sideways
```

### **Avantajlar ve Dezavantajlar**

Mixin'ler, sistemin içinde fonksiyonel tekrarı azaltmaya ve fonksiyon yeniden kullanımını artırmaya yardımcı olur. Bir uygulamanın muhtemelen nesne örnekleri arasında paylaşılan davranış gerektireceği durumlarda, bu paylaşılan işlevselliği bir Mixin içinde saklayarak yinelemeyi kolayca önleyebiliriz. Bu şekilde, gerçekten farklı olan işlevselliği uygulamak için odaklanabiliriz.

Ancak, Mixin'lerin dezavantajları biraz daha tartışmalıdır. Bazı geliştiriciler, bir sınıfa veya nesne prototipine işlevselliği enjekte etmenin kötü bir fikir olduğunu düşünür, çünkü hem prototip kirliliğine hem de işlevlerin kaynağı konusunda belirsizliğe yol açar. Büyük sistemlerde, bu durum doğru olabilir.

React'ta bile, ES6 sınıflarının tanıtılmasından önce bile Mixin'ler genellikle bileşenlere işlevsellik eklemek için kullanılırdı. React ekibi, Mixin'leri bileşene gereksiz karmaşıklık eklediği ve bileşeni sürdürmeyi ve yeniden kullanmayı zorlaştırdığı için teşvik etmiyor. React ekibi, bunun yerine yüksek düzeyli bileşenler ve Hook'ların kullanılmasını öneriyor.

Ben, sağlam belgelemenin karma karışıklığın miktarını azaltmada yardımcı olabileceğini savunurum. Yine de, her modelde olduğu gibi, uygulama sırasında dikkatli olursak sorun yaşamamamız gerekir.


