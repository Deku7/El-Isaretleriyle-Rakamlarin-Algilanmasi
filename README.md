# Projede Kullanılan Yazılım/Donanım Araçları
Projede nesneleri, yüzleri algılama ve tanımlama, insan eylemlerini sınıflandırma, hareket eden nesneleri takip etme ve düşük çözünürlükteki görüntüleri birleştirerek yüksek çözünürlük elde etmek gibi birçok alanda kullanılan, gerçek zamanlı bilgisayar görmeyi hedefleyen, **Opencv** kütüphanesinden yararlanıldı. Opencv Python, Java ve C++ arayüzlerine sahip olmakla birlikte Windows, Linux, Mac OS, IOS, ve Android’i destekler. Kütüphaneyi daha iyi anlamak adına mimarisinden ve kütüphaneyi oluşturan bileşenlerden bahsetmek gerekirse. **Core**, opencv’nin matris ve temel fonksiyonları, size gibi veri yapılarını bulundurur. Bunlara ek olarak görüntü üzerinde çizim yapabilmeyi sağlayan metotları ve XML için gerekli bileşenlere sahiptir. **HighGui**, pencereleri yönetme, resim görüntüleme ve videoların kaydedilip silindiği fonksiyonları içerir. **Imgproc**, kenar bulma, nesne belirleme, filtreleme ve geometri gibi görüntü işleme ile ilgili fonksiyonları barındırır. Renk uzayı dönüşümleri bu kısımda yer alır. **Imgcodecs**, resim ve videoların okuma/yazma işlemlerini dosya sistemi üzerinden gerçekleştiren metotları barındırır. **Videoio**, bilgisayara bağlı olan veya harici olarak takılan kameralara erişmede ve bu kameralardan görüntüleri almakta kullanılan metotlara sahiptir. Bahsi geçen bu bileşenlerle opencv kütüphanesi içerisinde görüntü işlemede ve makine öğrenmesinde kullanılabilecek 2500’den fazla algoritma bulunmaktadır.

Diğer bir kütüphane ise bilimsel hesaplamaların hızlı bir şekilde yapılmasını sağlayan matematiksel işlemlerin ağırlıklı olduğu **NumPy** kütüphanesidir. Bu kütüphanenin temelinde numpy dizileri bulunmakta, bu diziler python listelerine benzemesine karşın ek olarak hız ve işlevsellik açısından daha kullanışlıdır. Ayrıca python listelerinden bir diğer farkı ise homojen olması yani dizi içerisinde ki tüm elemanların aynı veri tipinde olmasıdır.

Python dili derlenmeye ihtiyaç duymadığından oldukça hızlı bir şekilde proje geliştirmeye ve uygulamaya imkan sunar. Basit ve sade yapısıyla konuşma diline yakın bir dil olduğundan kullanımı oldukça kolaydır. Nesne yönelimli olmasının yanı sıra prosedür yönelimli programları da destekler. Üst seviye bir dil olduğundan proje içerisinde ki kritik bir kod parçasının çok hızlı çalışması gerektiğinde sorunla karşılaşılmaması adına, o kod parçası c dilinde yazılıp python ile birleştirilebilir. İçerisinde birim testi, dokümantasyon oluşturma, iş parçacığı, veritabanları, XML, HTML, şifreleme, GUI vb. birçok kütüphane bulundurduğundan kullanım olanı oldukça geniştir. 


# Gelişme (Proje Ve Uygulamalar)
Projede 1’den 10’a kadar olan el işaretlerini kameradan algılayıp, bu işaretleri daha önceden uygulamaya eklenmiş el işaretleri ile karşılaştırarak hangi sayıya denk geliyor ise o sayıyı ekrana yazdırmak amaçlanmıştır. Geliştirme ortamı olarak Python için oluşturulmuş olan Pycharm IDE’si kullanılmaya karar verildi. Görüntü işleme denince akla ilk gelen kütüphanelerden biri olan opencv kütüphanesi araştırıldı ve projede nasıl kullanılacağına dair bilgiler edinildi. Python’un diğer temel kütüphanelerinden olan numpy ve os hakkında da projeye yönelik bilgiler edinildikten sonra proje uygulanmaya başlandı. Proje için gerekli olan görüntüleri laptop üzerinde yerleşik olarak bulunan kamera sayesinde elde etmek planlandı. Bu kameraya opencv kütüphanesinden **VideoCapture** metodu kullanılarak Proje içerisinde erişim sağlandı. Tüm görüntü üzerinden değil de kameradaki görüntünün sol üst köşesi üzerinde çalışılmak üzere kesildi. Kesilen karedeki görüntüde, el işaretlerini algılamak için HSV renk uzayından faydalandı. HSV de doygunluk ve parlaklık gibi değerler kullanıldığından eşik değerinin düzgün ayarlanması kaydı ile ten rengini ayırt etmede başarılı olacağı düşünüldü. Çeşitli kaynaklardan ve deneme yanılma yolu ile ten renginin algılanması için en verimli alt ve üst sınır değerleri belirlendi. Ortaya çıkan renk filtresi denendiğinde ten rengini ayırt edebildiğini ancak avuç içlerinde ve parmak kısmında boşluklar kaldığı gözlemlendi, bu boşluklar el işaretlerinin algılanmasında sorunlar oluşturacağı için opencv dokümanlarından faydalanılarak boşlukları doldurmak üzere **Closing morpholohyEx** metodu kullanıldı. Bu sayede boşluklar giderilerek tam bir el formu yakalanmış oldu. En iyi sonucu elde etmek ve parazitleri gidermek için renk filtresi üzerinde son olarak **Dilate** operatörü kullanıldı. Dilatein kullanım amacı görüntü üzerinde yayma-genişletme işlemi uygulayarak, görüntünün daha kararlı bir hale getirilmesidir. Renk filtresi bu işlemlerden sonra kullanılabilir hale gelmiş oldu ancak tüm bu işlemler bir kere değil kamera açık olduğu müddet anlık olarak gerçekleştirileceği için while döngüsü içerisine sokularak sürekli bir hale getirildi. While döngüsü sonsuz olduğu zaman kodlamada hata vereceğinden while döngüsü içerisine klavyeden ‘q’ tuşuna basıldığında döngüden çıkılması için break komutu kullanıldı. Döngüden çıkıldıktan sonra kamera kullanımı bittiği için **release** komutu ile kamera serbest bırakıldı ve opencv pencereleri **destroyWindows** ile kapatıldı. El işaretlerinin düzgün bir şekilde karşılaştırılabilmesi ve arka planda oluşabilecek parazitlerin önüne geçilebilmesi için, görüntü içerisindeki eli algılayıp sadece elin göz önüne alınarak işlem yapılması amaçlanmıştır. Bu doğrultuda opencv kütüphanesinden **Contours metodu** kullanıldı Contours metodu basitçe aynı yoğunluğa ve renklere sahip olan tüm noktaları, eğri biçiminde bir sınır boyunca birleştiren bir metottur. Elin algılanması ve bu görüntülerin karşılaştırılmasında kolaylık sağlar. Proje boyunca siyah beyaz görüntüler üzerinden çalışıldığı için contours metodu daha doğru sonuçlar vermiştir. Contourların bulunması için kullanılan **cv2.findContours()’de** 3 farklı argüman bulunmaktadır. İlk argüman projemizin kaynak görüntüsü olan renk filtresidir. İkinci argüman contour alma metodu olan **Retr_Tree** ve son argüman ise contour yaklaşım metodudur. Yaklaşım metodunda tüm contour bilgilerini saklamak için **Chain_Approx_None** komutu kullanılmıştır. Saklanan tüm koordinatlar for döngüsü içerisinde döndürülerek Max Genişlik ve Max Uzunluk belirlendi. Belirlenen noktalardan görüntü kesilip El resmi ortaya çıkarıldı. Projenin bu kısma kadar olan kodları kopyalanarak kaydetme işlemi için ayrı bir sınıf içerisine kopyalandı. Bu sınıf içerisinde Proje içinde oluşturulan veri klasörüne, karşılaştırılmak üzere kullanılacak jpg formatındaki resimler yüklendi. Yüklenen resimler daha önceki renk filtresi kullanılarak oluşturulduğundan karşılaştırma sırasında tutarlı verilerin elde edilmesi planlandı. Veri klasörü içerisine atılan resimlerin adları, resimlerle ilişkili olacak şekilde kaydedildi. (Resimde 1 işareti varsa resim adı bir) Kayıt işlemleri yapıldıktan sonra ana kodlamanın yapıldığı sınıfa dönerek karşılaştırılmanın yapılması için ResimFarkBul adlı fonksiyon oluşturuldu. Fonksiyona gönderilen iki resim resize komutu ile aynı boyutlarda olacak şekilde yapılandırıldı. Aynı boyutlara getirilen resimlerde, **absdiff** komutu kullanılarak resimler arasındaki farklar bulundu. Absdiff komutu bir resmi diğer resimden çıkardığı için fark ne kadar az olursa eşleşme oranı o kadar fazla demektir. Absdiff sonuç olarak bir başka görüntü döndürdüğünden sayısal bir sonuç elde etmek için bu görüntüdeki birler **countNonZero** komutu ile hesaplandı. Daha sonra sınıflandırmaların yapılması için el resmi, veri klasöründe bulunan resimler ve bu resimlerin isimlerinin gönderildiği Sınıflandır adlı bir fonksiyon tanımlandı. Bu fonksiyon içerisinde ResimFarkBul fonksiyonu da kullanılarak gönderilen resim, veri klasöründeki tüm resimlerle karşılaştırılıp aralarından en az fark olan resim belirlendi. Karşılaştırılmada en az fark çıkan resmin ismi konsolda görünecek şekilde yazdırıldı. 

<p align="center">
  <b>Şekil 1. Kesilmiş Kare</b><br>
  <img src="Resimler/KesilmişKare.png">
</p>

-	Şekil 1 de görüldüğü üzere, kamera açısının çok geniş olmasına rağmen daha rahat bir çalışma ortamı oluşturmak için görüntünün büyük bir bölümü kesilerek sadece ekranın sağ üst kısmını alacak şekilde ayarlandı.
-	Çalışılacak alanın küçültülmesi bize arka planda oluşabilecek parazitlerden kurtulma şansı verdi ve sadece el işaretlerinin algılanmasında yardımcı oldu. 
-	Kesilmiş kare kameranın 0:250, 0:250 koordinatlarıdır. Daha sonra bu kesilmiş kareden faydalanılarak aynı kare HSV renk uzayına dönüştürülecek. 
 
 <p align="center">
  <b>Şekil 2. Elin Algılanıp Kesilmesi</b><br>
  <img src="Resimler/ElinAlgılanıpKesilmesi.png">
</p>

Şekil 2’de el işaretlerinin boyutu kameraya uzaklıkla, işaretle ve kullanan kişiye bağlı olarak değişebileceğinden, kesilmiş ekranda ki tüm görüntüyü değil de el işaretini algılayarak sadece el işaretinin kesilip alınması sağlandı. Elin algılanmasında kullanılan findContours metodu sayesinde belirlenen max uzunluk ve max genişliklere göre kesme işlemi yapıldı. findContours metoduna gönderilen 3 farklı parametre vardır. Bu parametrelerden ilki hsv renk uzayına dönüştürüldükten sonra çeşitli düzeltmelerle son halini alan renk filtresi sonucu, ikincisi contour alma metodu olarak kullanılan retr_tree ve sonuncu parametre ise ortaya çıkacak şeklin koordinat bilgilerini saklamada kullanılan chain_approx_none parametresidir. Parametreler düzgün bir şekilde girildikten sonra contours içerisine kaydedilen x, y koordinatları ve genişlik, uzunluk bilgileri boundingRect metodu sayesinde döngüye sokularak tek tek kontrol edilir. Döngü sonucunda kesilecek görüntünün sınırlarını belirleyen maximum uzunluk ve maximum genişlik bilgileri kaydedilir. Kaydedilen bilgiler daha sonra rectangle metoduyla kesilen kare üzerinde yeniden çizdirilir. El işaretinin kesilip alınması ise aynı verilerin renk filtresi sonucu üzerinde uygulanmasıdır. 

 <p align="center">
  <b>Şekil 3. Renk Filtresinin Uygulanması</b><br>
  <img src="Resimler/RenkFiltresi.png">
</p>

 <p align="center">
  <b>Şekil 4. Closing Morphology Örneği</b><br>
  <img src="Resimler/ClosingMorphologyÖrneği.png">
</p>

 <p align="center">
  <b>Şekil 5. Dilation Morphology Örneği</b><br>
  <img src="Resimler/DilationMorphologyÖrneği.png">
</p>
 
HSV renk uzayı kullanılarak elde edilen şekil 3’de ten rengini alabileceği alt ve üst değerler belirlendi. Daha sonrasında avuç içlerinde ve parmak kısımlarından oluşan boşluklar opencv kütüphanesindeki mophologyEx metodu sayesinde giderildi. Yine open cv kütüphanesinde bulunan dilate metodu ile görüntü daha yumuşak ve net bir hale getirildi. HSV renk uzayının alt sınırı 0,30,60 olarak üst sınırı ise 40,255,255 olarak ayarlandı bu değerler ten rengini algılamada kullanılacak sınırlar olup deneme yanılma yolu ile optimum sonucun elde edilmesi amaçlandı. İnsandan insana ten rengi değişebileceği için aralık geniş tutulmak istense de bu sefer ortaya ten olmasa da ten rengine yakın objelerin algılanması problemi ortaya çıkacaktır. Bu sebeple alt ve üst sınırların belirlenmesi çok önemlidir. Şekil 4’de örneği verilen closing morphologi metodu, HSV renk filtresi oluştururken ortaya çıkan boşlukları kapatmada kullanılan bir uygulamadır. Avuç içlerinde ve parmaklarda ortaya çıkan parazitleri gidermekte kullanılmıştır. Örneği Şekil 5’de verilen dilation metodu ise el işaretini kalınlaştırmada kullanılmıştır.

 <p align="center">
  <b>Şekil 6. Karşılaştırma Verileri</b><br>
  <img src="Resimler/KarşılaştırmaVerileri.png">
</p>
 
-	El işaretlerini karşılaştırabilmek için gerekli olan veriler Şekil 6’de gözükmektedir.
-	JPG formatındaki bu verilerin elde edilmesinde aynı bir kaydetme sınıfı kullanıldı. Bu sınıf içerisindeki renk filtresi aynı şekilde yazılarak karşılaştırılacak görüntülerin aynı formatta olması sağlandı.
-	 Görüntüleri kayıt etmek için görüntüye verilecek isim proje çalıştırılmadan önce elle girilir ardından verilen ada uygun el işareti yapılarak klavyeden ‘q’ tuşuna basılır. Tuşa bastıktan sonra program çalışmayı durdurarak ekrandaki görüntüyü daha önceden girilen isimle birlikte ana proje içerisindeki veriler klasörüne kaydeder.
-	 Şekil 7’de ki örnekte verilerin nasıl olduğu görünmektedir. 

  <p align="center">
  <b>Şekil 7. Uc Numaralı Veri Örneği</b><br>
  <img src="Resimler/ÜçNumaralıVeriÖrneği.png">
</p>

  <p align="center">
  <b>Şekil 8. El İşaretinin Algılanıp Yazdırılması</b><br>
  <img src="Resimler/El_İşaretinin_Yazılması.png">
</p>

Şekil 8’de ki görüntü renk filtresi uygulanarak elde edilen el işaretinin, verilerde bulunan görüntülerle karşılaştırılması sonrasında eşleşmenin en yüksek olduğu veriye ait ismin ekrana yazılmasıdır. Görüntülerin karşılaştırılmasında 3 farklı fonksiyon kullanılmıştır:
-	Veri Yükleme Fonksiyonu, proje içerisindeki verilerin hepsini karşılaştırılabilmesi için kullanılmıştır. Verilerin isimleri ve görüntüleri liste içerisine append metoduyla kaydedilip fonksiyonun return değerleri olarak Veri İsimler ve Veri Resimler değerleri döndürülmüştür.
-	Resim Fark Bul Fonksiyonu, fonksiyona gönderilen iki resim arasındaki farklı sayısal olarak bulmaya yarar. İlk olarak iki resimde aynı boyutlara getirilir. Boyutlandırma resize metoduyla yapıldıktan sonra iki resim absdiff metoduna sokularak resim2 resim1 den çıkarılır bu sayede aralarındaki fark başka bir görüntü haline getirilip kaydedilmiş olur. Farkın kaydedildiği bu görüntü üzerinde countnonzero metodu kullanılarak farkın sayısal değeri elde edilir. Bu değer ne kadar küçük olursa benzerlik o kadar çok demektir.
-	Sınıflandır Fonksiyonu, bu fonksiyona 3 farklı veri gönderilir bu veriler kamerada gözüken resim, karşılaştırılmak üzere daha önceden kaydedilen resimler ve bu resimlerin isimleri.  Fonksiyonun amacı diğer fonksiyonlardan da faydalanarak resimlerin karşılaştırılması ve sınıflandırılmasıdır. Kamerada renk filtresine sokulmuş görüntü ile veriler içerisinde bulunan tüm resimler sırası ile resim fark bul fonksiyonuna gönderilir. Dönen sonuç kaydedilir ve sonraki resim karşılaştırıldığında daha küçük bir sonuç elde edilmiş ise o resmin ismi kaydedilir. Tüm karşılaştırılmalar bittikten sonra kayıtlı olan isim ekrana yazdırılmakta kullanılmak üzere Veri isim değeri return edilir.

<p align="center">
<b>Şekil 9. El İşaretinin Algılanıp Yazdırılması</b><br>
<img src="Resimler/El_İşaretinin_Yazdırılması_2.png">
</p>

Şekil 8 ve Şekil 9, sınıflandırmanın yapıldığı ve sonucun ekrana yazıldığı örnekler, karşılaştırma ve sınıflandırma sonuçları doğru çalışmış ve ekrana el işaretine denk gelen sayı yazdırılmıştır.

<p align="center">
<b>Şekil 10. Kaydetme Sınıfı Örneği</b><br>
<img src="Resimler/KaydetmeSınıfıÖrneği.png">
</p>
 
<p align="center">
<b>Şekil 11. Kayıt Edilmiş İşaret</b><br>
<img src="Resimler/KayıtEdilmişİşaret.png">
</p>

Verilerin kayıt edilmesinde kullanılan kayıt etme sınıfı çalıştırıldığında şekil 10’daki görüntü elde edilmektedir. Görüntünün elde edilmesinde el işaretlerinin algılandığı sınıf ile aynı renk filtresi kullanıldı. Şekil 11 ise görüntünün kesilerek kayıt edilmeden önceki son halini göstermektedir. Kesilen görüntü sadece el işaretini algılayarak doğru verinin kayıt edilmesini sağlar.
