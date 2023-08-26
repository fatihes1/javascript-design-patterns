# Bölüm 4. Karşı Desenler
Mühendisler olarak, çözümü teslim etmek için bir süre sınırlamasıyla karşılaşabileceğimiz durumlar veya kodun bir dizi yamayla gözden geçirilmeden dahil edildiği durumlar gibi durumlarla karşılaşabiliriz. Bu tür durumlarda kod her zaman iyi düşünülmemiş olabilir ve Anti-Desenler olarak adlandırdığımız şeyleri yayabilir. Bu bölüm, Anti-Desenlerin ne olduğunu ve onları anlamak ve tanımlamak neden önemli olduğunu açıklar. Ayrıca JavaScript'teki bazı tipik karşı desenlere de bakar.

## Karşı Desenler Nedir? 
Bir desen en iyi uygulamayı temsil ederken, bir karşı desen önerilen bir desenin neden yanlış gittiğini gösteren bir dersi temsil eder. GoF'un Design Patterns kitabından ilham alan Andrew Koenig, 1995 yılında Journal of Object-Oriented Programming - Volume 8 dergisindeki makalesinde terimi "Anti-Desen" olarak ilk kez kullanmıştır. O, Anti-Desenleri şu şekilde tanımlamıştır:

>  [TR] Bir karşı desen, bir desen gibi, ancak bir çözüm yerine yüzeysel olarak çözüm gibi görünen ancak gerçekte çözüm olmayan bir şey sunar.
> 
> [EN] An antipattern is just like a pattern, except that instead of a solution, it gives something that looks superficially like a solution but isn’t one.

O, iki tür karşı desen kavramı sunmuştur. Karşı desenler: 
- Belirli bir soruna kötü bir çözümü ve olumsuz bir durumun ortaya çıkmasına neden olanı tanımlar 
- Söz konusu durumdan nasıl çıkılacağını ve iyi bir çözüme nasıl gidileceğini tanımlar.

Bu konuda Alexander, iyi bir tasarım yapısı ile iyi bir bağlam arasında iyi bir denge kurmanın zorluklarından bahseder: 

> Bu notlar tasarım süreci hakkında; işlevle yanıt olarak yeni bir fiziksel düzen, düzen, form sergileyen fiziksel şeyleri icat etme süreci... her tasarım problemi, sorunun tanımını yapan iki varlık arasında uygunluğu başarmak için bir çaba ile başlar: söz konusu form ve bağlamı. Form, sorunun çözümüdür; bağlam, sorunu tanımlar.

Karşı desenleri anlamak, tasarım desenlerinin farkında olmak kadar önemlidir. Bunu neden önemli bir özellik olarak kabul ediyoruz? Bir uygulama oluştururken, bir proje yaşam döngüsü inşa aşamasıyla başlar. Bu aşamada, uygun gördüğünüz gibi mevcut iyi tasarım desenlerini seçme eğilimindesinizdir. Ancak ilk sürümden sonra, bakım yapılması gerekecektir.

Zaten üretime alınmış bir uygulamanın bakımı özellikle zorlayıcı olabilir. Daha önce uygulama üzerinde çalışmamış olan geliştiriciler, projeye kötü bir tasarımı yanlışlıkla dahil edebilirler. Eğer söz konusu kötü uygulamalar zaten karşı desenler olarak tanımlanmışsa, geliştiriciler bunları önceden tanıyacak ve bilinen yaygın hatalardan kaçınacaklardır. Bu, tasarım desenlerinin bilgisinin, bilinen ve faydalı standart tekniklerin nerede uygulanabileceğini tanımamıza nasıl yardımcı olduğuyla benzerdir.

Çözümün kalitesi geliştikçe, takımın ne kadar becerikli olduğuna ve ne kadar süre yatırım yaptığına bağlı olarak iyi veya kötü olabilir. Burada iyi ve kötü, bağlama göre değerlendirilir - bir "mükemmel" tasarım, yanlış bağlamda uygulandığında bir karşı desen olarak kabul edilebilir. 

Özetlemek gerekirse, bir karşı desen, belgelenmeye değer kötü bir tasarımdır.

## JavaScript'teki Karşı Desenler 
Geliştiriciler bazen kod teslimlerini hızlandırmak için kısa yollar ve geçici çözümler tercih ederler. Bu tür çözümler genellikle kalıcı hale gelir ve karşı desenlerden oluşan temelde bir teknik borç birikir. JavaScript, zayıf tür belirlemeli veya tür belirlemesiz bir dil olduğundan, bazı kısa yolları almayı daha kolay hale getirir. İşte JavaScript'te karşılaşabileceğiniz bazı karşı desen örnekleri:

- Global ad alanını (context) çok sayıda değişkenle kirletmek, genel bağlamda değişkenler tanımlamak. 
- `setTimeout` veya `setInterval`'e işlevler/fonksiyonlar yerine dizgileri yani stringleri geçirmek, çünkü bu içsel olarak `eval()` kullanımını tetikler. 
- Object sınıfının prototipini değiştirmek (bu özellikle kötü bir karşı desendir). 
- Esnek olmadığı için JavaScript'i iç satır formunda kullanmak. 
- `document.write` kullanmak, `document.createElement` gibi yerel DOM alternatifleri daha uygundur. Yıllar boyunca `document.write` yanlış kullanılmıştır ve bir kaç dezavantaja sahiptir. Sayfa yüklendikten sonra çalıştırılırsa, üzerinde bulunduğumuz sayfayı üzerine yazabilir, bu da `document.createElement`'ı çok daha iyi bir seçenek yapar. Bu işlemin canlı bir örneği için [bu](https://jsfiddle.net/addyosmani/6T9vX/) siteyi ziyaret edebilirsiniz. Ayrıca, `XHTML` ile çalışmaz, bu da `document.createElement` gibi daha DOM dostu yöntemlere yönelmenin tercih edilmesinin başka bir nedenidir. 

Karşı desenlerin bilinmesi başarı için önemlidir. Bu tür karşı desenleri tanımayı öğrendikten sonra, kodumuzu yeniden düzenleyerek bunları etkisiz hale getirebiliriz, böylece çözümlerimizin genel kalitesi anında artar.

## Özet
Bu bölüm, Anti-Desenler olarak bilinen sorunlara yol açabilecek desenleri ve JavaScript Anti-Desenlerinin örneklerini ele aldı. JavaScript tasarım desenlerini detaylı bir şekilde ele almadan önce, desenler üzerine yapacağımız tartışma için ilgili olacak bazı önemli modern JavaScript kavramlarına değinmeliyiz. Bu, modern JavaScript özelliklerini ve sözdizimini tanıtan bir sonraki bölümün konusudur.




