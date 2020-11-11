# Practical Python Programming - Eğitmen Notları

Yazar: David Beazley

## Genel Bakış

Bu belge, hedefler, hedef kitle, zorlayıcı kısımlar vb. dahil olmak üzere
"Pratik Python" kursumun içeriğini öğretmek için bazı genel notlar ve
tavsiyeler sağlar.

Bu talimatlar, kursu tipik bir üç günlük kurumsal eğitim ortamında öğreten 
kişilere verilmiştir. Size kendi kursunuzu öğretme konusunda biraz fikir 
verebilirler.

## Hedef Kitle ve Genel Yaklaşım

Bu kurs, zaten programlama tecrübesi olan kişiler için bir "Python'a Giriş" 
kursu olmayı amaçlamaktadır. Bu kesinlikle insanlara "programlama 101" i öğretmek 
için tasarlanmış bir kurs değildir.

Bununla birlikte, bir Python kursundaki tipik bir öğrencinin aynı zamanda bir 
hard-core yazılım mühendisi veya programcısı olma ihtimalinin düşük olduğunu 
gözlemledim. Bunun yerine, muhtemelen mühendisler, bilim adamları, web programcıları 
ve daha deneyimsiz geliştiricilerden oluşan bir karışım elde edeceksiniz. Öğrenci 
geçmişi büyük ölçüde değişir. Çok fazla C, C++, Java deneyimi olan bazı öğrencileriniz 
olabilir, diğerleri PHP ve HTML'yi biliyor olabilir, diğerleri MATLAB gibi araçlardan 
geliyor olabilir ve diğerleri, ön koşulları netleştirmek için en iyi çabalarıma 
rağmen, neredeyse hiç geleneksel "programlama" deneyimine sahip olmayabilir.

Bunu akılda tutarak, ders Python'a genel veri işleme problemi (özellikle borsa verileri) 
üzerinden öğretmeyi amaçlamaktadır. Bu alan, basit olduğu ve geçmişine bakılmaksızın herkesin 
bilmesi gereken bir şey olduğu için seçilmiştir. Örnek olarak, zayıf programlama becerilerine 
sahip öğrenciler, spreadsheet (örneğin, Excel) kullanmak gibi yaygın şeyleri bilme eğilimindedir. 
Dolayısıyla, gerçekten sıkışmışlarsa, onlara "pekala, bu tuple listesi bir e-tablodaki veri 
satırlarına benzer" veya "bir liste anlama, bir spreadsheet sütununa işlem uygulamakla ve 
sonucu farklı bir sütuna koymakla aynı fikirdir" gibi şeyler söyleyebilirsiniz. 
Temel fikir, ezoterik "bilgisayar bilimi" problemlerine 
(ör. "Hadi gidip fibonacci sayılarını hesaplayalım") saplanmaktansa gerçek dünya ortamında 
sabit kalmaktır.

Bu sorun alanı aynı zamanda diğer programlama konularını tanıtmak için de işe yarar. 
Örneğin, bilim adamları / mühendisler veri analizi veya çizim hakkında bilgi edinmek isteyebilir. 
Böylece onlara matplotlib kullanarak nasıl plan yapılacağını gösterebilirsiniz. Web programcıları 
borsa verilerini bir web sayfasında nasıl sunacaklarını bilmek isteyebilirler. Yani, şablon
motorları hakkında konuşabilirsiniz. Sistem yöneticileri, günlük dosyalarıyla bir şeyler yapmak isteyebilir. 
Böylece, onları gerçek zaman akışlı stok verilerinin bulunduğu bir günlük dosyasına 
yönlendirebilirsiniz. Yazılım mühendisleri tasarım hakkında bilgi sahibi olmak isteyebilir. 
Böylece, stok verilerini bir nesnenin içinde kapsüllemenin veya bir programı genişletilebilir 
hale getirmenin yollarına bakmalarını sağlayabilirsiniz (örneğin, bu programın 10 farklı tablo 
formatında çıktı üretmesini nasıl sağlarsınız). Fikri anladın.

## Sunum Yönergeleri

Sunum slaytları (notlar) derse bir anlatı yapısı sağlamak ve öğrenciler alıştırmalar üzerinde 
çalışırken başvurmak için oradadır. Her slayttaki her madde işaretini zahmetle gözden 
geçirmeyin - öğrencilerin okuyabildiğini ve kodlama yaparken geri dönmek için zamanları 
olacağını varsayın. Slaytları oldukça hızlı bir şekilde geçme eğilimindeyim, ilerledikçe kısa 
örnekleri etkileşimli olarak gösteriyorum. Genellikle slaytları tamamen canlı demolar lehine 
atlarım. Örneğin, listeler üzerinde bir sürü slayt anlatmanız gerekmiyor. Sadece Yorumlayıcıya gidin 
ve bunun yerine bazı örnekleri canlı olarak listeleyin. Genel kural: Alışılmadık derecede 
aldatıcı bir şey olmadıkça slayt başına 1 dakikadan fazla zaman geçirilmez. Dürüst olmak 
gerekirse, muhtemelen slaytların çoğunu atlayabilir ve sizin için işe yaradığını 
düşünüyorsanız, canlı demoları kullanarak ders verebilirsiniz. Bunu sık sık yaparım.

## Kurs Egzersizleri

Kursta yaklaşık 130 uygulamalı alıştırma var. Her alıştırmayı yapar ve öğrencilere düşünmeleri 
ve kod yazmaları için zaman verirseniz, muhtemelen 10-12 saat sürer. Pratikte, muhtemelen 
öğrencilerin belirli alıştırmalarda daha fazla zamana ihtiyaç duyduklarını göreceksiniz. 
Aşağıda bununla ilgili bazı notlarım var.

Öğrencilere, çözüm kodunun mevcut olduğunu ve ona bakıp kopyalamanın uygun olduğunu, özellikle 
zaman gereksinimleri nedeniyle, tekrar tekrar vurgulamalısınız.

Kursu öğretmeden önce, sürpriz olmaması için her bir kurs alıştırmasından geçmenizi 
ve çalışmanızı şiddetle tavsiye ederim.

Kurs sırasında, öğrenciler de çalışırken genellikle her alıştırmayı çözüme 
bakmadan bilgisayarımda sıfırdan yapıyorum. Bunun için, alıştırmaların bilgisayar 
ekranına (yansıtılan) çekmeden bakabileceğiniz basılı bir kopyasını almanızı şiddetle 
tavsiye ederim. Alıştırma süresinin sonuna doğru, çözüm kodumu tartışmaya başlayacağım, ekranda 
farklı bitleri vurgulayacak ve bunlar hakkında konuşacağım. Çözümle ilgili herhangi bir 
potansiyel sorun varsa (tasarım konuları dahil), bunun hakkında da konuşacağım. 
Öğrencilere, ilerlemeden önce çözüm koduna bakmak / kopyalamak isteyebileceklerini vurgulayın.


## Bölüm 1: Giriş

Bu bölümün temel amacı, insanların çevre ile tanışmalarını sağlamaktır. 
Bu, etkileşimli kabuğu kullanmayı ve kısa programları düzenleme / çalıştırmayı içerir. 
Bölümün sonunda, öğrenciler veri dosyalarını okuyan ve küçük hesaplamalar yapan kısa 
komut dosyaları yazabilmelidir. Sayılar, dizeler, listeler ve dosyalar hakkında bilgi 
sahibi olacaklar. Ayrıca işlevler, istisnalar ve modüllerle ilgili bazı açıklamalar da 
olacaktır, ancak pek çok ayrıntı eksik olacaktır.

Bu kursun ilk bölümü genellikle en uzun olanıdır çünkü öğrenciler araçlar konusunda 
yenidirler ve işleri yürütürken çeşitli problemler yaşayabilirler. Odanın içinde 
dolaşmanız ve herkesin basit programları düzenleyebildiğinden, çalıştırabildiğinden ve hatalarını 
ayıklayabildiğinden emin olmanız kesinlikle çok önemlidir. Python'un doğru kurulduğundan emin olun. 
Kurs alıştırmalarını indirdiklerinden emin olun. İnternetin çalıştığından emin olun. 
Ortaya çıkan diğer her şeyi düzeltin.

Zamanlama: 1. bölümü ilk gün öğle yemeği civarında bitirmeyi hedefliyorum.

## Bölüm 2 : Verilerle Çalışma

Bu bölüm muhtemelen kurstaki en önemli bölümdür. Tupleler (demetler), listeler, dictionaryler (sözlükler) 
ve kümeler dahil olmak üzere veri temsili ve işlemenin temellerini kapsar.

Bölüm 2.2 en önemlisi. Öğrencilere, alıştırmaları mantık çerçevesinde çalıştırmaları 
için ihtiyaç duydukları kadar zaman verin. Dinleyicilere bağlı olarak egzersizler 
45 dakika sürebilir. Bu alıştırmanın ortasında, genellikle Bölüm 2.3'e (biçimlendirilmiş çıktı) 
geçeceğim ve öğrencilere çalışmaya devam etmeleri için daha fazla zaman vereceğim. 
Birlikte Bölüm 2.2 / 2.3 bir saat veya daha fazla sürebilir.

Bölüm 2.4'te insanlar enumerate() ve zip() kullanımlarını araştırıyor. 
Bu fonksiyonların gerekli olduğunu düşünüyorum, bu yüzden hemen geçmeyin.

Bölüm 2.5, koleksiyonlar modülünü tanıtmaktadır. Koleksiyonlar hakkında söylenebilecek 
çok şey var, ancak şu anda öğrenciler tarafından tam olarak takdir edilmeyecek. 
Buna daha çok "işte bu harika modül, daha sonra bakmanız gereken. İşte birkaç harika örnek."

Bölüm 2.6, liste verilerini işlemek için önemli bir özellik olan liste anlamalarını tanıtır. 
Öğrencilere, liste anlayışlarının SQL veritabanı sorguları gibi şeylere çok benzediğini 
vurgulayın. Bu alıştırmanın sonunda, genellikle daha gelişmiş bir şeyi içeren etkileşimli bir 
demo yaparım. Belki bir liste kavrama yapılabilir ve bazı veriler matplotlib ile çizilebilir. 
Ayrıca istekliyseniz Jupyter'i tanıtmak için bir fırsat.

Bölüm 2.7 en karmaşık alıştırmadır. Python'da birinci sınıf verilerin kullanımı ve listeler 
gibi veri yapılarının istediğiniz her türlü nesneyi tutabileceği gerçeğiyle ilgilidir. 
Alıştırmalar, CSV dosyalarındaki veri sütunlarının ayrıştırılmasıyla ilgilidir ve kavramlar 
daha sonra Bölüm 3.2'de yeniden kullanılmaktadır.

Zamanlama: İdeal olarak, ilk gün 2. bölümü bitirmek istersiniz. 
Bununla birlikte, bölüm 2.5 veya 2.6 ile bitirmek yaygındır. 
Öyleyse, biraz geride kaldığınızı düşünüyorsanız panik yapmayın.

## 3. Program Organizasyonu


Bu bölümün temel amacı, fonksiyonlar hakkında daha fazla ayrıntı sunmak ve öğrencileri 
bunları kullanmaya teşvik etmektir. Bölüm, fonksiyonlardan modüllere ve komut yazımına kadar 
inşa edilir.

Bölüm 3.1, basit "komut dosyasından" fonksiyonlara geçmekle ilgilidir. Öğrenciler 
düzensiz "script" yazmaktan caydırılmalıdır. Bunun yerine, kod en azından işlevlere 
göre modülerleştirilmelidir. Kodun anlaşılmasını kolaylaştırır, daha sonra değişiklik 
yapmayı kolaylaştırır ve aslında biraz daha hızlı çalışır. Fonksiyonlar iyidir.

Bölüm 3.2 muhtemelen tüm kurstaki en gelişmiş alıştırmalardır. Öğrencilerin sütun odaklı 
verileri ayrıştırmak için genel amaçlı bir yardımcı program fonksiyonu yazmasını sağlar. 
Bununla birlikte, liste anlamalarının yanı sıra fonksiyon listelerinden de yoğun bir şekilde 
yararlanır (örneğin, birinci sınıf nesneler olarak fonksiyonlar). Muhtemelen insanlara 
bu kodun her adımında rehberlik etmeniz ve nasıl çalıştığını ayrıntılı olarak göstermeniz 
gerekecektir. Ancak kazanç çok büyük, insanlara inanılmaz derecede güçlü bir şey yapan 
ve çok sayıda çok karmaşık kod olmadan C, C ++ veya Java'da yazmaları neredeyse imkansız 
olan kısa bir genel amaçlı fonksiyon gösterebilirsiniz. Bu kod için birçok olası 
tasarım / tartışma yolu vardır. Hayal gücünü kullan.

Bölüm 3.3, Bölüm 3.2'de oluşturulan işleve hata işlemeyi ekler. Bu, genel olarak istisna 
işleme hakkında konuşmak için iyi bir zamandır. Kesinlikle tüm istisnaları yakalamanın 
tehlikelerinden bahsedin. Bu, "Zen of Python" daki "Hatalar asla sessizce geçmemelidir" 
öğesinden bahsetmek için iyi bir zaman olabilir.

*Not: Alıştırma 3.4'ten önce, öğrencilerin report.py, pcost.py ve fileparse.py'nin tam olarak çalışan sürümlerini edindiğinden emin olun. Gerekirse Çözümler klasöründen kopyalayın *

Bölüm 3.4 Modül içe aktarımlarını (import) tanıtır. Bölüm 3.2-3.3'te yazılan dosya, Bölüm 3.1'deki 
kodu basitleştirmek için kullanılır. Öğrencilerin IDLE, sys.path ve içe aktarmayla ve
diğer çeşitli ayarlarla ilgili sorunları gidermelerine yardımcı olmanız gerekebileceğini unutmayın.

Bölüm 3.5 `__main__` ve komut dosyası yazma hakkındadır. Komut satırı argümanları hakkında da biraz
bahsediliyor. Argparse gibi bir modülü tartışmaya meyilli olabilirsiniz. 
Ancak, bunu yapmanın bir bataklığa yol açacağı konusunda dikkatli olun. 
Genellikle bundan bahsetmek ve devam etmek daha iyidir.

Bölüm 3.6, Python'da genel olarak tasarım hakkında bir tartışma açar. Yalnızca dosya 
adlarıyla çalışacak şekilde bağlanmış kod yerine daha esnek olan bir kod yazmak daha mı iyidir? 
Bu, bir kod değişikliği yaptığınız ve mevcut kodu yeniden düzenlemeniz gereken ilk yerdir.

Buradan itibaren, alıştırmaların çoğu önceden yazılmış kodda küçük değişiklikler yapar.

## 4. Sınıflar ve Nesneler

Bu bölüm çok temel nesne yönelimli programlama hakkındadır. Genel olarak, insanların OO 
konusunda çok fazla geçmişe sahip olduğunu varsaymak güvenli değildir. Bu yüzden, buna başlamadan 
önce, genellikle OO "stilini", verilerin ve yöntemlerin nasıl bir araya getirildiğini 
açıklarım. "Nesne" olduklarını ve yöntemlerin nesne ile bir şeyler 
yaptığını göstermek için dizeler ve listelerle bazı örnekler yapın. Yöntemlerin nesnenin 
kendisine nasıl eklendiğini vurgulayın. Örneğin, items.append(x) yaparsınız, ayrı bir 
fonksiyon append(items, x) çağırmazsınız.

Bölüm 4.1, sınıf ifadesini tanıtır ve insanlara temel bir nesnenin nasıl yapılacağını gösterir. 
Gerçekten, bu sadece sınıfları basit bir veri yapısını tanımlamanın başka bir yolu olarak 
tanıtır - bu amaç için bölüm 2'de tuple ve dicts kullanmaya geri dön.

Bölüm 4.2, Inheritance ve genişletilebilir programlar oluşturmak için nasıl kullandığınızla ilgilidir. 
Bu alıştırma seti, OO programlama ve OO tasarımı açısından muhtemelen en önemlisidir. 
Öğrencilere üzerinde çalışmaları için bolca zaman verin (30-45 dakika). İlgiye bağlı 
olarak, OO'nun yönlerini tartışmak için çok zaman harcayabilirsiniz. Örneğin, farklı tasarım 
modelleri, kalıtım hiyerarşileri, soyut temel sınıflar vb.

Bölüm 4.3, özel yöntemlerle birkaç deney yapar. Bununla oyalanarak fazla zaman harcamam. 
Özel yöntemler Egzersiz 6.1'de ve başka yerlerde ortaya çıkar.

Zamanlama: Bu genellikle 2. günün sonudur.

## 5. Nesnelerin İçi

Bu bölüm, öğrencileri nesne sisteminin perde arkasına götürür ve sözlükler 
kullanılarak nasıl oluşturulduğu, örneklerin ve sınıfların nasıl birbirine 
bağlandığı ve inheritance'ın nasıl çalıştığı hakkında bilgi verir. Bununla birlikte, bu 
bölümün en önemli kısmı muhtemelen kapsülleme ile ilgili materyaldir 
(özel özellikler, özellikler, yuvalar vb.).

Bölüm 5.1 sadece kapakları geri çeker ve öğrencilerin, örneklerin ve sınıfların 
altında yatan kuralları gözlemlemesini ve bunlarla oynamasını sağlar.

Bölüm 5.2, nitelikleri get / set fonksiyonlarının arkasına gizlemek ve özellikleri 
kullanmakla ilgilidir. Genelde bu tekniklerin kitaplıklarda ve çerçevelerde yaygın olarak 
kullanıldığını vurguluyorum - özellikle bir kullanıcının ne yapmasına izin verildiği 
üzerinde daha fazla kontrolün istendiği durumlarda.

Zeki bir Python ustası, tanımlayıcılar veya öznitelik erişim yöntemleri (`__getattr__`,`__setattr__`) 
gibi gelişmiş konulardan hiç bahsetmediğimi fark edecektir. Deneyim yoluyla, bunun giriş 
dersini alan öğrenciler için çok fazla zihinsel bir yük olduğunu keşfettim. Bu noktada 
herkesin kafası çoktan patlamanın eşiğinde ve eğer tanımlayıcılar gibi bir şeyin nasıl 
çalıştığı hakkında konuşursanız, kursun geri kalanı olmasa da günün geri kalanında onları 
kaybedersiniz. "Gelişmiş Python" kursu için kaydedin.

"Bu kursu bitirmemin imkanı yok" diye saate bakıyorsanız, 5. bölümü tamamen atlayabilirsiniz.

## 6. Generatorlar

Bu bölümün temel amacı, özel yinelemeyi tanımlamanın bir yolu olarak generatorları tanıtmak ve 
bunları veri işlemeyle ilgili çeşitli sorunlar için kullanmaktır. Kurs alıştırmaları, öğrencilerin 
akış verilerini bir günlük dosyasına yazılan stok güncellemeleri biçiminde analiz etmelerini sağlar.

Vurgulanması gereken iki büyük fikir var. İlk olarak, generatorlar artımlı işlemeye dayalı 
kod yazmak için kullanılabilir. Bu, veri akışı veya aynı anda belleğe sığmayacak kadar 
büyük olan devasa veri kümeleri gibi şeyler için çok yararlı olabilir. 
İkinci fikir ise, işleme boru hatları (bir tür Unix boruları gibi) oluşturmak için 
generatorları / yineleyicileri birbirine zincirleyebileceğinizdir. 
Yine, bu akışları, büyük veri kümelerini vb. İşlemenin ve düşünmenin gerçekten güçlü bir yolu 
olabilir.

Bazı ihmaller: Yineleme protokolü açıklanmış olsa da, notlar yinelenebilir nesneler 
(__iter__() ve next() içeren sınıflar) oluşturma konusunda ayrıntıya girmez. 
Pratikte, bunu çok sık yapmanın gerekli olmadığını fark ettim (generatorlar genellikle 
daha iyi / daha kolaydır). Bu yüzden, zaman uğruna bilinçli bir karar vererek 
onu atladım. Ayrıca, genişletilmiş generatorlar (korutinler) veya eşzamanlılık 
için oluşturucuların kullanımları (görevler, vb.) dahil değildir. Bu, ileri düzey 
kurslarda daha iyi işlenir.

## 7. İleri Konular

Temel olarak bu bölüm, daha önce ele alınmış olabilecek, ancak kurs akışı ve 
kurs alıştırmalarının içeriğiyle ilgili çeşitli nedenlerden ötürü olmayan daha 
ileri konuların bir derlemesidir. Bilmeniz gerekiyorsa, bu materyali kursun başlarında 
sunardım, ancak öğrencilerin zaten yeterli bilgi ile aşırı yüklenmiş olduğunu gördüm. 
Daha sonra geri dönersek daha iyi işliyor gibi görünüyor - özellikle bu noktada 
herkes Python'da çalışmaya ve alışmaya başladığı için.

Konular, çeşitli fonksiyon argümanlarını (* args, ** kwargs), lambda, kapatmalar ve 
dekoratörleri içerir. Dekoratörlerin tartışılması, meta-programlamayla nelerin mümkün 
olduğuna dair yalnızca küçük bir ipucudur. Neyin mümkün olduğu hakkında daha fazlasını 
söylemekten çekinmeyin, ancak muhtemelen metasınıflardan uzak dururum! Son zamanlarda, daha 
ilginç bir dekoratör örneği olarak "numba" yı gösteriyorum.

Eğer zamanınız varsa, 7. bölümün çoğu atlanabilir veya aşırı derecede 
sıkıştırılabilir (örneğin alıştırmaları atlayabilirsiniz).

## 8. Test ve Hata Ayıklama

Bu bölümün temel amacı test etme, hata ayıklama ve yazılım geliştirme 
ile ilgili çeşitli araçları ve teknikleri tanıtmaktır. Herkese birim test modülünü 
gösterin. Logging modülünü tanıtın. İddiaları ve "sözleşmeler" fikrini tartışın. İnsanlara 
hata ayıklayıcıyı ve profil oluşturucuyu gösterin. Bunların çoğu kendinden açıklamalıdır.

## 9. Package

Bu noktada, öğrenciler bir dizi dosya (pcost.py, report.py, fileparse.py, tableformat.py, stock.py, portfolyo.py, follow.py, vb.) 
yazmışlardır. Bu bölümde iki ana hedef var. İlk olarak, tüm kodu bir Python paket yapısına 
yerleştirin. Bu, buna sadece nazik bir giriş, ancak dosyaları bir dizine taşıyacaklar 
ve her şey bozulacak. Import ifadelerini (pakete göre içe aktarmalar) düzeltmeleri 
ve belki bir __init__.py dosyasıyla uğraşmaları gerekecek. İkinci hedef, kodu paketlemek 
ve birisine vermek için kullanabilecekleri basit bir setup.py dosyası yazmak. 
Bu kadar. Kursun sonu.

[Contents](Contents.md)

