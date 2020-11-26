[Contents](../Contents.md) \| [Previous (9.1 Packages)](01_Packages.md) \| [Next (9.3 Distribution)](03_Distribution.md)

# 9.2 Üçüncü Taraf Modülleri

Python gömülü modüller (*piller dahil*) için geniş bir kütüphaneye sahip.
Birçok üçüncü taraf modülü var. Bunlara ulaşmak için [Python Package Index](https://pypi.org/) veya PyPi sayfalarını ziyaret edebilirsiniz. Veya spesifik bir konu için basit bir Google araması da yapabilirsiniz.

Üçüncü taraf bağımlılıklarını yönetmek Python için sürekli gelişen bir konu. Bu bölüm beyninizi nasıl çalıştığına bağlamanıza yardımcı olacak temel bilgileri kapsamakta.


### Modül Arama Yolu

`sys.path`,  `import` komutu tarafından içe aktarılıp doğrulanmış bütün dizinleri içeren dizindir.  


```python
>>> import sys
>>> sys.path
... look at the result ...
>>>
```

Eğer herhangi bir şeyi import ederseniz ve import ettiğiniz şey bu dizinlerden birinde değilse `ImportError` re hatası alacaksınızdır.


### Standart Kütüphane Modülleri

Python’ın standart kütüphanelerinden alınan modüller genellikle `/usr/local/lib/python3.6' gibi konumlardan gelir. Kendiniz de kısa bir testle bunu gözlemleyebilirsiniz.


```python
>>> import re
>>> re
<module 're' from '/usr/local/lib/python3.6/re.py'>
>>>
```

Sadece REPL'deki bir modüle bakmak, hakkında bilgi sahibi olmak için iyi bir hata ayıklama yoludur.


### Üçüncü Taraf Modülleri 

Üçüncü taraf modülleri genellikle özel bir `site-packages` dizininde bulunur. Eğer aşağıdaki adımları takip ederseniz siz de göreceksiniz.

```python
>>> import numpy
>>> numpy
<module 'numpy' from '/usr/local/lib/python3.6/site-packages/numpy/__init__.py'>
>>>
```

Tekrardan,  `import` işlemiyle ilgili bir şeyin neden beklendiği gibi çalışmadığını anlamaya çalışıyorsanız, bir modüle bakmak iyi bir hata ayıklama yolu olabilir. 



### Modülleri Yüklemek

Üçüncü taraf modüllerini yüklemenin en yaygın yolu `pip` kullanmaktır. Örneğin:

```bash
bash % python3 -m pip install packagename
```

Bu komut paketi indirecek ve `site-packages` dizinine yükleyecektir.


### Problemler

* Python’ın direkt olarak kontrol edemeyeceğin bir sürümünü kullanıyor olabilirsin.
  *Kurumsal onaylı bir kurulum
  *İşletim sistemi ile birlikte gelen Python sürümünü kullanıyorsunuz. 
* Bilgisayarınıza global paketleri yüklemeye izniniz olmayabilir.
* Bu sorunlardan başka sorunlar da olabilir.

### Sanal Ortamlar


Paket yüklemelerinde yaşanan probleri çözmenin en yaygın yolu “sanal ortam” oluşturmaktır. Doğal olarak bunu yapmak için sadece tek bir yol yok, hatta birbirine rakip olan birkaç araç ve teknik var. Bununla beraber, standart bir Python kurulumu kullanıyorsanız şunu yazmayı deneyebilirsiniz:

```bash
bash % python -m venv mypython
bash %
```

Birkaç dakika bekledikten sonra, kendi küçük Python kurulumunuz olan yeni bir `mypython` dizinine sahip olacaksınız. Bu dizinin içinde `bin/` (Unix) dizinini veya `Scripts/` (Windows) dizinini bulacaksınız. Orada bulunan activate komut dosyasını çalıştırırsanız, bu Python sürümünü "etkinleştirecek" ve onu kabuk için varsayılan `python` komutu haline getirecektir. Örneğin:

```bash
bash % source mypython/bin/activate
(mypython) bash %
```

Burada artık kendiniz için Python paketlerini kurmaya başlayabilirsiniz. Örneğin:


```
(mypython) bash % python -m pip install pandas
...
```

Farklı paketleri deneyimlemek amacıyla sanal bir ortam kurmak genellikle iyi bir şekilde çalışacaktır. Diğer bir yandan, bir uygulama oluşturuyorsunuz ve belirli paket zorunlulukları varsa bu biraz daha farklı bir sorun haline gelebilir.


### Uygulamanızda Üçüncü Taraf Bağımlılıklarını Ele Alma

Bir uygulama yazdıysanız ve belirli üçüncü taraf bağımlılıkları varsa, bir sorun kodunuzu ve bağımlılıkları içeren ortamın oluşturulması ve korunmasıyla ilgilidir. Ne yazık ki, bu Python'un yaşamı boyunca büyük bir kafa karışıklığı ve sık sık değişim alanı oldu. Şimdi bile değişmeye devam ediyor.

Yakında güncelliğini yitirecek bilgiler vermek yerine, sizi [Python Packaging User Guide](https://packaging.python.org).'na yönlendiriyorum.


## Alıştırmalar

### Alıştırma 9.4: Sanal Bir Ortam Yaratmak

Yukarıda gösterildiği gibi, sanal bir ortam oluşturma ve içine pandas yükleme adımlarını yeniden oluşturup oluşturamayacağınıza bakın.

[Contents](../Contents.md) \| [Previous (9.1 Packages)](01_Packages.md) \| [Next (9.3 Distribution)](03_Distribution.md)






