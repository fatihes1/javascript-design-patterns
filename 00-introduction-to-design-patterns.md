# Bölüm 1. Tasarım Desenlerine Giriş

> İyi kod, onu sürdürecek olan sonraki geliştiriciye yazılmış bir aşk
> mektubu gibidir!

Tasarım desenleri, kodu yapılandırmak için ortak bir kelime dağarcığı sağlar ve bu sayede anlaşılması daha kolay hale gelir. Bu, diğer geliştiricilere olan bu bağlantının kalitesini artırmaya yardımcı olur. Tasarım desenlerini bilmek, gereksinimlerde tekrarlayan temaları tanımamıza ve onları kesin çözümlere eşlememize yardımcı olur. Benzer bir sorunla karşılaşmış ve optimize edilmiş bir yöntem geliştirmiş diğer kişilerin deneyimlerine güvenebiliriz. Bu bilgi, kodu sürdürülebilir hale getirmek için kod yazma veya yeniden düzenleme yolunu açtığı için paha biçilmezdir. 

Sunucuda veya istemcide olsun, JavaScript modern web uygulama geliştirmenin temel taşıdır. Bu kitabın önceki baskısı, JavaScript bağlamında birkaç popüler tasarım desenine odaklanmıştı. Yıllar içinde, JavaScript özellikleri ve sözdizimi açısından önemli ölçüde evrildi. Şimdi, daha önce olmayan modüller, sınıflar, ok işlevleri (array function) ve şablon dizelerini desteklemektedir. Aynı zamanda birçok web geliştiricisi için işleri kolaylaştıran gelişmiş JavaScript kütüphaneleri ve çerçeveleri bulunmaktadır. Peki, modern JavaScript bağlamında Tasarım Desenleri ne kadar relevanttir?

Geleneksel olarak tasarım desenlerinin ne önerici ne de dil özgü olduğunu belirtmek önemlidir. Onları uygun gördüğünüzde uygulayabilirsiniz, ancak zorunlu değilsinizdir. Veri yapıları veya algoritmalar gibi, modern programlama dilleri de dahil olmak üzere klasik tasarım desenlerini hala kullanabilirsiniz, örneğin JavaScript kullanarak. Bazı tasarım desenlerine modern çerçevelerde veya kütüphanelerde zaten soyutlandığı yerlerde ihtiyaç duymayabilirsiniz. Aksine, belirli desenlerin kullanımı, bazı çerçeveler tarafından teşvik edilebilir. 

Bu baskıda desenlere pragmatik bir yaklaşım benimsiyoruz. Belirli desenlerin neden belirli özellikleri uygulamak için uygun olabileceğini ve bir desenin modern JavaScript bağlamında hala önerilip önerilmediğini inceleyeceğiz. 

Uygulamalar daha etkileşimli hale geldikçe ve büyük miktarda JavaScript gerektirdikçe, dil, performans üzerindeki olumsuz etkisi nedeniyle sürekli eleştirilere maruz kaldı. Geliştiriciler, JavaScript performansını optimize edebilecek yeni desenler arıyorlar. Bu baskı, ilgili olduğu her yerde böyle geliştirmeleri vurgular. Ayrıca, React Hooks ve Higher Order Component gibi çerçeve özgü desenleri de ele alacağız ki bu desenler React.js çağında giderek daha popüler hale gelmiştir. 

Bir adım geriye gidersek, tasarım desenlerinin tarihini ve önemini keşfetmeye başlayalım. Eğer bu tarihe zaten aşina iseniz, okumaya devam etmek için "Bir Desen Nedir?" kısmına atlayabilirsiniz.

## Tasarım Desenlerinin Tarihi
Tasarım desenleri, bir mimar olan Christopher Alexander'ın erken dönem çalışmalarına kadar izlenebilir. Oftentimes, tasarım sorunlarını çözme deneyimlerini ve bunların binalar ve kasabalarla nasıl ilişkili olduğunu yazdı. Bir gün, Alexander'a belli tasarım yapılarının tekrar tekrar kullanıldığında istenen optimal etkiye yol açtığı fark edildi. 

Alexander, iki diğer mimar olan Sara Ishikawa ve Murray Silverstein ile işbirliği yaparak bir desen dili oluşturdu. Bu dil, herhangi bir ölçekte tasarım yapmak ve inşa etmek isteyen herkese yardımcı olacaktı. 1977 yılında "Bir Desen Dili" başlıklı bir makalede yayınladılar ve daha sonra tam bir sert kapaklı kitap olarak yayınladılar.

Yaklaşık 1990 yılında, yazılım mühendisleri, Alexander'ın yazdığı ilk belgeleri yazılım tasarım desenleri hakkında, becerilerini geliştirmek isteyen yeni geliştiricilere rehberlik etmek için kullanmaya başladılar. Tasarım desenlerinin arkasındaki kavramların, daha az resmi bir biçimde olsa da, programlama endüstrisinde başlangıcından beri var olduğunu belirtmek önemlidir. 

Yazılım mühendisliğinde tasarım desenleri hakkında ilk ve muhtemelen en ikonik resmi çalışmalardan biri, 1995 yılında Erich Gamma, Richard Helm, Ralph Johnson ve John Vlissides tarafından yazılan "Design Patterns: Elements of Reusable Object-Oriented Software" adlı kitaptır. Bugün çoğu mühendis, bu grubu Dört Kişilik Çete (ya da kısa adıyla GoF) olarak tanır. 

GoF yayını, tasarım desenleri kavramını alanımızda daha da ileri taşımada önemli bir role sahiptir. Bu yayın, dünya genelinde sıkça kullanılan 23 temel nesne yönelimli tasarım desenini açıklar ve birçok geliştirme tekniğini ve riskini tanımlar. Bu desenleri 6. Bölümde daha ayrıntılı olarak ele alacağız ve aynı zamanda 7. Bölümdeki tartışmalarımızın temelini oluşturacaklar.

## Desen (Pattern) Nedir?
Bir desen, yazılım tasarımındaki tekrarlayan sorunlar ve temalar için uygulayabileceğiniz yeniden kullanılabilir bir çözüm şablonudur. Diğer programlama dillerine benzer şekilde, bir JavaScript web uygulaması oluştururken, şablonu farklı durumlarda JavaScript kodunuzu yapılandırmak için kullanabilirsiniz, düşündüğünüz yerlerde yardımcı olabileceğini düşündüğünüz durumlar. 

Tasarım desenlerini öğrenmek ve kullanmak, temel olarak geliştiriciler için avantajlıdır, çünkü: 

- *Desenler kanıtlanmış çözümlerdir.* 

	- Onlar, onları tanımlamaya yardımcı olan geliştiricilerin bir araya getirdiği deneyim ve görüşlerin birleşimi sonucu ortaya çıkar. Bu, yazılım geliştirmede belirli sorunları çözerken işe yaradığı kanıtlanmış ve zaman testi geçmiş yaklaşımlardır.

- Desenler kolayca yeniden kullanılabilir. 
	- Bir desen genellikle ihtiyaçlarınıza uyacak şekilde benimseyebileceğiniz ve uyarlayabileceğiniz hazır bir çözüm sunar. Bu özellik onları oldukça sağlam kılar. 

- Desenler ifade edici olabilir. 
	- Desenler, ortak bir dil ve yapı seti kullanarak geniş çaplı sorunlara zarif çözümler sunmanıza yardımcı olabilir. 

- Desenlerin sunduğu ek avantajlar şunlardır: 
	- Desenler, uygulama geliştirme sürecinde önemli sorunlara neden olabilecek küçük sorunları engellemeye yardımcı olur. 
		- Kod oluşturmak için belirlenmiş desenleri kullandığınızda, yapıyı yanlış yapma konusunda rahatlayabilir ve genel çözümün kalitesine odaklanabilirsiniz. Desen, kodu daha yapılandırılmış ve düzenli bir şekilde yazmanızı teşvik eder, bu da gelecekte temizlik için yeniden düzenlemeye ihtiyaç duyulmasını önler. 
	- Desenler, belirli bir soruna bağlı kalmalarını gerektirmeyen şekilde belgelenmiş genelleştirilmiş çözümler sunar. 
		- Bu genelleştirilmiş yaklaşım, tasarım desenlerini, uygulama (ve birçok durumda programlama dili) ne olursa olsun, kod yapısını iyileştirmek için kullanmanızı sağlar. 

- Bazı desenler, tekrardan kaçınarak genel kod dosya boyutunu azaltabilir.
	- Tasarım desenleri, geliştiricilere çözümlerini daha yakından incelemeleri için teşvik eder, böylece tekrarları anında azaltabilecekleri alanları belirlemelerine yardımcı olur. Örneğin, benzer işlemleri gerçekleştiren fonksiyonların sayısını azaltarak kod tabanınızın boyutunu azaltmak için tek bir genelleştirilmiş fonksiyonu tercih edebilirsiniz. Bu, aynı zamanda kodu daha "dry" hale getirme olarak da bilinir. 
- Desenler, geliştiricilerin iletişimini hızlandıran bir kelime dağarcığı ekler. 
	- Geliştiriciler, takımlarıyla iletişim kurarken, tasarım desenleri topluluğunda tartışırken veya başka bir geliştirici daha sonra kodu sürdürdüğünde desene başvurabilirler.
- Popüler tasarım desenleri, bu desenleri kullanan geliştiricilerin kolektif deneyimlerini kullanarak ve topluluğa katkıda bulunarak daha da geliştirilebilir. 
	- Bazı durumlarda, bu, tamamen yeni tasarım desenlerinin oluşturulmasına yol açabilirken, diğer durumlarda belirli desenlerin kullanımıyla ilgili geliştirilmiş yönergeleri ortaya çıkarabilir. Bu, desen tabanlı çözümlerin ad hoc çözümlerden daha sağlam hale gelmesini sağlayabilir.

NOT: Desenler kesin çözümler değildir. Bir desenin rolü sadece bize bir çözüm şeması sunmaktır. Desenler tüm tasarım sorunlarını çözmez veya iyi yazılım tasarımcılarını yerine koymaz. Hala, genel tasarımı geliştirebilecek doğru desenleri seçen sağlam tasarımcılara ihtiyacınız vardır.

## Tasarım desenleri için günlük kullanım örneği
Eğer React.js kullandıysanız, muhtemelen **Provider** desenine denk gelmişsinizdir. Aksi takdirde, aşağıdaki durumu yaşamış olabilirsiniz. 

Web uygulamalarındaki bileşen ağacı, genellikle kullanıcı bilgileri veya kullanıcı erişim izinleri gibi paylaşılan verilere erişim ihtiyacı duyar. Bu işlemi JavaScript'te geleneksel olarak yapmanın yolu, bu özellikleri kök seviye bileşen için ayarlamak ve ardından onları üstten alta doğru ebeveyn-bağlı bileşenlere iletmektir. Bileşen hiyerarşisi derinleştikçe ve daha fazla iç içe geçmiş hale geldikçe, verilerinizi ilettiğiniz bileşene doğru incelemiş olursunuz, bu da prop drilling olarak bilinen bir uygulamaya yol açar. Bu, her çocuk bileşeninde tekrar tekrar tekrarlanan ve bu verilere dayanan bileşenlerdeki özellik ayarlamasını ve iletimini içeren bakımsız bir kod yapısına yol açar.

React ve birkaç diğer çerçeve, bu sorunu Provider Deseni kullanarak ele alır. Provider Deseni ile React Context API, durumu/veriyi bir bağlam sağlayıcı aracılığıyla birden çok bileşene iletebilir. Paylaşılan veriye ihtiyaç duyan alt bileşenler, bu sağlayıcıya bir bağlam tüketici olarak bağlanabilir veya useContext Hook'unu kullanabilir. 

Bu, çok yaygın bir sorunun çözümünü optimize etmek için kullanılan bir tasarım deseni mükemmel bir örneğidir. Bu kitapta bunu ve bunun gibi birçok deseni detaylı bir şekilde ele alacağız.

## Özet
Tasarım desenlerinin önemine ve modern JavaScript bağlamındaki önemine bu girişle yaklaştıktan sonra, şimdi JavaScript Tasarım Desenlerini öğrenmeye daha derinlemesine dalabiliriz. Bu kitaptaki ilk birkaç bölüm, önce desenleri yapılandırma ve sınıflandırma, ardından JavaScript için tasarım desenlerinin ayrıntılarına geçmeden önce, karşı desenleri tanımlamaktan bahseder. Ancak önce, bir önerilen "proto-desenin" bir sonraki bölümde bir desen olarak nasıl tanınacağını görelim.





