[Contents](../Contents.md) \| [Previous (3.3 Error Checking)](03_Error_checking.md) \| [Next (3.5 Main Module)](05_Main_module.md)

# 3.4 Modüller
 Bu bölüm,modül kavramını ve birden çok dosyaya yayılan
 işlevler ile çalışmayı tanıtır.


### Modüller ve İçe Aktarma

 Herhangi bir Python kaynak dosyası bir modüldür. 

```python
# foo.py
def grok(a):
    ...
def spam(b):
    ...
```
 `import` ifadesi yükler ve modülü *çalıştırır*.

```python
# program.py
import foo

a = foo.grok(2)
b = foo.spam('Hello')
...
```

### Namespaces (İsim Alanı)
 Modül, adlandırılmış değerler topluluğudur ve bazen 
*isim alanı* olduğu söylenir. Adlar, kaynak dosyada 
tanımlanan tüm genel değişkenler ve işlevlerdir. İçe 
aktarmadan sonra modül ismi ön ek olarak kullanılır.
Bu nedenle *isim alanı*'dır.

```python
import foo

a = foo.grok(2)
b = foo.spam('Hello')
...
```

Modül ismi doğrudan dosya ismine bağlıdır (foo -> foo.py).

### Global Tanımlar
 *Global* kapsamında tanımlanan her şey, modül, *isim alanı*'nı
dolduran şeydir. İkisi de aynı `x` değişkeni tanımlayan iki modülü
değerlendirelim.

```python
# foo.py
x = 42
def grok(a):
    ...
```

```python
# bar.py
x = 37
def spam(a):
    ...
```

Bu durumda,`x` tanımları farklı değişkenleri ifade eder.
Birisi `foo.x`, bir diğeri ise `bar.x`'dir. Farklı modüller
aynı isimleri kullanabilir ve bu isimler ve birbiriyle çelişmez.

**Modüller izole edilir.**
 
### Ortam Olarak Modüller
 Modüller,içinde tanımlanan bütün kodlar için bir çevre ortamı
oluşturur.

```python
# foo.py
x = 42

def grok(a):
    print(x)
```
*Global* değişkenler her zaman çevreleyen modüle bağlıdır (aynı dosyaya).
Her kaynak dosya kendi küçük evrenidir. 

### Modül Yürütme
 Bir modül içe aktarıldığında, *modüldeki tüm ifadeler* dosyanın 
sonuna ulaşılana kadar birbiri ardına çalıştırılır. Modül isim
alanının içeriği, yürütme işleminin sonunda hala tanımlanan 
*global* adların tümüdür. Eğer genel kapsamda görevleri yürüten 
bir komut dosyası ifadesi (yazdırma,dosya oluşturma,vb.) varsa,
onları içe aktarım sırasında çalıştığını göreceksiniz.


### `import as` komutu
 Bir modülü içe aktarırken modülün ismini değiştirebilirsiniz:

```python
import math as m
def rectangular(r, theta):
    x = r * m.cos(theta)
    y = r * m.sin(theta)
    return x, y
```
 Normal bir alma işlemiyle aynı şekilde çalışır. Sadece o dosyadaki 
modülün ismini yeniden adlandırır.

### `from` modül alma
Bu, seçilen sembolleri bir modülden seçer ve yerel olarak kullanılabilir
hale getirir.

```python
from math import sin, cos

def rectangular(r, theta):
    x = r * cos(theta)
    y = r * sin(theta)
    return x, y
```
 Bu, modül önekini yazmak zorunda kalmadan modülün parçalarının kullanılmasına 
izin verir. Sık kullanılan isimler için kullanışlıdır.


### İçe Aktarmada Yorum
 İçe aktarmadaki çeşitlilikler modüllerin çalışma şeklini değiştirmez.

```python
import math
# vs
import math as m
# vs
from math import cos, sin
...
```
Özellikle, `import` her zaman bütün dosyayı yürütür ve modüller
hala yalıtılmış ortamlardır.

`import module as` ifadesi sadece yerel olarak adı değiştiriyor.
`from math import cos, sin` ifadesi perde arkasındaki bütün matematik modülü 
yükler. Bittikten sonra, sadece 'cos' ve 'sin' isimlerini modülden yerel alana 
kopyalar.

### Modül İndirme

Her modül *yalnızca bir kez* yüklenir ve çalıştırılır.
*Not: Tekrarlanan içe aktarmalar sadece önceden yüklenen modüle bir referans 
döndürür.*
  
`sys.modules` yüklenen tüm modüllerin diktesidir. 


```python
>>> import sys
>>> sys.modules.keys()
['copy_reg', '__main__', 'site', '__builtin__', 'encodings', 'encodings.encodings', 'posixpath', ...]
>>>
```

*Dikkat*: Eğer `import`(içe aktarma) ifadesi bir modülün kaynak kodu değiştikten sonra tekrarlanırsa,
sık karşılaşılan bir karışklık ortaya çıkar. `sys.modules` modül önbelleği nedeniyle, tekrarlanan içe aktarımlar
her zaman bir önceki yüklenen modüle döner--bir değişiklik yapılsa bile. Değiştirilen kodu Python'a 
yüklemenin en güvenli yolu, yorumcuyu kapatıp tekrar başlatmaktır.

### Modülleri Bulma
 Python,modülleri ararken bir yol listesine (sys.path) başvurur.

```python
>>> import sys
>>> sys.path
[
  '',
  '/usr/local/lib/python36/python36.zip',
  '/usr/local/lib/python36',
  ...
]
```
Geçerli çalışma dizini genellikle ilk olandır.


### Modül Arama Yolu
 Not ettiğim gibi, `sys.path` arama yolu içerir. Eğer ihtiyacınız olursa manuel olarak
ayarlayabilirsiniz. 

```python
import sys
sys.path.append('/project/foo/pyfiles')
```
Ayrıca erişim yolu(path) ortam değişkenleri aracılığıyla da eklenebilir.

```python
% env PYTHONPATH=/project/foo/pyfiles python3
Python 3.6.0 (default, Feb 3 2017, 05:53:21)
[GCC 4.2.1 Compatible Apple LLVM 8.0.0 (clang-800.0.38)]
>>> import sys
>>> sys.path
['','/project/foo/pyfiles', ...]
```
Genel kural olarak, modül arama yolunu manuel olarak ayarlamak gerekli olmamalıdır.
Ancak, bazen olağandışı bir konumda bulunan Python kodunu içe aktarmak veya
kolayca erişilemeyen mevcut çalışma dizininden dolayı ortaya çıkar. 

## Alıştırmalar

Modülleri içeren bu alıştırma için, Python'u uygun bir 
ortamda çalıştırdığınızdan emin olun. Modüller genellikle programcıların
geçerli çalışma dizini veya Python'un yol ayarlarıyla ilgili sorunlarla karşılaşmasında
kullanılır. Bu kurs için, tüm kodunuzu `İş/ dizini`'nde yazdığınız varsayılır.
En iyi sonuçlar için, tercümanı başlattığınızda da bu dizinde olduğunuzdan emin olmalısınız.
Değilse,`pratik-python/Work`'ün `sys.path`'e eklenmiş olduğuna emin olmanız
gerekir.

### Alıştırma 3.11: Modül Alma

Bölüm 3'te CSV veri dosyalarının içeriğini ayrıştırmak için genel bir amaç
işlevi `parse_csv()` oluşturduk. Şimdi, bu fonksiyonu diğer programlarda nasıl kullanacağımızı
göreceğiz. İlk olarak yeni bir shell penceresi açmakla başlayın.
Tüm dosyalarınızın bulunduğu klasöre gidin. Onları içe aktaracağız.

Python etkileşimli modu başlatın.
 
```shell
bash % python3
Python 3.6.1 (v3.6.1:69c0db5050, Mar 21 2017, 01:21:04)
[GCC 4.2.1 (Apple Inc. build 5666) (dot 3)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>>
```
Bunu yaptıktan sonra, daha önce yazdığınız programlardan bazılarını içe
aktarmayı deneyin. Çıktıların tıpatıp eskisiyle aynı olduğunu göreceksiniz.
Sadece vurgulamak için, bir modülü içe aktarma onun kodunu çalıştırır.

```python
>>> import bounce
... watch output ...
>>> import mortgage
... watch output ...
>>> import report
... watch output ...
>>>
```

Eğer hiçbirisi çalışmazsa, büyük ihtimalle Python'ı yanlış bir dizinde çalıştırıyorsunuz.
Şimdi, `fileparse` modülü içe aktarmaya çalışın ve bu konuda biraz yardım almayı deneyin.

```python
>>> import fileparse
>>> help(fileparse)
... look at the output ...
>>> dir(fileparse)
... look at the output ...
>>>
```

Bazı bilgileri okumak için modülü deneyin:

```python
>>> portfolio = fileparse.parse_csv('Data/portfolio.csv',select=['name','shares','price'], types=[str,int,float])
>>> portfolio
... look at the output ...
>>> pricelist = fileparse.parse_csv('Data/prices.csv',types=[str,float], has_headers=False)
>>> pricelist
... look at the output ...
>>> prices = dict(pricelist)
>>> prices
... look at the output ...
>>> prices['IBM']
106.11
>>>
```
Modül adını eklemeniz gerekmeyecek şekilde bir işlevi içe aktarmayı deneyin:

```python
>>> from fileparse import parse_csv
>>> portfolio = parse_csv('Data/portfolio.csv', select=['name','shares','price'], types=[str,int,float])
>>> portfolio
... look at the output ...
>>>
```

### Alıştırma 3.12: Kütüphane Modülünüzü Kullanma
Bölüm 2'de, şöyle bir hisse senedi raporu hazırlayan 'report.py' programını yazdınız:
 
```
      Name     Shares      Price     Change
---------- ---------- ---------- ----------
        AA        100       9.22     -22.98
       IBM         50     106.28      15.18
       CAT        150      35.46     -47.98
      MSFT        200      20.89     -30.34
        GE         95      13.48     -26.89
      MSFT         50      20.89     -44.21
       IBM        100     106.28      35.84
```
 Bu programı alın ve tüm giriş dosyası işlemleri `fileparse` modülündeki işlevleri kullanarak 
yapılır şekilde değiştirin. Bunu yapmak için,`fileparse`'i modül olarak içeri aktar ve
`parse_csv()` işlevini kullanmak için `read_portfolio()` ve `read_prices()` işlevlerini değiştirin.
Bu alıştırmanın başındaki etkileşimli örneği kılavuz olarak kullanın. Daha sonra, daha önce olduğu gibi tam olarak aynı çıktı almalısınız.


### Alıştırma 3.14: Daha Fazla Kütüphane İçe Aktarımı Kullanma
 1.bölümde, bir portföyü okuyan ve maliyetini hesaplayan bir `pcost.py`
programı yazdınız.

```python
>>> import pcost
>>> pcost.portfolio_cost('Data/portfolio.csv')
44671.15
>>>
```
`pcost.py` dosyasını `report.read_portfolio()` işlevini kullanacak şekilde değiştirin.

## Yorum
Bu alıştırmayı bitirince, elinde 3 tane programın olması gerekir.
Genel bir amaç içeren `fileparse.py`, `parse_csv()` işlevi, güzel bir rapor
oluşturan `report.py`, ancak ayrıca `read_portfolio()` ve `read_prices()` işlevlerini
de içerir. Ve son olarak, portföy maliyetini hesaplayan ancak `report.py` programı için
yazılmış `read_portfolio()` işlevini kullanan `pcost.py`.

[Contents](../Contents.md) \| [Previous (3.3 Error Checking)](03_Error_checking.md) \| [Next (3.5 Main Module)](05_Main_module.md)
