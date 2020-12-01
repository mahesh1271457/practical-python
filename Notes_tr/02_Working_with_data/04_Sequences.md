[Contents](../Contents.md) \| [Previous (2.3 Formatting)](03_Formatting.md) \| [Next (2.5 Collections)](05_Collections.md)

# 2.4 Sequences (Diziler)

### Sequence (Dizi) Veri Tipi

Python'da  üç tane *sequence (dizi)* veri tipi vardır.

* String: `'Hello'`. Stringler bir karakter dizisidir
* List: `[1, 4, 5]`.
* Tuple: `('GOOG', 100, 490.1)`.

Tüm diziler sıraladırlar, integerlar ile indekslenmiş ve bir uzunluğa sahiplerdir.

```python
a = 'Hello'               # String
b = [1, 4, 5]             # List
c = ('GOOG', 100, 490.1)  # Tuple

# İndexli sıralama
a[0]                      # 'H'
b[-1]                     # 5
c[1]                      # 100

# Dizinin uzunluğu
len(a)                    # 5
len(b)                    # 3
len(c)                    # 3
```

Diziler çoğaltılabilirler : `s * n`.

```python
>>> a = 'Hello'
>>> a * 3
'HelloHelloHello'
>>> b = [1, 2, 3]
>>> b * 2
[1, 2, 3, 1, 2, 3]
>>>
```

Diziler elamanları aynı tipite olduklarında birleştirilebilirler `s + t`.

```python
>>> a = (1, 2, 3)
>>> b = (4, 5)
>>> a + b
(1, 2, 3, 4, 5)
>>>
>>> c = [1, 5]
>>> a + c
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: can only concatenate tuple (not "list") to tuple
```

### Slicing (Dilimleme)

Slicing diziden bir alt dizi elde etme anlamına gelir.
Syntax'ı (sözdizimi) `[start:end]` şeklindedir. Nerden başladığı `start` ve nerde bittiği `end` indexleri ile belirlersiniz.

```python
a = [0,1,2,3,4,5,6,7,8]

a[2:5]    # [2,3,4]
a[-5:]    # [4,5,6,7,8]
a[:3]     # [0,1,2]
```

* `start` ve  `end` indexleri integer tipinde olmalıdır.
* Slices(parçalar) son indexteki değeri içermezler. Matematikteki yarı-açık aralık gibide düşünülebilir.
* Eğer indexler ihmal edilimiş ise varsayılan olarak listenin başına veya sonuna giderler.

### Slice(dilim)lerde yeniden atama

Listelerde slices(dilimler) yeniden atanabilir veya silinebilirler.

```python
# Reassignment
a = [0,1,2,3,4,5,6,7,8]
a[2:4] = [10,11,12]       # [0,1,10,11,12,4,5,6,7,8]
```

*Not: Yeniden atamaya yapılan slices (dilimler) eskisiyle aynı uzunluğa sahip olmazlar.*

```python
# Deletion
a = [0,1,2,3,4,5,6,7,8]
del a[2:4]                # [0,1,4,5,6,7,8]
```

### Sequence Reductions (Dizilerde İndirgeme)

Dizileri tek bir değere indirgeme için bazı yaygın fonksiyonlar vardır .

```python
>>> s = [1, 2, 3, 4]
>>> sum(s)
10
>>> min(s)
1
>>> max(s)
4
>>> t = ['Hello', 'World']
>>> max(t)
'World'
>>>
```

### Dizilerde Yineleme (Iteration) 

for-loop (for döngüsü) ile dizilerin elemanları üzerinde dolaşabilirsiniz.

```python
>>> s = [1, 4, 9, 16]
>>> for i in s:
...     print(i)
...
1
4
9
16
>>>
```

Her döngüde çalışabileceğiniz yeni öğeler elde edersiniz.
Bu yeni değer iterasyon değişkeninde (iteration variable) yer alır. Bu örnekte, 
iterasyon değişkenimiz(iteration variable) `x` tir:

```python
for x in s:         # `x` is an iteration variable
    ...statements
```

Her iterasyonda ,yeni değer öncekinin üzerine yazılır (eğer varsa).
Döngü bittiğinde değişken son değerini almış olur.

### break ifadesi (kırma-durdurma)

`break` ifadesini kullanarak döngüyü erken sonlandırabilirsiniz.

```python
for name in namelist:
    if name == 'Jake':
        break
    ...
    ...
statements
```

`break` ifadesi yürütüldüğünde, döngüden çıkar ve bir sonraki ifadeyle devam edilir.
 `break` ifadesi sadece en içteki döngü için geçerlidir. Eğer döngü ile başka bir döngü çalışıyorsa,
dışta olan döngü break ifadesiyle durdurulmayacaktır.

### continue ifadesi (devam etme-atlama)

Bir elamanı atlamak ve bir sonrakine geçmek için `continue` ifadesi kullanılır.

```python
for line in lines:
    if line == '\n':    # Skip blank lines
        continue
    # More statements
    ...
```

Bu, mevcut elamana ihtiyaç duymuyorsanız veya mevcut işlemi görmezden gelmek istiyorsanız kullanmalısınız.

### İntegerlar üzerinde döngü

Eğer saymak istiyorsanız `range()` kullanın.

```python
for i in range(100):
    # i = 0,1,...,99
```

Sözdizimi `range([start,] end [,step])` (başlangıç ve bitiş ve adım)

```python
for i in range(100):
    # i = 0,1,...,99
for j in range(10,20):
    # j = 10,11,..., 19
for k in range(10,50,2):
    # k = 10,12,...,48
    # Notice how it counts in steps of 2, not 1.
```

* Son değer dahil değildir. Bu slice'lar (dilim) ile benzerdir.
* `start` (başlangıcı) opsiyoneldir. Varsayılanı `0` dır.
* `step` (adım) opsiyoneldir. Varsayılanı `1` dir.
* `range()` ihtiyaca göre hesaplar(üretir). Aslında büyük bir sayı deposu gibi değildir.

### enumerate() fonksiyonu

`enumerate` fonksiyonu ekstra hesaplama(sayma) için iterasyona değer sağlar.

```python
names = ['Elwood', 'Jake', 'Curtis']
for i, name in enumerate(names):
    # Loops with i = 0, name = 'Elwood'
    # i = 1, name = 'Jake'
    # i = 2, name = 'Curtis'
```

Genel formu  `enumerate(sequence [, start = 0])`.   `start` opsiyoneldir.
`enumerate()` kullanımı için iyi bir örnekte dosyayı okurken satır sayısını takip etmedir:

```python
with open(filename) as f:
    for lineno, line in enumerate(f, start=1):
        ...
```

Sonuç olarak, `enumerate` güzel bir kısayoldur:

```python
i = 0
for x in s:
    statements
    i += 1
```

`enumerate` kullanımı daha az yazmak içindir ve biraz daha hızlı çalışır.

### For and tuples

Birden fazla iterasyon değişkeni ile iterate(yineleme) yapabilirsiniz.

```python
points = [
  (1, 4),(10, 40),(23, 14),(5, 6),(7, 8)
]
for x, y in points:
    # Loops with x = 1, y = 4
    #            x = 10, y = 40
    #            x = 23, y = 14
    #            ...
```

Birden fazla değişken kullandığında, her tuple *unpacked(paketlenmemiş)* bir iterasyon değişkenine gider.
Değişken sayısı tuple içindeki öğe sayısıyle eşit olmalıdır.

### zip() fonksiyonu

`zip` fonksiyonu birden fazla diziyi alır ve onları birleştiren iterator (yineleyici) yapar.

```python
columns = ['name', 'shares', 'price']
values = ['GOOG', 100, 490.1 ]
pairs = zip(columns, values)
# ('name','GOOG'), ('shares',100), ('price',490.1)
```

Sonucu almak için iterate(yineleyici) yapmalısınız. Daha önce gösterildiği gibi tupleları 
açmak için birden çok değişken kullanabilirsiniz.

```python
for column, value in pairs:
    ...
```

`zip`in yaygın bir kullanımıda anahtar/değer (key/value) çifti yapmaktır.(sözlükleri oluşturmak için)

```python
d = dict(zip(columns, values))
```

## Alıştırmalar

### Alıştırma 2.13: Counting (Sayma)

Basit bir sayma örneği görelim:

```python
>>> for n in range(10):            # Count 0 ... 9
        print(n, end=' ')

0 1 2 3 4 5 6 7 8 9
>>> for n in range(10,0,-1):       # Count 10 ... 1
        print(n, end=' ')

10 9 8 7 6 5 4 3 2 1
>>> for n in range(0,10,2):        # Count 0, 2, ... 8
        print(n, end=' ')

0 2 4 6 8
>>>
```

### Alıştırma 2.14: Daha fazla sequence(dizi) işlemi

Bazı dizi indirgeme işlemlerini etkileşimli olarak deneyin.

```python
>>> data = [4, 9, 1, 25, 16, 100, 49]
>>> min(data)
1
>>> max(data)
100
>>> sum(data)
204
>>>
```

Veri üzerinde döngü deneyelim.

```python
>>> for x in data:
        print(x)

4
9
...
>>> for n, x in enumerate(data):
        print(n, x)

0 4
1 9
2 1
...
>>>
```

Bazen `for` ifadesi, `len()`, ve `range()` kullanmak,
acemiler tarafından paslı bir C programının derinliklerinden 
çıkmış gibi görünen bir tür korkunç kod parçasında kullanılır.

```python
>>> for n in range(len(data)):
        print(data[n])

4
9
1
...
>>>
```

Bunu yapmayın! Okurken sadece gözünüzü kanatmaz,
bellek açısından verimsizdir ve çok daha yavaş çalışır. Sadece normal 
bir `for` döngüsü kullanın , eğer verinin üzerinde 
dolaşmak istiyorsanız. `enumerate()` kullanın,
eğer herhangi bir nedenle indexlere ihtiyaç duyduysanız.

### Alıştırma 2.15: Pratik bir enumerate() örneği

`Data/missing.csv` odsyasını tekrar çağırın (hisse senedi verilerini içeren portföy)
ancak eksik veri içeren bazı satırlar var.  `enumerate()` kullanın,
`pcost.py` programınızı satır numarasını yazdıracak şekilde düzenleyin,
böylece hatalı girdiyle karşılaştığında uyarı verecektir.

```python
>>> cost = portfolio_cost('Data/missing.csv')
Row 4: Couldn't convert: ['MSFT', '', '51.23']
Row 7: Couldn't convert: ['IBM', '', '70.44']
>>>
```

Bunu yaparken, kodunuzdaki bazı parçaları değiştirmen gerekiyor.

```python
...
for rowno, row in enumerate(rows, start=1):
    try:
        ...
    except ValueError:
        print(f'Row {rowno}: Bad row: {row}')
```

### Alıştırma 2.16:  zip() fonksiyonunu kullanmak

 `Data/portfolio.csv`dosyasında, ilk satır başlıkları içerir.
 Önceki tüm kodlarımızda onlardan kurtuluyorduk.

```python
>>> f = open('Data/portfolio.csv')
>>> rows = csv.reader(f)
>>> headers = next(rows)
>>> headers
['name', 'shares', 'price']
>>>
```

Bununla birlikte, başlıkları yararlı bir şey için kullanırsanız ne olur? İşte burada `zip()` 
fonksiyonu fotoğrafa dahil oluyor.  Önce dosya başlıklarını bir veri satırıyla eşleştirmeyi deneyin:

```python
>>> row = next(rows)
>>> row
['AA', '100', '32.20']
>>> list(zip(headers, row))
[ ('name', 'AA'), ('shares', '100'), ('price', '32.20') ]
>>>
```

Uyarı : `zip()` sütun başlıklarını sütun değerleriyle eşleştirdi.
`list()` kullandık ve sonucu listeye çevirdik. Normal olarak, `zip()` bir iterator oluşturur.
Bu olmalı, for-döngüsü bunu kullanır. 
Bu eşleştirme bir sözlük oluşturmak için bir ara adımdır.
Şimdi şunu deneyelim:

```python
>>> record = dict(zip(headers, row))
>>> record
{'price': '32.20', 'name': 'AA', 'shares': '100'}
>>>
```

Bu dönüşüm, bilinmesi gereken en yararlı püf noktalarından biridir,
çok fazla veri dosyası işlerken. Örneğin,`pcost.py` programının çeşitli girdi dosyalarıyla
çalışmasını sağlamak istediğinizi varsayalım ama paylaştığı gerçek sütun numarasına bakılmaksızın,
adı, hisse ve fiyatı görünür.

`pcost.py`dosyasındaki `portfolio_cost()` fonksiyonunu düzenleyelim:

```python
# pcost.py

def portfolio_cost(filename):
    ...
        for rowno, row in enumerate(rows, start=1):
            record = dict(zip(headers, row))
            try:
                nshares = int(record['shares'])
                price = float(record['price'])
                total_cost += nshares * price
            # This catches errors in int() and float() conversions above
            except ValueError:
                print(f'Row {rowno}: Bad row: {row}')
        ...
```

Şimdi fonksiyonunuzu farklı bir veri dosyasında deneyelim.
`Data/portfoliodate.csv` dosyasında şöyle görünecektir:

```csv
name,date,time,shares,price
"AA","6/11/2007","9:50am",100,32.20
"IBM","5/13/2007","4:20pm",50,91.10
"CAT","9/23/2006","1:30pm",150,83.44
"MSFT","5/17/2007","10:30am",200,51.23
"GE","2/1/2006","10:45am",95,40.37
"MSFT","10/31/2006","12:05pm",50,65.10
"IBM","7/9/2006","3:15pm",100,70.44
```

```python
>>> portfolio_cost('Data/portfoliodate.csv')
44671.15
>>>
```

Doğru yaptıysanız, veri dosyası öncekinden tamamen farklı bir sütun biçimine
sahip olsa bile programınızın hala çalıştığını göreceksiniz. Çok havalı!

Burada yapılan değişiklik küçük, ama önemli. Tek bir sabit dosya biçimini okumak için kodlanmış 
'portfolio_cost()' yerine yeni sürüm herhangi bir CSV dosyasını okur ve bunlar dışındaki
ilgilenilen değerleri seçer. Dosya gerekli sütunlara sahip olduğu sürece kod çalışacaktır.

Bölüm 2.3'te yazdığınız 'report.py' programını, sütun başlıklarını seçmek için aynı tekniği kullanacak şekilde düzeltin.

'report.py' programını 'Data/portfoliodate.csv' dosyası üzerinde çalıştırmayı deneyin ve öncekiyle aynı cevabı ürettiğini görün.

### Alıştırma 2.17: Sözlüğü ters çevirmek

Bir sözlük anahtarları (keys) değerlerle (value) eşleştirir. Örneğin, hisse senedi fiyatları sözlüğü.

```python
>>> prices = {
        'GOOG' : 490.1,
        'AA' : 23.45,
        'IBM' : 91.1,
        'MSFT' : 34.23
    }
>>>
```

Eğer items() metodunu kullanırsanız, (anahtar, değer) çiftlerini alabilirsiniz:

```python
>>> prices.items()
dict_items([('GOOG', 490.1), ('AA', 23.45), ('IBM', 91.1), ('MSFT', 34.23)])
>>>
```

Ancak, bunun yerine (değer, anahtar) listelerini almak isteseydiniz?
*İpucu: zip()'i kullanın.*

```python
>>> pricelist = list(zip(prices.values(),prices.keys()))
>>> pricelist
[(490.1, 'GOOG'), (23.45, 'AA'), (91.1, 'IBM'), (34.23, 'MSFT')]
>>>
```

Neden bunu yaptık? İlk olarak zip(), sözlük verileri üzerinde belirli 
türden veri işlemlerini gerçekleştirmenize izin verir.

```python
>>> min(pricelist)
(23.45, 'AA')
>>> max(pricelist)
(490.1, 'GOOG')
>>> sorted(pricelist)
[(23.45, 'AA'), (34.23, 'MSFT'), (91.1, 'IBM'), (490.1, 'GOOG')]
>>>
```

Bu aynı zamanda tuple'ın önemli bir özelliğini gösterir. 
Karşılaştırılmalı durumlarda kullanıldığı zaman, tuple'lar ilk öğe ile başlayarak 
eleman-eleman karşılaştırılır.  String'lerin karakter-karakter karşılaştırılması ile benzerdir.

`zip()` sıklıkla farklı yerlerdeki üst verinin eşleştirilmesi gereken yerlerde kullanılır.  
Örneğin, isimlendirilmiş değerlerin bir sözlüğünü yapmak için sütun isimleri ile sütun değerlerini eşleştirmek.

Not: zip() 'in eşleştirilmelerle sınırlı olmadığını unutmayın. Örneğin,
zip()'i herhangi bir girdi listesinin (input lists) sayısı ile de kullanabilirsiniz:

```python
>>> a = [1, 2, 3, 4]
>>> b = ['w', 'x', 'y', 'z']
>>> c = [0.2, 0.4, 0.6, 0.8]
>>> list(zip(a, b, c))
[(1, 'w', 0.2), (2, 'x', 0.4), (3, 'y', 0.6), (4, 'z', 0.8))]
>>>
```

Ayrıca, zip()'in, en kısa girdi dizisi bittiğinde durduğunu unutmamalıyız.

```python
>>> a = [1, 2, 3, 4, 5, 6]
>>> b = ['x', 'y', 'z']
>>> list(zip(a,b))
[(1, 'x'), (2, 'y'), (3, 'z')]
>>>
```

[Contents](../Contents.md) \| [Previous (2.3 Formatting)](03_Formatting.md) \| [Next (2.5 Collections)](05_Collections.md)
