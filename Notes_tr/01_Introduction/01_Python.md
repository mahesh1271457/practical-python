[Contents](../Contents.md) \| [Next (1.2 A First Program)](02_Hello_world.md)

# 1.1 Python

### Python Nedir?

Python yorumlanmış yüksek seviye bir dildir.  Genellikle bir
["scripting dili"](https://en.wikipedia.org/wiki/Scripting_language) olarak sınıflandırılır ve Perl, Tcl veya Ruby dillerine benzediği kabul edilir. Python dilinin sözdizimi genel olarak C programlamanın öğelerinden esinlenmiştir.

Python 1990larda, ismini Monty Python’ın şerefine koyan Guido Van Rossum tarafından yaratılmıştır.
 
### Pyhton’ı Nasıl Edinebilirim?

[Python.org](https://www.python.org/) sitesi Python’ı elde edebileceğiniz yerdir.  Bu kurs için yalnızca temel yükleme yapmaya ihtiyacınız olacaktır. Ben Python 3.6 veya daha üst sürümü yüklemenizi öneririm.
Python 3.6 notlarda ve çözümlerde kullanılır.

### Python Neden Yaratıldı?

Python’ın yaratıcısının sözleriyle:

> Python’ı oluşturmak için asıl motivasyonum Amoeba [İşletim Sistemleri]
> projesi için duyulan yüksek seviyeli dil ihtiyacıydı. Sistem yöneticisi
> araçlarını C dilinde geliştirmenin çok uzun sürdüğünü fark ettim. Üstelik 
> bunları Bourne kabuğunda yapmak çeşitli nedenlerden dolayı işe yaramazdı.
> Yani, C ile kabuk arasındaki boşluğa köprü olacak bir dile ihtiyaç vardı.
>
> - Guido van Rossum

### Python Makinemin Neresinde?

Python çalıştırabileceğiniz birçok ortam olsa da, Python genellikle terminal veya komut satırından çalıştırılabilen bir program olarak kurulur. Terminalden bu şekilde `python` yazabiliyor olmalısınız:

```
bash $ python
Python 3.8.1 (default, Feb 20 2020, 09:29:22)
[Clang 10.0.0 (clang-1000.10.44.4)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> print("hello world")
hello world
>>>
```

Eğer shell veya terminal kullanmak konusunda yeniyseniz, muhtemelen burada durup bu konu için ufak bir eğitim bitirip tekrar buraya gelmelisiniz.

Python yazabileceğiniz birçok shell dışı ortam olmasına rağmen eğer terminalde Python çalıştırır, hata ayıklar ve etkileşime geçebilirseniz çok güçlü bir Python programcısı olursunuz. Bu Python’ın ana ortamıdır. Eğer Python’ı burada kullanabilirseniz her yerde kullanabilirsiniz.

## Egzersizler

### Egzersiz 1.1: Pyhton’ı Hesap Makinesi Olarak Kullanmak

Makinenizde Python’ı başlatın ve aşağıdaki problemi çözebilmek için hesap makinesi olarak kullanın.

Şanslı Larry fiyatı $235.14’ten 75 tane Google hissesi satın aldı. Bugün Google’ın hisseleri $711.25 oldu. Python’ın interaktif modunu hesap makinesi olarak kullanarak Larry’nin tüm hisselerini satması durumunda ne kadar kar edeceğini bulun.

```python
>>> (711.25 - 235.14) * 75
35708.25
>>>
```

Öneri: Önceki hesabın sonucunu kullanabilmek için alt çizgi (\_) değişkenini kullanın. Örneğin Larry, onun şeytani Broker’ı %20 kesinti yaptıktan sonra ne kadar kâr elde eder? 

```python
>>> _ * 0.80
28566.600000000002
>>>
```

### Egzersiz 1.2: Yardım almak

`help()` komutunu kullanarak `abs()` fonksiyonu için yardım alın. Sonrasında `help()` komutuyla  `round()` fonksiyonu için yardım alın. Yalnızca `help()` komutunu yazarak interaktif yardım görüntüleyiciye erişebilirsiniz.

`help()` komutu `for`, `if`, `while` gibi temel Python ifadeleri için çalışmamaktadır. (örneğin `help(for)` yazarsanız sözdizimi hatası alırsınız.) Yardım almak istediğiniz ifadeyi tırnak işaretleri arasına almayı deneyebilirsiniz. Örneğin `help("for")` şeklinde. Eğer bu da işe yaramazsa internetten arayabilirsiniz.

Tamamlayıcı: <http://docs.python.org> adresine gidin ve `abs()` fonksiyonu için dökümantasyonu bulun. (ipucu: Yerleşik fonksiyonlarla ilgili kütüphane referanslarının altında bulunur.)

### Egzersiz 1.3: Kesmek ve Yapıştırmak

Bu kurs sizi, **kendinizin yazacağı** interaktif Python kod örneklerini denemenizi teşvik edecek bir dizi geleneksel web sayfası olarak yapılandırılmıştır. Eğer Python’ı ilk kez öğreniyorsanız, bu "yavaş yaklaşım" teşvik edilir.  Yavaşlayarak, bir şeyleri yazarak ve ne yaptığını düşünerek dil üzerindeki hakimiyetinizi arttıracaksınız. 

Eğer kod örneklerini "kes ve yapıştır" yapmanız gerekiyorsa, 
`>>>` komutundan sonra başlayan kodu seçin ancak ilk boşluk satırından veya sonraki `>>>` komutundan daha uzağa gitmeyin (ilk görünende). Tarayıcıdan "kopyala" yı seçin, Python ekranına gidin, ve "yapıştır" seçeneğini seçerek Python shell ekranına kopyalayın. Kodu çalıştırmak için, yapıştırdıktan sonra “Return”’e tekrar basabilirsiniz.

Bu oturumda Python komutlarını kullanabilmek için kes-yapıştır kullanın:

```python
>>> 12 + 20
32
>>> (3 + 4
         + 5 + 6)
18
>>> for i in range(5):
        print(i)

0
1
2
3
4
>>>
```

Uyarı: Python shell’de tek seferde birden fazla Python komutu yapıştırmak asla mümkün değildir. (`>>>` dan sonra gözüken komutlar) Her komutu tek tek yapıştırmalısınız.

Bunu yaptığınıza göre, kodu --kes yapıştır yaparak değil de yavaşça ve üzerinde düşünerek yazarak daha fazlasını elde edeceğinizi unutmayın.

### Egzersiz 1.4: Otobüsüm Nerede?

Daha ileri seviye bir şey deneyin ve Chicago’daki Clark Sokağı ile Balmoral köşesinde insanların bir sonraki kuzeye giden CTA /#22 otobüsünü ne kadar beklediklerini bu komutları kullanarak bulun:

```python
>>> import urllib.request
>>> u = urllib.request.urlopen('http://ctabustracker.com/bustime/map/getStopPredictions.jsp?stop=14791&route=22')
>>> from xml.etree.ElementTree import parse
>>> doc = parse(u)
>>> for pt in doc.findall('.//pt'):
        print(pt.text)

6 MIN
18 MIN
28 MIN
>>>
```

Evet, bir web sayfası indirdiniz, bir XML dökümanı olarak ayrıştırdınız ve
6 satırlık bir kodda işe yarar bir bilgiyi çıkardınız. Eriştiğiniz veri
<http://ctabustracker.com/bustime/home.jsp> web sitesini besliyor. Bir daha deneyin ve tahminlerin değiştiğini görün.

Not: Bu servis sadece 30 dakikalık ulaşım saatlerini gösteriyor. Eğer farklı zaman dilimindeyseniz ve Chicago’da saat sabah 3 ise, farklı bir çıktı elde edemeyebilirsiniz.
  Yukarıdaki tracker linkini kullanarak tekrar kontrol edin.
 
Eğer ilk import komutu olan `import urllib.request` başarısız olursa, muhtemelen Python 2 kullanıyorsunuzdur. 
 Bu kurs için Python 3.6 veya daha ileri sürümlerini kullandığınızdan emin olmalısınız. Eğer ihtiyacınız varsa <https://www.python.org> sitesine gidip indirebilirsiniz.

Eğer çalışma ortamınız HTTP proxy server kullanımını zorunlu kılıyorsa egzersizin bu kısmını çalıştırabilmek için `HTTP_PROXY` ortam değişkenini atamanız gerekebilir. Örneğin:

```python
>>> import os
>>> os.environ['HTTP_PROXY'] = 'http://yourproxy.server.com'
>>>
```

Eğer bunu çalıştıramadıysanız endişelenmeyin.  Kursun geri kalanının XML ayrıştırmayla alakası yok.

[Contents](../Contents.md) \| [Next (1.2 A First Program)](02_Hello_world.md)



