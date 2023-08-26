# Bölüm 2. “Örüntü” Testi (Pattern Test), Proto-Örüntüler ve Üç Kuralı
Yeni bir desen önerildiği andan itibaren potansiyel yaygın kabulüne kadar, bir desen tasarım topluluğu ve yazılım geliştiricileri tarafından birden çok aşamalı derinlemesine bir inceleme sürecinden geçebilir. Bu bölüm, bir "Proto-Desen"nin "desen" olarak tanınana kadar geçirdiği bu yolculuğu, Üç Kuralı karşılarsa sonunda bir desen olarak tanınmasını anlatır. 

Bu ve sonraki bölüm, yeni oluşturulan tasarım desenlerini yapılandırma, yazma, sunma ve gözden geçirme yaklaşımını keşfeder. Eğer öncelikle belirlenmiş tasarım desenlerini öğrenmeyi tercih ederseniz, şu an için bu iki bölümü atlayabilirsiniz.

## Proto-Patterns nedir? 
Unutmayın ki her algoritma, en iyi uygulama veya çözüm, tamamlanmış bir desen olarak kabul edilebilecek şeyi temsil etmez. Birkaç temel unsur eksik olabilir ve desen topluluğu genellikle kapsamlı ve eleştirel bir değerlendirme olmadan bir şeyin bir desen olarak iddia edilmesinden çekinir. Bir şeyin bize sunulsa da ve bir desen için gerekli kriterleri karşıladığı görünse bile, başkaları tarafından uygun bir süre boyunca incelenmemiş ve test edilmemişse onu bir desen olarak düşünmemeliyiz. 

Alexander'ın çalışmalarına bir kez daha dönersek, bir desenin hem bir süreç hem de bir "şey" olması gerektiğini belirtir. Bu tanım, sürecin "şey"i yaratması gerektiğini belirttiği için anlaşılması güçtür. Bu nedenle desenler genellikle görsel olarak tanımlanabilir bir yapıyı ele almayı amaçlar; deseni uyguladığınızda ortaya çıkan yapıyı temsil eden bir resmi (veya çizimi) görsel olarak oluşturabilmeliyiz.

## "Desen" Testleri 

Tasarım desenlerini incelediğinizde sıkça "proto-desen" terimini görebilirsiniz. Bu nedir? Bir desen henüz kesin olarak "desen" testlerini geçmemişse, genellikle proto-desen olarak adlandırılır. Proto-desenler, belirli bir çözümü toplulukla paylaşmaya değer bulan birinin çalışmasından kaynaklanabilir. Ancak, nispeten genç yaşına rağmen, topluluk önerilen çözümü uygun şekilde değerlendirme fırsatına sahip olmamış olabilir. 

Alternatif olarak, deseni paylaşan birey(ler) "desen" sürecinden geçme zamanı veya ilgisi olmayabilir ve bu nedenle proto-desenlerinin kısa bir açıklamasını yayınlayabilirler. Bu tür desenlerin kısa açıklamalarına veya parçalarına patlet adı verilir. Nitelikli bir deseni eksiksiz olarak belgelemek için gereken çalışma oldukça korkutucu olabilir. Tasarım desenleri alanındaki bazı en eski çalışmalara geri döndüğümüzde, bir desen aşağıdakileri yapıyorsa "iyi" olarak kabul edilebilir:

- Belirli bir problemi çözer. 
	- Desenler sadece prensipleri veya stratejileri yakalamak için değil, aynı zamanda çözümleri yakalamak için de tasarlanmıştır. Bu, iyi bir desen için en temel bileşenlerden biridir. 
- Açık bir çözümü yoktur. 
	- Problem çözme tekniklerinin genellikle iyi bilinen ilk prensiplerden türetmeye çalıştığımızı görebiliriz. En iyi tasarım desenleri genellikle sorunlara dolaylı olarak çözümler sunar - bu, tasarımla ilgili en zorlu problemler için gerekli bir yaklaşım olarak kabul edilir. 
- Kanıtlanmış bir kavramı açıklar. 
	- Tasarım desenleri, tanımlandığı gibi çalıştıklarını kanıtlamalarını gerektirir ve bu kanıt olmadan tasarım ciddi şekilde ele alınamaz. Eğer bir desen doğası gereği oldukça spekülatifse, sadece cesur olanlar onu kullanmaya teşebbüs eder.
- Bir ilişki açıklar. 
	- Bazı durumlarda, bir desenin bir tür modülü açıkladığı görünebilir. Uygulama nasıl görünürse görünsün, desenin resmi açıklaması, kod ile olan ilişkisini açıklayan çok daha derin sistem yapılarını ve mekanizmalarını tanımlamalıdır. 

Kılavuzları karşılamayan bir proto-desenin öğrenmeye değer olmadığını düşünmemiz bağışlanabilir; ancak bu gerçekten uzak bir ihtimaldir. Birçok proto-desen aslında oldukça iyidir. Tüm proto-desenlerin incelenmeye değer olduğunu söylemiyorum, ancak gelecekteki projelerimize yardımcı olabilecek birçok kullanışlı proto-desen vardır. Yukarıdaki liste göz önünde bulundurularak en iyi değerlendirmenizi yapın, seçim sürecinizde başarılı olacaksınız.

## Üç Kuralı 
Bir desenin geçerli olması için ek gereksinimlerden biri, tekrarlayan bazı olguları göstermeleridir. Bu genellikle en az üç ana alanda belirlenebilir ve buna Üç Kuralı denir. Bu kuralı kullanarak tekrarlamayı göstermek için şunları kanıtlamak gereklidir:
- **Amaç Uygunluğu**: Desen nasıl başarılı olarak kabul edilir? 
- **Faydalılık**: Desen neden başarılı olarak kabul edilir? 
- **Uygulanabilirlik**: Tasarım, daha geniş uygulanabilirliği nedeniyle bir desen olarak değerli midir? Öyleyse, bu açıklanmalıdır. Bir deseni gözden geçirirken veya tanımlarken yukarıdakileri akılda tutmak çok önemlidir.

## Özet
Bu bölüm, her önerilen proto-desenin her zaman bir desen olarak kabul edilmeyebileceğini gösterdi. Bir sonraki bölüm, desenleri yapılandırmak ve belgelemek için temel unsurları ve en iyi uygulamaları paylaşır, böylece topluluk onları kolayca anlayabilir ve kullanabilir.
