[İçindekiler](../Contents.md) \| [Önceki (9.2 Üçüncü Part Paketler)](02_Third_party.md) \| [Sonraki (SON)](TheEnd.md)

# 9.3 Dağıtım

Bir noktadan sonra artık kodunuzu bir başkasıyla, belki iş arkadaşlarınızla paylaşmak isteyebilirsiniz.
Bu bölümde, bunu yapmanın en temel tekniğinden bahsedeceğiz. Daha detaylı bilgi için, yanda yazılı olan linke başvurabilirsiniz. [Python Packaging User Guide](https://packaging.python.org)

### 'setup.py ' Dosyasını Oluşturmak


'setup.py' isimli dosyanızı proje dizininizin öncelikli bölümüne ekleyin.

```python
# setup.py
import setuptools

setuptools.setup(
    name="porty",
    version="0.0.1",
    author="Your Name",
    author_email="you@example.com",
    description="Practical Python Code",
    packages=setuptools.find_packages(),
)
```

### MANIFEST.in  Dosyasını Oluşturmak

Projenizle alakalı başka dosyalar var ise onları da 'MANIFEST.in' dosyası altında toplayın.

Örnek:

```
# MANIFEST.in
include *.csv
```

'MANIFEST.in' dosyasını 'setup.py' dosyanızla aynı dizine koyun.

### Kaynak Dağıtımı Oluşturmak

Kodunuzun dağıtımını oluşturmak için 'setup.py' dosyasını kullanın.
Örnek:

```
bash % python setup.py sdist
```
Bu satır `dist/` dizininin içinde `.tar.gz` ya da `.zip` dosyası oluşturacaktır. Bu işlem sonucunda, dosya artık sizin kodlarınızı 
başkalarına verebileceğiniz bir formata dönüşmüştür.

### Kodu İndirmek

İnsanlar sizin Python kodlarınızı diğer paketlerde yaptıkları gibi 'pip' komutuyla indirebilirler.
Bunun için ihtiyaçları olan tek şey bir önceki adımda oluşturduğumuz dosyaya sahip olmaları.

Örnek:

```
bash % python -m pip install porty-0.0.1.tar.gz
```

### Açıklama

Yukarıdaki adımlar başkalarına verebileceğiniz Python kodunun pakete dönüşmesini açıklayan en temel bilgileri anlatmaktadır.
Gerçek hayatta bu adımları uygulamak, uygulamanızın C/C++ gibi başka kodları içerip içermediğine bağlı olarak 
bir üçüncü part durumundan dolayı daha karmaşık olabilir. Bu konuları işlemek, kursun alanının dışında kalıyor.
Biz şimdiye kadar sadece bu konu hakkındaki ilk küçük adımlarımızı atmış olduk.


## Alıştırmalar

### Alıştırma 9.5:  Paket Oluşturmak

Alıştırma 9.3 için yazdığınız `porty-app/` kodunu alın ve burada anlatılan adımları
uygulayabilip uygulayamayacağınızı test edin. Daha net olmak gerekirse, 'setup.py'
ve 'MANIFEST.in' dosyalarınızı öncelikli dizine ekleyin.`python setup.py sdist` kodunu çalıştırarak
kaynak dağıtım dosyasını oluşturun. Son olarak, Python virtual environment'a bu paketi
kurup kuramadığınızı test edin.

[İçindekiler](../Contents.md) \| [Önceki (9.2 Üçüncü Part Paketler)](02_Third_party.md) \| [Sonraki (SON)](TheEnd.md)





