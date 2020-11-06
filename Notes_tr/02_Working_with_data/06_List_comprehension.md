[Contents](../Contents.md) \| [Previous (2.5 Collections)](05_Collections.md) \| [Next (2.7 Object Model)](07_Objects.md)

# 2.6 Liste işlevleri (list comprehension)
Python’da listeler yaygın olarak kullanılır. Bu bölümde listedeki elemanları işlemek/değiştirmek için güçlü bir araç olan liste işlevlerini (list comprehension)  anlatacağım.

### Yeni liste oluşturmak
Liste işlevlerini kullanarak bir dizideki her elemanı belli bir işleme sokarak yeni bir liste oluşturabilirsiniz.

```python
>>> a = [1, 2, 3, 4, 5]
>>> b = [2*x for x in a ]
>>> b
[2, 4, 6, 8, 10]
>>>
```

Başka bir örnek:
```python
>>> names = ['Elif', 'Jale']
>>> a = [name.lower() for name in names]
>>> a
['elif', 'jale']
>>>
```

Genel olarak yazım kuralımız (syntax) 
[ <işlem> for <değişken_adı> in <dizi> ] diyebiliriz.



### Filtreleme

Liste işlevlerini kullanırken verileri filtreleyebilirsiniz de.

```python
>>> a = [1, -5, 4, 2, -2, 10]
>>> b = [2*x for x in a if x > 0 ]
>>> b
[2, 8, 4, 20]
>>>
```

### Kullanım Alanları

Liste işlevleri çok kullanışlıdır. Örneğin bir sözlüğün belirli bir alanlarındaki (field) değerleri alabilirsiniz.

```python
stocknames = [s['name'] for s in stocks]
```

Diziler üzerinde veri tabanlarındaki gibi sorgular yapabilirsiniz.

```python
a = [s for s in stocks if s['price'] > 100 and s['shares'] > 50 ]
```

Liste işlevlerini dizi indirgemesiyle (sequence reduction) de kullanabilirsiniz.

```python
cost = sum([s['shares']*s['price'] for s in stocks])
```


Genel yazım kuralımız (syntax) böyle diyebiliriz :
```code
[ <işlem> for <değişken_adı> in <dizi> if <koşul>]
```

Uzun şekilde yazarsak:

```python
result = []
for variable_name in sequence:
    if condition:
        result.append(expression)
```


### Ufak bir not

Liste işlevleri, matematikte kullanılan ortak özelliklerle küme oluşturma yönteminden esinlenilerek yapılmıştır.

```code
a = [ x * x for x in s if x > 0 ]  # Python

a = { x^2 | x ∈ s, x > 0 }         # Matematik
```
Python dışında birkaç başka dilde de bu özellik vardır. Çoğu kod yazan kişi liste işlevlerini sadece faydalı bir kısa yol olarak düşünür ama aklınızın bir köşesinde bu bilgi de bulunsun.


### Alıştırmalar

Alıştırmalarda hisse portföylerini kullanacağız. Verileri kullanabilmek için için ‘report.py’ adlı programı çalıştırarak başlayalım.

```bash
bash % python3 -i report.py
```

Python’un interaktif arayüzüne girdik. Aşağıda belirtilen işlemleri yapmak için gerekli ifadeleri yazalım. Bu işlemler kullanacağımız veri üzerinde indirgeme, dönüşüm ve bazı sorgular (query) yapıyor.

### Alıştırma 2.19 : Liste işlevleri

Yazım kurallarına ve kullanmaya kendimizi alıştırmak için birkaç basit liste işlevi kodu yazalım. 

```python
>>> sayılar = [1,2,3,4]
>>> kareler = [ x * x for x in sayılar ]
>>> kareler
[1, 4, 9, 16]
>>> iki_katı = [ 2 * x for x in sayılar if x > 2 ]
>>> iki_katı
[6, 8]
>>>
```

Liste işlevleri uygun şekilde dönüştürülmüş/filtrelenmiş verilerle yeni bir liste oluşturuyor.


### Alıştırma 2.20: Dizi İndirgemeleri

1 satırlık python koduyla portföyün toplam maliyetini hesaplayalım.

```python
>>> portfolio = read_portfolio('Data/portfolio.csv')
>>> cost = sum([ s['shares'] * s['price'] for s in portfolio ])
>>> cost
44671.15
>>>
```

Bunu yaptıktan sonra tek satırlık kodla portföyün değerini hesaplayalım.

```python
>>> value = sum([ s['shares'] * prices[s['name']] for s in portfolio ])
>>> value
28686.1
>>>
```

Yukarıda yaptığımız iki işlem de birer “map-reduction” örneği. Liste işlevi de bu işlemleri (reduction, indirgeme) tüm listeye uygulamamızı (map) sağlıyor. 

```python
>>> [ s['shares'] * s['price'] for s in portfolio ]
[3220.0000000000005, 4555.0, 12516.0, 10246.0, 3835.1499999999996, 3254.9999999999995, 7044.0]
>>>
```

Sonrasında da sum() fonksiyonu ile sonucu indirgiyoruz. Adından da anlayabileceğiniz üzere listedeki bütün değerleri topluyor.

```python
>>> sum(_)
44671.15
>>>
```


Sadece bu bilgileri kullanarak bile bir büyük veri startup’ı kurabilirsiniz.


### Alıştırma 2.21: Veri Sorguları

Aşağıdaki örnekleri çeşitli veri sorgularıyla deneyin.
Öncelikle yüzden fazla payı olan portföy varlıklarını listeleyelim.

```python
>>> more100 = [ s for s in portfolio if s['shares'] > 100 ]
>>> more100
[{'price': 83.44, 'name': 'CAT', 'shares': 150}, {'price': 51.23, 'name': 'MSFT', 'shares': 200}]
>>>
```

MSFT ve IBM hisseleri için bütün portföy varlıklarını bulalım.

```python
>>> msftibm = [ s for s in portfolio if s['name'] in {'MSFT','IBM'} ]
>>> msftibm
[{'price': 91.1, 'name': 'IBM', 'shares': 50}, {'price': 51.23, 'name': 'MSFT', 'shares': 200},
  {'price': 65.1, 'name': 'MSFT', 'shares': 50}, {'price': 70.44, 'name': 'IBM', 'shares': 100}]
>>>
```

$10000’dan daha maliyetli olan porföy varlıklarını bulalım.

```python
>>> maliyet10k = [ h for h in portföy if h['paylar'] * h['fiyat'] > 10000 ]
>>> maliyet10k
[{'fiyat': 83.44, 'isim': 'CAT', 'paylar': 150}, {'fiyat': 51.23, 'isim': 'MSFT', 'paylar': 200}]
>>>
```

### Alıştırma 2.22: Veri Çekmek(extract)

Şimdi de (isim, paylar) şeklinde demetlerden (tuple) oluşan bir liste oluşturalım. İsim ve paylar verilerini portföy veri setinden çekeceğiz.

```python
>>> name_shares =[ (s['name'], s['shares']) for s in portfolio ]
>>> name_shares
[('AA', 100), ('IBM', 50), ('CAT', 150), ('MSFT', 200), ('GE', 95), ('MSFT', 50), ('IBM', 100)]
>>>
```

Kare parantezleri ( ‘[]’ ) küme paranteziyle ( ‘{}’ ) değiştirirsek kullandığımız ‘liste işlevleri’ ‘küme işlevleri’ne (set comprehension) dönüşür. Matematikte olduğu gibi pythonda da kümelerdeki değerler eşsizdir, her eleman farklı bir değere sahiptir.

Bu küme işlevini kullanarak portföy veri setindeki tüm hisse isimlerinin kümesini bulalım.

```python
>>> names = { s['name'] for s in portfolio }
>>> names
{ 'AA', 'GE', 'IBM', 'MSFT', 'CAT'] }
>>>
```

Eğer ‘anahtar:değer’ çiftleri belirtirsek bir sözlük oluşturabiliriz. Örnek olarak hisse isimleri ve tuttukları toplam payları eşleyen bir sözlük oluşturalım. Buna ‘sözlük işlevleri’ (dictionary comprehension) deniyor.

```python
>>> holdings = { name: 0 for name in names } # Sözlüğü oluşturuyoruz.
>>> holdings
{'AA': 0, 'GE': 0, 'IBM': 0, 'MSFT': 0, 'CAT': 0}
>>>
>>> for s in portfolio:                      # Hisse isimleriyle tuttukları
        holdings[s['name']] += s['shares']   # toplam payı eşliyoruz.

>>> holdings
{ 'AA': 100, 'GE': 95, 'IBM': 150, 'MSFT':250, 'CAT': 150 }
>>>

```

### Alıştırma 2.23 CSV dosyalarından veri çekmek(extract)

Liste, küme ve sözlük işlevlerini çeşitli kombinasyonlarla kullanmayı bilmek çeşitli veri işleme problemlerini çözmekte faydalı olacaktır. Bir CSV dosyasının belirli sütunlarından nasıl veri çekebileceğimizi gösteren bir örnek yapalım.


```python
>>> import csv
>>> f = open('Data/portfoliodate.csv')
>>> rows = csv.reader(f)
>>> headers = next(rows)
>>> headers
['name', 'date', 'time', 'shares', 'price']
>>>
```

CSV verisini okuduk, şimdi de almak istediğimiz sütunların adlarından bir liste oluşturalım.

```python
>>> select = ['name', 'shares', 'price']
>>>
```

Sonrasında istediğimiz sütunların indislerini bulalım.

```python
>>> indices = [ headers.index(colname) for colname in select ]
>>> indices
[0, 3, 4]
>>>
```

Son olarak da satırları okuyup sözlük işlevini kullanarak bir sözlük oluşturalım


```python
>>> row = next(rows)
>>> record = { colname: row[index] for colname, index in zip(select, indices) }   # dict-comprehension
>>> record
{'price': '32.20', 'name': 'AA', 'shares': '100'}
>>>
```

Az önce yaptığımız işlemleri rahatça anladıysanız, geri kalan kısmı da okuyabilirsiniz.

```python
>>> portfolio = [ { colname: row[index] for colname, index in zip(select, indices) } for row in rows ]
>>> portfolio
[{'price': '91.10', 'name': 'IBM', 'shares': '50'}, {'price': '83.44', 'name': 'CAT', 'shares': '150'},
  {'price': '51.23', 'name': 'MSFT', 'shares': '200'}, {'price': '40.37', 'name': 'GE', 'shares': '95'},
  {'price': '65.10', 'name': 'MSFT', 'shares': '50'}, {'price': '70.44', 'name': 'IBM', 'shares': '100'}]
>>>
```

Vay be, read_portfolio() fonksiyonunun büyük bir kısmını tek bir ifadeye indirgedik.



### 

Liste işlevleri genelde python’da veriyi dönüştürmenin, filtrelemenin ve veri toplamanın verimli bir yolu olarak kullanılır. Yazım kurallarından dolayı aşırı uzun liste işlevi yazmamak okunabilirlik açısından daha iyi olacaktır, bu yüzden liste işlevlerini olabildiğince kısa tutmaya çalışın. Her şeyi tek adımda yapmak zorunda değiliz sonuçta, yapacağımız işlemi bir adım yerine birkaç adımda yapmak daha iyi olacaktır. 

Sözün özü, veriyi nasıl hızlıca manipüle edeceğimizi bilmek çok işimize yarayacaktır. Bu bölümde öğrendiğiniz bilgileri kullanmanız gereken birçok problem olacak. Liste, küme ve sözlük işlevlerini kullanmakta uzmanlaştığınızda karşılaştığınız problemlere çözüm bulmanız çok daha kısa sürecektir. ‘collections’ modülünü de unutmayın.


[Contents](../Contents.md) \| [Previous (2.5 Collections)](05_Collections.md) \| [Next (2.7 Object Model)](07_Objects.md)

