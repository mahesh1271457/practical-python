[Contents](../Contents.md) \| [Previous (2.6 List Comprehensions)](06_List_comprehension.md) \| [Next (3 Program Organization)](../03_Program_organization/00_Overview.md)

# 2.7 Objeler

Bu bölümde pythondaki iç obje modelinden (internal object model) bahsedeceğiz. Aynı zamanda hafıza yönetimi, kopyalama ve tip kontrolü (type checking) hakkında konuşacağız.

### Değer atama işlemi

Pythonda yaptığımız birçok operasyon veri *atamak* veya verileri *saklamak* ile alakalıdır. 

```python
a = değer             # Değişkene değer atama işlemi.
s[n] = değer          # Listeye değer atama işlemi
s.append(değer)       # Listeye değer ekleme işlemi
d['anahtar'] = değer  # Sözlüğe değer ekleme işlemi
```

*UYARI: Değer atama işlemleri, atanan değerin **asla bir kopyasını oluşturmaz.** *
Tüm atama işlemleri sadece referans kopyalarıdır. (İşaretçi (pointer) kopyaları da diyebiliriz.)


### Değer atama örneği

Şu koda bir bakalım

```python
a = [1,2,3]
b = a
c = [a,b]
```

Altta gerçekleşen hafıza (RAM) operasyonları. Bu örnekte sadece bir adet liste objesi ( `[1, 2, 3]` ) var. Ancak bu objeye 4 farklı referans var.


![References](references.png)

Bu da demek oluyor ki listedeki bir değeri değiştirmek *bütün* referansları değiştiriyor.

```python
>>> a.append(999)
>>> a
[1,2,3,999]
>>> b
[1,2,3,999]
>>> c
[[1,2,3,999], [1,2,3,999]]
>>>
```

Orijinal listedeki bir değişimin nasıl da her yerde göründüğünü fark ettiniz mi? Bunun sebebi hiçbir kopyanın oluşturulmamış olmasıdır. Her şey aynı objeye işaret ediyor.

### Yeniden değer atama işlemi

Yeniden değer atadığımızda yeni değer *asla* önceki değerin kullandığı hafıza alanının üzerine yazılmaz.

```python
a = [1,2,3]
b = a
a = [4,5,6]

print(a)      # [4, 5, 6]
print(b)      # [1, 2, 3]    Holds the orijinal değerini koruyor
```

Unutmayın: **Değişkenler isimlerdir, hafızadaki konumlar değil.**

### Bazı tehlikeler

Az önce anlattığım bilgiyi sakın unutmayın. Aksi halde bir yerde hatayla karşılaşabilirsiniz. Tipik bir örnek vereyim, kendi özel kopyanız olduğunu düşünerek bir değeri değiştiriyorsunuz ve programın başka bir bölümünün bozulmasına yol açıyorsunuz. Bu konuda çok dikkatli olun.

*Ek bilgi: Bu anlattığım olay, ilkel veri tiplerinin değiştirilemez (sadece okunabilir)  ((immutable, read-only)) olmasının bir sebebidir.*

### Eşlik ve Referanslar

‘is’ operatörünü kullanarak iki objenin tıpatıp aynı olup olmadığını kontrol edebilirsiniz. 


```python
>>> a = [1,2,3]
>>> b = a
>>> a is b
True
>>>
```


‘is’ operatörü objelerin kimliklerini (bu bir tam sayıdır) karşılaştırır. Bu kimliğe id() fonksiyonunu kullanarak ulaşabilirsiniz.


```python
>>> id(a)
3588944
>>> id(b)
3588944
>>>
```

Not: Objeleri kontrol ederken neredeyse her zaman ‘==’ operatörünü kullanmak ‘is’ operatörünü kullanmaktan daha iyidir. ‘is’ bazen beklenmedik sonuçlar verebilir.

```python
>>> a = [1,2,3]
>>> b = a
>>> c = [1,2,3]
>>> a is b
True
>>> a is c
False
>>> a == c
True
>>>
```


### Sığ kopyalar

Listelerin ve sözlüklerin kopyalama metotları vardır. 

```python
>>> a = [2,3,[100,101],4]
>>> b = list(a) # Kopyasını oluşturuyor.
>>> a is b
False
```

b yeni bir liste. Ama listedeki değerler ortak.

```python
>>> a[2].append(102)
>>> b[2]
[100,101,102]
>>>
>>> a[2] is b[2]
True
>>>
```

Örneğin a listesinin içindeki `[100, 101, 102]` listesi ortak kullanılıyor. Buna *sığ kopya* deniyor. Bir resimle gösterelim:

![Shallow copy](shallow.png)


### Derin kopyalar

Bazen bir objenin ve içinde olan bütün objelerin kopyasını oluşturmamız gerekir. Bu işlem için ‘copy’ modülünü kullanabilirsiniz.

```python
>>> a = [2,3,[100,101],4]
>>> import copy
>>> b = copy.deepcopy(a)
>>> a[2].append(102)
>>> b[2]
[100,101]
>>> a[2] is b[2]
False
>>>
```





### İsimler, değerler, tipler

Değişken adlarının bir *tipi (type)* yoktur. Sadece bir isimden ibaretler. Ancak değerlerin belirli tipleri vardır.

```python
>>> a = 42
>>> b = 'Hello World'
>>> type(a)
<type 'int'>
>>> type(b)
<type 'str'>
```


`type()`, girdiğiniz değerin hangi tipte olduğunu söyler. Çoğunlukla fonksiyon olarak kullanılır.


### Tip kontrolü

Bir objenin belirli bir tipte olup olmadığını kontrol etme yöntemidir.

```python
if isinstance(a, list):
    print('a bir listedir.')
```

Birden fazla tip için de kontrol edebiliriz.

```python
if isinstance(a, (list,tuple)):
    print('a bir liste veya demettir.')
```

*Dikkat: tip kontrolünü aşırı kullanmamaya özen gösterin. Yazdığınız kodu aşırı karmaşık hale dönüştürebilir. Genelde sadece kodunuzu kullanan insanların yanlış tipte değer girmediğinden emin olmak için kullanılır.*

### Her şey bir objedir. 

Sayılar, karakter dizileri (string), fonksiyonlar, hata göstergeleri(exceptions), sınıflar vb. hepsi birer objedir. Yani isimlendirilebilen her obje, hiçbir kısıtlama olmadan veri olarak aktarılabilir, listelerin, sözlüklerin vb içine veri olarak yazılabilir. *Özel* bir obje türü yoktur. Bazen tüm objelerin “birinci sınıf” objeler olduğu da söylenir.

```python
>>> import math
>>> items = [abs, math, ValueError ]
>>> items
[<built-in function abs>,
  <module 'math' (builtin)>,
  <type 'exceptions.ValueError'>]
>>> items[0](-45)
45
>>> items[1].sqrt(2)
1.4142135623730951
>>> try:
        x = int('not a number')
    except items[2]:
        print('Failed!')
Failed!
>>>
```


Yukarıdaki örnekte gördüğümüz `items`, içinde bir fonksiyon, modül ve hata göstergesi (exception) barındıran bir liste. Bunların isimlerini yazarak kullanmak yerine direkt listeden kullanabilirsiniz: 

```python
items[0](-45)       # abs
items[1].sqrt(2)    # math
except items[2]:    # ValueError
```

Ama bunu yapabiliyor olmanız, yapacağınız anlamına gelmiyor. Bu yöntemi kullanmanızı önermiyorum.


### Alıştırmalar

Şimdi yapacağımız alıştırmalarda bu birinci sınıf objelerin bize verdiği bazı güçleri kullanacağız.


### Alıştırma 2.24: Birinci sınıf veri

‘Data/portfolio.csv’ dosyasında sütunlar halinde düzenlenmiş bir veriyi okumuştuk.

```csv
name,shares,price
"AA",100,32.20
"IBM",50,91.10
...
```

Önceki bölümde (2.6 Liste işlevleri) yazdığımız kodda veriyi okumak için ‘csv’ modülünü kullanmıştık. Ancak yine bazı tip dönüşümlerini elle yapmak zorunda kaldık. Mesela:

```python
for row in rows:
    name   = row[0]
    shares = int(row[1])
    price  = float(row[2])
```

Bu tip dönüştürme işlemini temel liste operasyonlarıyla daha akıllıca yapabiliriz.

Değerleri istediğimiz tiplere dönüştüren fonksiyonları sırayla bulunduran bir liste oluşturalım. 

```python
>>> types = [str, int, float]
>>>
```

Böyle bir listeyi oluşturabiliyor olmamızın sebebi biraz önce de bahsettiğim gibi pythonda tüm objelerin *birinci sınıf* olması. Yani fonksiyonlardan oluşan bir liste mi yapmak istiyoruz, yapabiliriz. ‘types’ listesinde de bunu yaptık. Listedeki elemanların her biri `x` değerini belirli bir tipe dönüştüren fonksiyonlar. (`str(x)`, `int(x)`, `float(x)` gibi.)


Şimdi dosyadan bir satır okuyalım.

```python
>>> import csv
>>> f = open('Data/portfolio.csv')
>>> rows = csv.reader(f)
>>> headers = next(rows)
>>> row = next(rows)
>>> row
['AA', '100', '32.20']
>>>
```


Bu satırı okumuş olmamız hesap yapmamız için yeterli değil çünkü veriler istediğimiz tipte değil. Bu şekilde hesap yapmaya çalışırsak program hata verecektir.

```python
>>> row[1] * row[2]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: can't multiply sequence by non-int of type 'str'
>>>
```

Program bize iki karakter dizisini çarpamayacağını söyledi.

Ama önceden oluşturduğumuz `types` listesindeki fonksiyonlarla `row` listesindeki değerleri eşlersek hesap yapabiliriz. Örneğin:

```python
>>> types[1]
<type 'int'>
>>> row[1]
'100'
>>>
```

Değerlerden birini dönüştürelim:

```python
>>> types[1](row[1])     # int(row[1]) yazmakla aynı şey
100
>>>
```

Bir diğer değeri dönüştürelim:

```python
>>> types[2](row[2])     # float(row[2]) yazmakla aynı şey
32.2
>>>
```

Dönüştürdüğümüz değerlerle hesap yapmayı deneyelim:

```python
>>> types[1](row[1])*types[2](row[2])
3220.0000000000005
>>>
```

`types` listesindeki fonksiyonlarla `row` listesindeki değerleri birleştirelim (zip):

```python
>>> r = list(zip(types, row))
>>> r
[(<type 'str'>, 'AA'), (<type 'int'>, '100'), (<type 'float'>,'32.20')]
>>>
```


Her bir tip dönüştürme fonksiyonu, bir değerle eşlendi. Örneğin `int()` fonksiyonu `'100'` değeriyle eşlendi.

Birleştirmiş (ziplenmiş) liste, art arda bütün değerlerin tiplerini değiştirmek istiyorsak kullanışlı olur.

```python
>>> converted = []
>>> for func, val in zip(types, row):
          converted.append(func(val))
...
>>> converted
['AA', 100, 32.2]
>>> converted[1] * converted[2]
3220.0000000000005
>>>
```

Yukarıdaki kodda ne olduğunu, neyi neden yaptığımızı iyice anladığınızdan emin olun. Döngüde `func` adlı değişken, `types` listesindeki tip dönüştürme fonksiyonlarından biri (int, str, float). `val` değişkeni ise `row` listesindeki elemanlardan biri (`AA`, `100` gibi). 

func(val) ifadesi de sırayla `types` listesindeki fonksiyonları `row` listesindeki değerlere uyguluyor.

Yukarıda uzun uzun yazdığımız kodun yaptığı işlemi tek satırlık liste işleviyle de yapabiliriz.

```python
>>> converted = [func(val) for func, val in zip(types, row)]
>>> converted
['AA', 100, 32.2]
>>>
```



### Alıştırma 2.25
Elinizde bir dizi `anahtar:değer` çiftleriniz varsa `dict()` fonksiyonunu kullanarak kolayca sözlük oluşturabileceğinizi hatırlıyorsunuz değil mi?
Şimdi bunu kullanarak sütünların başlıklarından bir sözlük oluşturalım:

```python
>>> headers
['name', 'shares', 'price']
>>> converted
['AA', 100, 32.2]
>>> dict(zip(headers, converted))
{'price': 32.2, 'name': 'AA', 'shares': 100}
>>>
```


Sözlük işlevi kabiliyetleriniz yeterince güçlendiyse sözlük işlevi de kullanabilirsiniz elbette.

```python
>>> { name: func(val) for name, func, val in zip(headers, types, row) }
{'price': 32.2, 'name': 'AA', 'shares': 100}
>>>
```
 

### Alıştırma 2.26: Büyük Resim

Bu bölümde öğrendiğiniz yöntemlerle sütunlardan oluşan çoğu veri dosyasındaki verileri kolayca bir python sözlüğüne dönüştürebilirsiniz.

Örneğin aşağıdaki gibi farklı bir dosyadan veri okuyacaksınız:

```python
>>> f = open('Data/dowstocks.csv')
>>> rows = csv.reader(f)
>>> headers = next(rows)
>>> row = next(rows)
>>> headers
['name', 'price', 'date', 'time', 'change', 'open', 'high', 'low', 'volume']
>>> row
['AA', '39.48', '6/11/2007', '9:36am', '-0.18', '39.67', '39.69', '39.45', '181800']
>>>
```

Biraz önce kullandığımız yöntemi yeniden kullanarak veri tiplerini istediğimiz tipe dönüştürelim.

```python
>>> types = [str, float, str, str, float, float, float, float, int]
>>> converted = [func(val) for func, val in zip(types, row)]
>>> record = dict(zip(headers, converted))
>>> record
{'volume': 181800, 'name': 'AA', 'price': 39.48, 'high': 39.69,
'low': 39.45, 'time': '9:36am', 'date': '6/11/2007', 'open': 39.67,
'change': -0.18}
>>> record['name']
'AA'
>>> record['price']
39.48
>>>
```


Bonus soru: Yukarıdaki örnekteki `date` sütunundaki verileri (6, 11, 2007) gibi bir demete nasıl dönüştürürdünüz?

Bu bonus soruyla biraz vakit harcayıp bu bölümde öğrendiklerinizi pekiştirin. İleriki bölümlerde bu fikirlere yeniden döneceğiz.

[Contents](../Contents.md) \| [Previous (2.6 List Comprehensions)](06_List_comprehension.md) \| [Next (3 Program Organization)](../03_Program_organization/00_Overview.md)

