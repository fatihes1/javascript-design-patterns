# Bölüm 3. Yapılandırma ve Yazma Kalıpları

Bir yeni fikrin başarısı, kullanışlılığına ve aynı zamanda ona yardım etmeye çalıştığı kişilere nasıl sunduğunuza bağlıdır. Geliştiricilerin bir tasarım desenini anlamaları ve benimsemeleri için, desenin bağlamı, koşulları, önkoşulları ve önemli örnekleri hakkında ilgili bilgilerle sunulması gerekir. Bu bölüm, belirli bir deseni anlamaya çalışanlar ve yeni bir deseni tanıtmaya çalışanlar için geçerlidir çünkü desenlerin nasıl yapılandırıldığı ve yazıldığı konusunda temel bilgiler sunar.

## Bir Tasarım Deseninin Yapısı 
Desen yazarları, desenin amacını tanımlayamazlarsa deseni başarıyla oluşturamaz ve yayınlayamazlar. Benzer şekilde, geliştiriciler geçmiş deneyim veya bağlama sahip değillerse bir deseni anlamak veya uygulamak zor olabilir. Desen yazarları, yeni bir desenin tasarımını, uygulamasını ve amacını belirlemelidir. Yazarlar, başlangıçta yeni bir deseni, şu unsurlar arasında bir ilişki kurarak sunar: 
- Bir bağlam 
- O bağlamda ortaya çıkan bir kuvvet sistemi 
- Bu kuvvetlerin bağlamda çözülmesine izin veren bir yapılandırma 

Bu bilgiler ışığında, şimdi bir tasarım deseni için bileşen öğelerini özetleyelim. Bir tasarım deseni aşağıdakilere sahip olmalıdır ve ilk beş unsur en önemli olanlardır:
- **Desen adı**: Desenin amacını temsil eden benzersiz bir ad. 
- **Açıklama**: Desenin ne tür başarılar elde etmeye yardımcı olduğuna dair kısa bir açıklama. 
- **Bağlam**: taslağı Desenin etkili bir şekilde kullanıcı ihtiyaçlarına nasıl yanıt verdiğinin bağlamları. 
- **Problem açıklaması**: Desenin niyetini anlamamızı sağlamak için ele alınan sorunun bir açıklaması. Çözüm Kullanıcının problemi nasıl çözdüğünü anlaşılır bir adım ve algılar listesi şeklinde açıklama. 
- **Tasarım**: Desenin tasarımının ve özellikle kullanıcının etkileşimde bulunduğu davranışın açıklaması.
- **Uygulama**: Geliştiricilerin deseni nasıl uygulayacaklarını gösteren bir rehber. 
- **Görseller**: Desendeki sınıfların görsel temsilleri (örneğin, bir diyagram). 
- **Örnekler**: Desenin asgari bir şekilde uygulanmış örneklemeleri. 
- **Çekirdek/Temel Koşullar**: Tanımlanan desenin kullanımını desteklemek için hangi diğer desenlere ihtiyaç duyulabilir? 
- **İlişkiler**: Bu desen hangi diğer desenlere benzemektedir? Başka hangi desenlere yakın bir şekilde benzemektedir?
- **Bilinen Kullanım**: Desen gerçek dünyada kullanılıyor mu? Eğer öyleyse, nerede ve nasıl? 
- **Tartışmalar**: Takım veya yazarın desenin heyecan verici faydalarına dair düşünceleri.

## İyi Yazılmış Desenler 
Bir tasarım deseninin yapısını ve amacını anlamak, neden bir desene ihtiyaç duyulduğu konusundaki mantığı daha derinlemesine anlamamıza yardımcı olabilir. Aynı zamanda deseni kendi ihtiyaçlarımız doğrultusunda değerlendirmemize yardımcı olur. 

İyi bir desen, ideal olarak son kullanıcılar için yeterli miktarda referans materyali sağlamalıdır. Desenler ayrıca neden gereklilikleri olduğuna dair kanıtlar sunmalıdır. 

Bir desenin genel bir bakışını sahip olmak, günlük olarak karşılaşabileceğimiz kod içinde onları tanımamıza yardımcı olmak için yeterli değildir. Gördüğümüz bir kod parçasının belirli bir deseni takip edip etmediği veya tesadüfen bir deseni benzeyip benzemediği her zaman açık değildir. 

Gördüğünüz kodun bir deseni kullandığını düşünüyorsanız, kodun belirli bir mevcut desen veya desen kümesine giren bazı yönlerini yazmayı düşünün. Kodun, rastlantısal olarak belirli bir desinin kurallarıyla örtüşen sağlam prensipleri ve tasarım uygulamalarını takip ettiği olabilir.

***İpucu: İnteraksiyonlar veya tanımlanmış kurallar görünmeyen çözümler desenler değildir.***

Desenlerin planlama ve yazım aşamalarında yüksek bir başlangıç maliyeti olabilir; ancak bu yatırımdan elde edilen değer buna değebilir. Desenler, çözümler oluştururken veya sürdürürken organizasyon veya ekip içindeki tüm geliştiricilerin aynı sayfada olmalarına yardımcı oldukları için değerlidir. Kendi bir desen üzerinde çalışmayı düşünüyorsanız, önceden araştırma yapın, çünkü mevcut, kanıtlanmış desenleri kullanmak veya genişletmek, sıfırdan başlamaktan daha faydalı olabilir.

## Bir Desen Yazmak 
Eğer kendi bir tasarım deseni geliştirmeye çalışıyorsanız, süreci zaten geçmiş ve iyi bir şekilde tamamlamış olanlardan öğrenmeyi öneririm. Farklı tasarım deseni açıklamalarından gelen bilgileri emmek ve size anlamlı gelenleri içselleştirmek için zaman harcayın. Yapıyı ve semantikleri keşfedin - bunu, ilgilendiğiniz desenlerin etkileşimlerini ve bağlamını inceleyerek yapabilirsiniz; böylece bu desenleri değerli yapılandırmalarda bir araya getirme konusunda yardımcı olan prensipleri belirleyebilirsiniz. 

Mevcut formattan yararlanarak kendi deseninizi yazabilir veya fikirlerinizi entegre ederek iyileştirebilecek yolları görebilirsiniz. Son yıllarda bunu yapan bir geliştirici örneği, mevcut Modül desenini alıp temelde değerli değişiklikler yaparak Açığa Çıkan Modül desenini *(Revealing Module pattern)* oluşturan Christian Heilmann'dır. Yeni bir tasarım deseni oluşturmayı veya mevcut bir deseni uyarlamayı düşünüyorsanız aşağıdaki kontrol listesine uymak yardımcı olur:

- **Desen ne kadar pratik?** Desenin, sadece belirli sorunlara dair spekülatif çözümleri değil, kanıtlanmış tekrarlayan sorunlara dair çözümleri tanımladığından emin olun. 
- **En iyi uygulamaları göz önünde bulundurun.** Tasarım kararlarımızı, en iyi uygulamaları anlamaktan türettiğimiz prensiplere dayandırmalıyız. 
- **Tasarım desenlerimiz kullanıcıya karşı şeffaf olmalıdır.** Tasarım desenleri, son kullanıcı deneyimine tamamen şeffaf olmalıdır. Öncelikle kullanan geliştiricilere hizmet ederler ve beklenen kullanıcı deneyimine değişiklik yapmamalıdırlar. 
- **Unutmayın ki orijinallik desen tasarımında temel değildir.** Bir desen yazarken, belgelenmiş çözümlerin orijinal keşfedicisi olmanız gerekmez, ayrıca tasarımınızın diğer desenlerin küçük parçalarıyla örtüşmesi konusunda endişelenmeniz gerekmez. Yaklaşım yeterince güçlü ise geniş uygulanabilirliği varsa, geçerli bir desen olarak tanınma şansı vardır. 
- **Desenlerin güçlü bir örnek setine ihtiyacı vardır.** İyi bir desen açıklamasını, desenin başarılı uygulamasını gösteren eşit derecede etkili bir örnek setinin takip etmesi gereklidir. Geniş kullanımı göstermek için, sağlam tasarım prensiplerini sergileyen örnekler idealdir.

Desen yazma, genel, özgün ve en önemlisi kullanışlı bir tasarım oluşturma arasında dikkatli bir denge gerektirir. Bir desen yazarken mümkün olan tüm uygulama alanlarını kapsamak için kapsamlı bir şekilde ele aldığınızdan emin olmaya çalışın.

Desen yazıp yazmamanız fark etmeksizin, umarım bu kısa desen yazma girişinin, öğrenme sürecinize yardımcı olacak bazı iç görüler sunmuş ve kitabın devamındaki bölümlerde ele alınan desenleri anlamanıza yardımcı olmuştur.

## Özet
Bu bölüm, ideal bir "iyi" desenin nasıl olduğuna dair bir resim çizdi. Aynı şekilde, "kötü" desenlerin de var olduğunu anlamak, onları tanımlayıp kaçınabilmek için eşit derecede önemlidir. İşte bu nedenle bir sonraki bölümde "Karşı Desenler" ***(Anti-Patterns)*** hakkında bilgi bulunmaktadır.


