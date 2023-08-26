# Bölüm 5. Modern JavaScript Sözdizimi ve Özellikleri

JavaScript uzun yıllardır var ve birçok revizyondan geçti. Bu kitap modern JavaScript bağlamında tasarım desenlerini ele alır ve tartışılan tüm örnekler için modern ES2015+ sözdizimini kullanır. Bu bölüm, güncel JavaScript bağlamında tasarım desenlerini daha fazla tartışmak için gerekliliğini koruyan ES2015+ JavaScript özellikleri ve sözdizimini ele alır.

**Not:** ES2015 ile birlikte JavaScript sözdizimine bazı temel değişiklikler getirilmiştir ve bu değişiklikler özellikle desenler üzerine yapacağımız tartışma açısından önemlidir. Bu konular BabelJS ES2015 kılavuzunda detaylı olarak ele alınmıştır.

## Uygulamaların Bağımsızlaştırılmasının (Decoupling) Önemi

Modüler JavaScript, uygulamanızı mantıksal olarak daha küçük parçalara, modüllere, bölebilmenizi sağlar. Bir modül, diğer modüller tarafından içe aktarılabilir ve sırasıyla daha fazla modül tarafından içe aktarılabilir. Bu şekilde, uygulama birçok iç içe geçmiş modülden oluşabilir.

Ölçeklenebilir JavaScript dünyasında bir uygulamanın modüler olduğunu söylediğimizde genellikle, bunun anlamı, modüllerde saklanan, yüksek derecede bağımsız ve farklı işlevlere sahip bir dizi işlevselliğin bir araya getirildiği anlamına gelir. Gevşek bağlantı, mümkün olduğunda bağımlılıkları kaldırarak uygulamaların daha kolay bakımını sağlar. Verimli bir şekilde uygulandığında, sistemin bir bölümünde yapılan değişikliklerin başka bir bölümü nasıl etkileyebileceğini görmek mümkün olur.

Daha geleneksel programlama dillerinin aksine, JavaScript'in daha eski sürümleri (ES5'e kadar - Standart ECMA-262 5.1 Sürümü) geliştiricilere kod modüllerini düzenlemek ve içe aktarmak için temiz bir şekilde kullanabilecekleri araçlar sağlamamıştır. Bu, daha düzenli JavaScript uygulamalarına olan ihtiyaç daha belirgin hale geldiğinde daha yakın zamana kadar büyük bir düşünce gerektirmeyen spesifikasyonlarla ilgili endişelerden biriydi. İlk JavaScript sürümlerinde uygulamaları bağımsızlaştırmak için en popüler desenler AMD (Asenkron Modül Tanımı) ve CommonJS Modülleri idi. 

Bu sorunlara yönelik yerel çözümler ES6 veya ES2015 ile geldi. ECMAScript ve gelecekteki sürümlerinin sözdizimini ve semantiğini tanımlama görevi olan TC39, büyük ölçekli geliştirme için JavaScript kullanımının evrimini yakından takip etmekteydi ve daha modüler JS yazmak için daha iyi dil özelliklerine olan ihtiyacın farkındaydı. JavaScript'te modül oluşturmak için sözdizimi, ES2015'te ECMAScript Modülleri'nin yayınlanmasıyla geliştirildi ve standartlaştırıldı. Bugün, tüm büyük tarayıcılar JavaScript modüllerini desteklemektedir. Bu, JavaScript'te modern modüler programlamanın de facto yöntemi haline gelmiştir. Bu bölümde, ES2015+'daki modül söz dizimini kullanarak kod örneklerini inceleyeceğiz.

## İçe Aktarmalar (Import)  ve Dışa Aktarmalar (Export) ile Modüller

Modüller, uygulama kodumuzu bağımsız birimlere, her biri işlevselliğin bir yönü için kod içeren birimlere ayırmamıza olanak tanır. Modüller aynı zamanda kodun tekrar kullanılabilirliğini teşvik eder ve farklı uygulamalara entegre edilebilecek özellikleri ortaya koyar. Bir dil, modüler programlamayı desteklemek için modül bağımlılıklarını içe aktarmak ve modül arayüzünü (diğer modüllerin tüketebileceği genel API/değişkenler) dışa aktarmak için özelliklere sahip olmalıdır. JavaScript Modüllerinin (ayrıca ES Modülleri olarak da adlandırılır) desteklenmesi ES2015'te JavaScript'e tanıtıldı. Bu, modül bağımlılıklarını belirtmek için `import` anahtar kelimesini kullanmanıza olanak sağlar. Benzer şekilde, modülün içinden hemen hemen her şeyi dışa aktarmak için `export` anahtar kelimesini kullanabilirsiniz.

- `import` ifadeleri, bir modülden dışarı aktarılan (export edilen) yerel değişkenler olarak bağlar ve ad çakışmalarını/çatışmalarını önlemek veya adlandırmak için yeniden adlandırılabilir. 

- `export` ifadeleri, bir modülün yerel bir bağlamını dışa doğru görünür hale getirir, böylece diğer modüller bu export edilen değerleri okuyabilir ancak değiştiremez. İlginç bir şekilde, modüller alt modüller export edebilir ancak dışarıda tanımlanmış modülleri export edemez. Ayrıca, export edilen değerleri yeniden adlandırabiliriz, böylece dış adları yerel adlardan farklı olabilir.

**Not** `.mjs`, JavaScript modüllerini klasik komut dosyalarından (`.js`) ayırt etmemize yardımcı olan bir uzantıdır. `.mjs` uzantısı, ilgili dosyaların çalışma zamanları ve derleme araçları tarafından bir modül olarak ayrıştırılmasını sağlar. (Örneğin, Node.js, Babel)

Aşağıdaki örnek, fırın personeli için üç modülü, fırında yaparken gerçekleştirdikleri işlevleri ve fırını içermektedir. Bir modül tarafından export edilen işlevselliklerin diğer modül tarafından içe aktarıldığını yani import edildiğini ve kullanıldığını görebiliriz.

```javascript
// Dosya Adı: staff.mjs // ========================================= 
// specify (public) exports that can be consumed by other // modules 
export const baker = {
bake(item) {
console.log( `Woo! I just baked ${item}` );
    }
};
```

```javascript
// Dosya Adı: cakeFactory.mjs // ========================================= 
// specify dependencies import baker from "/modules/staff.mjs"; 
export const oven = {
  makeCupcake(toppings) {
    baker.bake( "cupcake", toppings ); },
  makeMuffin(mSize) {
    baker.bake( "muffin", size ); }
}
```

```javascript
// Dosya Adı: bakery.mjs 
// ========================================= 
import {cakeFactory} from "/modules/cakeFactory.mjs";
cakeFactory.oven.makeCupcake( "sprinkles" );
cakeFactory.oven.makeMuffin( "large" );
```

Genellikle bir modül dosyası birkaç ilişkili fonksiyon, sabit ve değişken içerir. Bu kaynakları toplu halde export etmek için dosyanın sonunda tek bir export ifadesi kullanabilirsiniz. Export etmek istediğiniz modül kaynaklarını virgülle ayrılmış bir liste şeklinde belirtebilirsiniz.

```javascript
// Dosya Adı: staff.mjs 
// ========================================= 
const baker = { //baker functions }; 
const pastryChef = { //pastry chef functions }; 
const assistant = { //assistant functions }; export { baker, pastryChef, assistant };
```

Benzer şekilde, yalnızca ihtiyacınız olan işlevleri içe aktarabilirsiniz.

```javascript
import {baker, assistant} from "/modules/staff.mjs";
```

JavaScript modüllerini içeren `script` etiketlerini tarayıcılara kabul ettirmek için `type` öz niteliğini `module` değeriyle belirterek söyleyebilirsiniz:

```html
<script type="module" src="main.mjs"></script>
<script nomodule src="fallback.js"></script>
```

`nomodule` özniteliği, modern tarayıcıların klasik bir betiği modül olarak yüklememesi gerektiğini belirtir. Bu, modül sözdizimini kullanmayan yedek betikler için kullanışlıdır. Bu, HTML içinde modül sözdizimini kullanmanıza ve bunun, desteklemediği tarayıcılarda çalışmasını sağlamanıza olanak tanır. Bu, performans dahil birkaç nedenle kullanışlıdır. Modern tarayıcılar modern özellikler için polifil gerekmezken, daha büyük çevrilmiş kodu sadece eski tarayıcılara sunmanıza olanak tanır.

## Modül Nesneleri
Modül kaynaklarını içe aktarmanın ve kullanmanın daha temiz bir yaklaşımı, modülü bir nesne olarak içe aktarmaktır. Bu, tüm dışa aktarmaları nesne üyeleri olarak kullanılabilir hale getirir.
```javascript
// Dosya Adı: cakeFactory.mjs
import * as Staff from "/modules/staff.mjs";
export const oven = {
    makeCupcake(toppings) {
        Staff.baker.bake("cupcake", toppings);
    },
    makePastry(mSize) {
        Staff.pastryChef.make("pastry", type);
    }
}
```

## Uzaktan (Remote) Kaynaklardan Yüklenen Modüller
ES2015+ ayrıca uzaktan modülleri (örneğin, üçüncü taraf kütüphaneleri) destekler, böylece dışarıdan modülleri yüklemek daha basit hale gelir. İşte yukarıda tanımladığımız modülü çekerek ve kullanarak bir örnek:
```javascript
import { cakeFactory } from "https://example.com/modules/cakeFactory.mjs"; 
// eagerly loaded static import
cakeFactory.oven.makeCupcake("sprinkles");
cakeFactory.oven.makeMuffin("large");
```

## Statik İçe Aktarmalar
Yukarıda bahsedilen içe aktarma türüne statik içe aktarma denir. Modül grafiği, ana kod çalıştırılmadan önce statik içe aktarma ile indirilmeli ve yürütülmelidir. Bu bazen başlangıç sayfasının yüklenmesiyle birlikte önemli miktarda kodun hızla yüklenmesine ve önemli özelliklerin daha erken kullanılabilir hale gelmesinin gecikmesine yol açabilir.
```javascript
import { cakeFactory } from "/modules/cakeFactory.mjs"; 
// eagerly loaded static import
cakeFactory.oven.makeCupcake("sprinkles");
cakeFactory.oven.makeMuffin("large");
```

## Dinamik İçe Aktarmalar
Bazen bir modülü önceden yüklemek istemeyebilirsiniz, ancak ihtiyaç duyulduğunda gerektiğinde yüklemek isteyebilirsiniz. Tembel yükleme (lazy-loading) modüllerin ihtiyaç duyulduğunda yüklenmesine izin verir. Örneğin, kullanıcı bir bağlantıya veya düğmeye tıkladığında. Bu, başlangıç yükleme performansını iyileştirir. Dinamik içe aktarma bu olasılığı mümkün kılar.
Dinamik içe aktarma, bir fonksiyon gibi yeni bir içe aktarma biçimi tanıtır.
`import(url)`, istenen modülün modül ad alanı nesnesi için bir söz veri döndürür. Bu nesne, modülün bağımlılıklarının hepsinin alınması, örneklendirilmesi ve değerlendirilmesi, aynı zamanda modülün kendisi dahil olmak üzere, gerçekleştirildikten sonra oluşturulur.
Aşağıda, cakeFactory modülü için dinamik içe aktarmaları gösteren bir örnek bulunmaktadır.
```javascript
form.addEventListener("submit", e => {
  e.preventDefault();
  import("/modules/cakeFactory.js")
    .then((module) => {
      // Modülle bir şey yapın.
      module.oven.makeCupcake("sprinkles");
      module.oven.makeMuffin("large");
    });
});
```
Dinamik içe aktarma aynı zamanda await anahtar kelimesi kullanılarak da desteklenebilir:
```javascript
let module = await import("/modules/cakeFactory.js");
```
Dinamik içe aktarma ile modül grafiği, modül kullanıldığında yalnızca indirilir ve değerlendirilir.
Dinamik İçe Aktarma özelliği kullanılarak Vanilla JavaScript'te İnteraksiyonla İçe Aktarma ve Görünürlükle İçe Aktarma gibi yaygın desenler kolayca uygulanabilir.

### İnteraksiyonla İçe Aktarma
Bazı kütüphaneler, kullanıcının web sayfasındaki belirli bir özellikle etkileşime girmeye başladığı durumda gerekebilir. Tipik örnekler sohbet widget'ları, karmaşık iletişim kutuları veya video gömme özellikleridir. Bu tür özellikler için kütüphaneleri sayfa yükleme sırasında içe aktarmak gerekli değildir, bunlar kullanıcı bu özelliklerle etkileşimde bulunduğunda yüklenebilir (örneğin, bileşen yüzeyine veya yer tutucuya tıklamak gibi). Eylem, ilgili kütüphanelerin dinamik olarak içe aktarılmasını tetikleyebilir ve ardından istenen işlevselliği etkinleştirmek için işlev çağrısını yapabilir.

Örneğin, dışarıdan getirilen lodash.sortby modülünü kullanarak ekrandaki sıralama işlevini uygulayabilirsiniz. Bu modül dinamik olarak yüklenir.
```javascript
const btn = document.querySelector('button');
btn.addEventListener('click', e => {
  e.preventDefault();
  import('lodash.sortby')
    .then(module => module.default)
    .then(sortInput()) // getirilen bağımlılığı kullanın
    .catch(err => { console.log(err) });
});
```
Bu şekilde, kullanıcının etkileşimde bulunduğu özellikler için kütüphaneleri yalnızca ihtiyaç duyulduğunda yüklemiş olursunuz, bu da başlangıç yükleme süresini azaltır ve performansı artırır.

### Görünürlüğe Göre İçe Aktarma
Birçok bileşen başlangıç sayfa yükleme sırasında görünmez olabilir, ancak kullanıcı aşağı kaydırdıkça görünür hale gelir. Kullanıcılar her zaman aşağı kaydırmayabilir, bu nedenle bu bileşenlere karşılık gelen modüller, görünür hale geldiklerinde tembelce yüklenir. IntersectionObserver API, bir bileşen yer tutucusunun yakında görünür hale geleceğini tespit edebilir ve Dinamik içe aktarma ilgili modülleri yükleyebilir.

```javascript
// Intersection Observer kullanarak bileşen yer tutucusunun görünürlüğünü gözlemleme
const observer = new IntersectionObserver(entries => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      import('modulun-yolu')
        .then(module => module.default)
        .then(initComponent()) // bileşeni başlatın
        .catch(err => { console.log(err) });
      observer.unobserve(entry.target); // görünürlüğü gözlemlemeyi durdur
    }
  });
});

// Bileşen yer tutucusunu gözlemlemeye başla
const placeholder = document.querySelector('.bilesen-yer-tutucusu');
observer.observe(placeholder);
```

Bu şekilde, kullanıcı sayfayı aşağı kaydırdıkça bileşenler yavaşça yüklenir ve performansı artırır. IntersectionObserver API, görünürlük durumunu izleyerek doğru anlarda modülleri yüklemenizi sağlar.

## Sunucu İçin Modüller
Node'un 15.3.0 sürümünden itibaren JavaScript modüllerini desteklemektedir. Bu modüller deneysel bir bayrak olmadan çalışır ve npm paket ekosistemi ile uyumludur. Node, .mjs ile biten dosyaları ve üst düzey bir "type" alanı değeri olarak "module" içeren .js dosyalarını JavaScript Modülleri olarak işler.

```json
{
  "name": "js-modules",
  "version": "1.0.0",
  "description": "JS Modülleri kullanan bir paket",
  "main": "index.js",
  "type": "module",
  "author": "",
  "license": "MIT"
}
```

Bu şekilde bir "package.json" dosyası, Node'un modülü olarak çalıştırılacak paketin belirlenmesini sağlar. "type" alanı "module" olarak ayarlandığında, .mjs ve .js dosyaları JavaScript modüller olarak kabul edilir. Bu sayede sunucu tarafında da modüler yapılar kullanılabilir hale gelir.

## Modüllerin Kullanılmasının Avantajları

Modüllerin kullanımı ve modüler programlama, birkaç benzersiz avantaj sunar. Bunlardan bazıları aşağıda sıralanmıştır:

1. **Tek Seferlik Değerlendirme**: Tarayıcı, modül komut dosyalarını yalnızca bir kez değerlendirir. Buna karşılık, klasik komut dosyaları DOM'a her eklendiklerinde yeniden değerlendirilir. Bu, bağımlılıkların daha verimli şekilde çözümlendiği ve iç içe geçmiş bağımlılıklı modüllerde, içerideki modülün önce değerlendirildiği bir yapıyı beraberinde getirir. Bu, iç içe geçmiş modülün önce değerlendirilmesi anlamına gelir ve bağımlı modüllerin ihracatına erişim sağlar.

2. **Otomatik Bekletilen Yükleme**: Diğer komut dosyalarıyla karşılaştırıldığında, modül komut dosyalarının yüklemesi otomatik olarak bekletilir. Bu, daha iyi performans ve kaynak yönetimi sağlar.

3. **Bakım ve Yeniden Kullanım Kolaylığı**: Modüller kodun parçalanmasını teşvik eder, böylece bağımsız parçalar diğer modüller üzerinde büyük değişiklikler yapmadan ayrı ayrı bakımlanabilir. Ayrıca aynı kodu farklı işlevlerde yeniden kullanmanıza olanak tanır.

4. **İsim Alanları Oluşturma**: Modüller, ilgili değişkenler ve sabitler için özel bir alan oluşturur. Bu, global isim alanının kirletilmesini engeller ve isim çakışmalarının önüne geçer.

5. **Ölü Kod Elemesi**: Geleneksel komut dosyalarıyla kullanılmayan kodun kaldırılması manuel bir çaba gerektiriyordu. Modüller, Webpack ve Rollup gibi paketleyicilerin kullanılmayan modülleri otomatik olarak tanımlamasını ve bunları elemesini sağlar. Bu işlem, "ağaç sallama" olarak bilinir ve nihai paketin verimliliğini artırır.

6. **Tüm Modern Tarayıcılar Desteği**: Tüm modern tarayıcılar, modül içe ve dışa aktarmayı nesnel bir şekilde destekler ve herhangi bir geriye dönüş mekanizması gerektirmez.

Özetlemek gerekirse, modüller, kod organizasyonu, bakım ve performans açısından modern JavaScript uygulamalarına bir dizi avantaj sunar.

## Constructors, Getters ve Setters ile Sınıflar
Modüllerin yanı sıra, ES2015+'ın bir parçası olarak sınıfları oluşturmanızı ve bir dereceye kadar gizlilik sağlamanızı da sağlar. JavaScript sınıfları class anahtar kelimesiyle tanımlanır. Aşağıdaki örnekte, bir constructor ve iki getter ve setter içeren Cake adında bir sınıf tanımlıyoruz:

```javascript
class Cake {
    // Sınıf constructor fonksiyonunun gövdesini, class değişkenlerinin bir listesi ile kullanarak tanımlayabiliriz.
    constructor(name, toppings, price, cakeSize) {
        this.name = name;
        this.cakeSize = cakeSize;
        this.toppings = toppings;
        this.price = price;
    }

    // ES2015+'ın gereksizce her şeyi fonksiyon olarak kullanımını azaltma çabası olarak, aşağıdaki gibi bazı durumlarda kullanılmadığını fark edeceksiniz.
    // Bir tanımlayıcı, argüman listesi ve bir gövdeye sahip olan bir yöntem tanımlar.
    addTopping(topping) {
        this.toppings.push(topping);
    }

    // Getters, tanımlayıcı / yöntem adından önce get ifadesinin kullanılmasıyla tanımlanabilir ve bir süslü parantez içeriği ile birlikte gelir.
    get allToppings() {
        return this.toppings;
    }

    get qualifiesForDiscount() {
        return this.price > 5;
    }

    // Getter'lara benzer şekilde, setter'lar bir tanımlayıcı önünde set anahtar kelimesini kullanarak tanımlanabilir.
    set size(size) {
        if (size < 0) {
            throw new Error("Pastanın geçerli bir boyut olması gerekmektedir: küçük, orta veya büyük");
        }
        this.cakeSize = size;
    }
}

// Kullanım
let cake = new Cake("çikolata", ["çikolata parçaları"], 5, "büyük");
```

JavaScript sınıfları prototiplere dayanır ve başvurulmadan önce tanımlanmaları gereken JavaScript fonksiyonlarının özel bir kategorisidir.

Ayrıca, bir sınıfın başka bir sınıftan miras aldığını belirtmek için extends anahtar kelimesini de kullanabilirsiniz.

```javascript
class BirthdayCake extends Cake {
    surprise() {
        console.log(`Mutlu Yıllar!`);
    }
}

let birthdayCake = new BirthdayCake("çikolata", ["çikolata parçaları"], 5, "büyük");
birthdayCake.surprise();
```

Tüm modern tarayıcılar ve Node, ES2015 sınıflarını destekler. Ayrıca, ES6'da tanıtılan yeni stil sınıf sözdizimiyle de uyumludur.

JavaScript Modüller ve Sınıflar arasındaki fark, Modüllerin içe aktarıldığı ve dışa aktarıldığı, Sınıfların ise class anahtar kelimesiyle tanımlandığıdır. Yukarıdaki örneklerde 'function' kelimesinin eksik olduğunu fark etmiş olabilirsiniz. Bu bir yazım hatası değildir: TC39, kod yazma şeklimizi basitleştirmeye yardımcı olabileceğini umarak her şey için fonksiyon anahtar kelimesini kötüye kullanma eğilimimizi azaltmak için bilinçli bir çaba harcamıştır.

JavaScript sınıfları aynı zamanda super anahtar kelimesini destekler. Bu, üst sınıfın yapıcı yöntemini çağırmanıza izin verir. Bu, kendinden miras alma desenini uygulamak için kullanışlıdır. super'ı, üst sınıfın yöntemlerini çağırmak için kullanabilirsiniz.

```javascript
class Cookie {
    constructor(flavor) {
        this.flavor = flavor;
    }
    showTitle() {
        console.log(`Bu kurabiyenin tadı ${this.flavor}.`);
    }
}

class FavoriKurabiye extends Cookie {
    showTitle() {
        super.showTitle();
        console.log(`${this.flavor} harika.`);
    }
}

let benimKurabiyem = new FavoriKurabiye('çikolata');
benimKurabiyem.showTitle();
// Bu kurabiyenin tadı çikolata.
// çikolata harika.
```

Modern JavaScript, genel ve özel sınıf üyelerini destekler. Genel sınıf üyeleri diğer sınıflara erişilebilir. Özel sınıf üyeleri yalnızca tanımlandığı sınıfa erişilebilir. Sınıf alanları, varsayılan olarak geneldir. Özel sınıf alanları, # işareti öneki kullanılarak oluşturulabilir.

```javascript
class CookieWithPrivateField { 
	#privateField; 
} 
class CookieWithPrivateMethod { 
	#privateMethod() { 
		return 'delicious cookies'; 
		} 
	}
```

JavaScript sınıfları, statik yöntemleri ve özellikleri static anahtar kelimesini kullanarak destekler. Statik üyeler, sınıf örneği oluşturmadan referans alınabilir. Statik yöntemleri yardımcı işlevler oluşturmak için ve statik özellikleri yapılandırma veya önbelleğe alınmış verileri tutmak için kullanabilirsiniz.

```javascript
class Cookie {
    constructor(flavor) {
        this.flavor = flavor;
    }
    static brandName = "En İyi Fırın";
    static discountPercent = 5;
}

console.log(Cookie.brandName); // Çıktı = "En İyi Fırın"
```

Static yöntemler ve özellikler, sınıf örneği oluşturmadan doğrudan sınıf üzerinden erişilebilir.

## JavaScript Frameworklarında Sınıflar
Son birkaç yıl içinde, bazı modern JavaScript kütüphaneleri ve çerçeveleri - özellikle React - sınıflara alternatifler sunmuştur. React Hooks, ES2015 sınıf bileşeni olmadan React durumu ve yaşam döngü yöntemlerini kullanmanıza olanak tanır. Hooks olmadan, React geliştiricilerinin işlevsel bileşenleri sınıf bileşenleri olarak yeniden düzenlemeleri gerekiyordu, böylece durumu ve yaşam döngü yöntemlerini yönetebilirlerdi. Bu genellikle zordu ve ES2015 sınıflarının nasıl çalıştığını anlamayı gerektirirdi. React Hooks, bileşenin durumunu ve yaşam döngü yöntemlerini sınıflara dayanmadan yönetmenizi sağlayan işlevlerdir.
Unutmayın ki Web Bileşenleri topluluğu gibi web için geliştirme yaklaşımlarında, bileşen geliştirmek için temel olarak sınıfları kullanmaya devam eden birçok diğer yaklaşım vardır.


## Özet
Bu bölüm, Modüller ve Sınıflar için JavaScript dilindeki sözdizimini tanıttı. Bu özellikler, nesne yönelimli tasarım ve modüler programlama prensiplerine uygun olarak kod yazmamıza olanak tanır. Bu kavramları farklı tasarım desenlerini sınıflandırmak ve tanımlamak için kullanacağız. Bir sonraki bölüm farklı tasarım deseni kategorileri hakkında konuşuyor.

## İlgili Okuma
[JavaScript Modülleri v8 üzerinde](https://v8.dev/features/modules)
[JavaScript Modülleri MDN üzerinde](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules)
