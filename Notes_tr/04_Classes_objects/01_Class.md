[Contents](../Contents.md) \| [Previous (3.6 Design discussion)](../03_Program_organization/06_Design_discussion.md) \| [Next (4.2 Inheritance)](02_Inheritance.md)

# 4.1 Sınıflar

Bu bölüm sınıf ifadesini ve yeni nesneler yaratma fikrini tanıtır.

### Nesne Yönelimli Programlama(OOP)

Kodun *nesneler* koleksiyonu olarak organize edildiği bir programlama tekniği.

Bir * nesne * şunlardan oluşur:

* Veriler. Öznitellikler
* Davranış. Nesneye uygulanan fonksiyonlar(metotlar).

Bu kurs sırasında zaten biraz Nesne Tabanlı Programlama kullanıyorsunuz.

Örneğin, bir listeyi manipüle etmek.

```python
>>> numaralar = [1, 2, 3]
>>> numaralar.append(4)      # Metot
>>> numaralar.insert(1,10)   # Metot
>>> numaralar
[1, 10, 2, 3, 4]        # Veri
>>>
```

`numaralar` bir listenin *örneğidir*.

Metotlar (`append()` and `insert()`), ("numaralar") örneğine eklenir.

### `Sınıf` ifadesi

Yeni bir nesne tanımlamak için "class" ifadesini kullanın.

```python
class Player:
    def __init__(self, x, y):
        self.x = x
        self.y = y
        self.health = 100

    def move(self, dx, dy):
        self.x += dx
        self.y += dy

    def damage(self, pts):
        self.health -= pts
```

Özetle, sınıf, sözde * örnekler * üzerinde çeşitli işlemler gerçekleştiren bir dizi fonksiyondur.

### Örnekler

Örnekler, programınızda manipüle ettiğiniz gerçek *nesnelerdir*.

Sınıfı bir fonksiyon olarak çağırarak oluşturulurlar.

```python
>>> a = Oyuncu(2, 3)
>>> b = Oyuncu(10, 20)
>>>
```

`a` ve `b`, `Oyuncu`nun örnekleridir.

*Dikkat: Sınıf ifadesi sadece tanımdır(kendi başına hiçbir şey yapmaz). Fonksiyon tanımına benzer.*

### Örnek Veri

Her örneğin kendi yerel verileri vardır.

```python
>>> a.x
2
>>> b.x
10
```

Bu veri `__init__()` ile ilklendirilir.

```python
class Oyuncu:
    def __init__(self, x, y):
        # "Self" de depolanan herhangi bir değer örnek verileridir.
        self.x = x
        self.y = y
        self.health = 100
```

Saklanan özniteliklerin türü veya toplam sayısı konusunda herhangi bir kısıtlama yoktur.

###  Örnek Metotları

Örnek yöntemleri, bir nesnenin örneklerine uygulanan fonksiyonlardır.

```python
class Oyuncu:
    ...
    # `move` bir metottur.
    def move(self, dx, dy):
        self.x += dx
        self.y += dy
```

Nesnenin kendisi her zaman ilk argüman olarak aktarılır.

```python
>>> a.move(1, 2)

#  `a` ile `self` eşleşir.
#  `1` ile `dx` eşleşir.
#  `2` ile `dy` eşleşir.
def move(self, dx, dy):
```

Geleneksel olarak, örnek `self` olarak adlandırılır.Bununla birlikte, 
kullanılan gerçek ad önemsizdir.Nesne her zaman ilk argüman olarak aktarılır.
Bu argümanı `self` olarak adlandırmak yalnızca Python programlama stilidir.

### Sınıf Kapsamı

Sınıflar bir ad kapsamını tanımlamaz.

```python
class Oyuncu:
    ...
    def move(self, dx, dy):
        self.x += dx
        self.y += dy

    def left(self, amt):
        move(-amt, 0)       # HAYIR. Global bir `move` işlevi çağırır.
        self.move(-amt, 0)  # EVET. Yukarıdan `move` metodunu çağırır.
```

Bir örnek üzerinde çalışmak istiyorsanız, ona her zaman açıkça atıfta bulunursunuz  (örneğin, `self`).

## Alıştırmalar

Bu alıştırmalardan başlayarak, önceki bölümlerden mevcut kodda bir 
dizi değişiklik yapmaya başlarız. Başlamak için Egzersiz 3.18'in 
çalışan bir sürümüne sahip olmanız çok önemlidir. Buna sahip 
değilseniz, lütfen `Solutions/3_18` dizininde bulunan çözüm 
kodundan çalışın. Kopya sorun değil.


### Alıştırma 4.1: Veri Yapıları Olarak Nesneler

Bölüm 2 ve 3'te, demet ve sözlük olarak temsil edilen verilerle çalıştık.
Örneğin, bir hisse senedi şu şekilde bir demet olarak temsil edilebilir:

```python
s = ('GOOG',100,490.10)
```

veya bunun gibi bir sözlük olarak:

```python
s = { 'name'  : 'GOOG',
      'shares': 100,
      'price' : 490.10
}
```

Bu tür verileri manipüle etmek için fonksiyonlar bile yazabilirsiniz.

```python
def cost(s):
    return s['shares'] * s['price']
```

Bununla birlikte, programınız büyüdükçe daha iyi bir organizasyon
duygusu yaratmak isteyebilirsiniz.  Bu nedenle, verileri temsil etmek
için başka bir yaklaşım bir sınıf tanımlamak olacaktır.  `Stock.py`
adlı bir dosya oluşturun ve tek bir hisse senedi tutmayı temsil eden
bir `Stock` sınıfı tanımlayın.  "Hisse Senedi" örneklerinin `name`, 
`shares`, and `price` özelliklerine sahip olmasını sağlayın. Örneğin:

```python
>>> import stock
>>> a = stock.Stock('GOOG',100,490.10)
>>> a.name
'GOOG'
>>> a.shares
100
>>> a.price
490.1
>>>
```

Birkaç `Stock` nesnesi daha oluşturun ve bunları manipüle edin. 
Örneğin:

```python
>>> b = stock.Stock('AAPL', 50, 122.34)
>>> c = stock.Stock('IBM', 75, 91.75)
>>> b.shares * b.price
6117.0
>>> c.shares * c.price
6881.25
>>> stocks = [a, b, c]
>>> stocks
[<stock.Stock object at 0x37d0b0>, <stock.Stock object at 0x37d110>, <stock.Stock object at 0x37d050>]
>>> for s in stocks:
     print(f'{s.name:>10s} {s.shares:>10d} {s.price:>10.2f}')

... çıktıya bakın ...
>>>
```
Burada vurgulanması gereken bir nokta, `Stock` sınıfının,
nesnelerin örneklerini oluşturmak için bir üretici gibi 
davranmasıdır.  Temelde ona bir fonksiyon diyorsunuz ve o
sizin için yeni bir nesne yaratıyor.  Ayrıca, her nesnenin
farklı olduğu vurgulanmalıdır---her birinin, oluşturulmuş 
diğer nesnelerden ayrı kendi verileri vardır.

Bir sınıf tarafından tanımlanan bir nesne, bir sözlüğe biraz
benzer - sadece biraz farklı sözdizimiyle.  Örneğin,
`s['name']` veya `s['price']`yazmak yerine, artık `s.name`ve
`s.price`yazarsınız.


### Alıştırma 4.2: Bazı Metotlar(Yöntemler) Ekleme

Sınıflarla nesnelerinize fonksiyonlar ekleyebilirsiniz.
Bunlar yöntemler olarak bilinir ve bir nesnenin içinde
depolanan veriler üzerinde çalışan fonksiyonlardır.
`Stock` nesnenize bir `cost()` ve `sell()` metodu ekleyin.
Bu şekilde çalışmaları gerekir:


```python
>>> import stock
>>> s = stock.Stock('GOOG', 100, 490.10)
>>> s.cost()
49010.0
>>> s.shares
100
>>> s.sell(25)
>>> s.shares
75
>>> s.cost()
36757.5
>>>
```

### Alıştırma 4.3: Örnek listesi oluşturma

Sözlükler listesinden Stock örneklerinin bir listesini
yapmak için bu adımları deneyin. Ardından toplam maliyeti hesaplayın:

```python
>>> import fileparse
>>> with open('Data/portfolio.csv') as lines:
...     portdicts = fileparse.parse_csv(lines, select=['name','shares','price'], types=[str,int,float])
...
>>> portfolio = [ stock.Stock(d['name'], d['shares'], d['price']) for d in portdicts]
>>> portfolio
[<stock.Stock object at 0x10c9e2128>, <stock.Stock object at 0x10c9e2048>, <stock.Stock object at 0x10c9e2080>,
 <stock.Stock object at 0x10c9e25f8>, <stock.Stock object at 0x10c9e2630>, <stock.Stock object at 0x10ca6f748>,
 <stock.Stock object at 0x10ca6f7b8>]
>>> sum([s.cost() for s in portfolio])
44671.15
>>>
```

### Alıştırma 4.4: Sınıfınızı kullanma

`read_portfolio()` fonksiyonunu `report.py` programından değiştirin,
böylece bir portföyü Alıştırma 4.3'te gösterildiği gibi `Stock` örnekleri 
listesine okur.  Bunu yaptıktan sonra, `report.py` ve `pcost.py`deki
tüm kodu, sözlükler yerine `Stock` örnekleriyle çalışacak şekilde düzeltin.

İpucu: Kodda büyük değişiklikler yapmanız gerekmemelidir.  Esas olarak 
`s['shares']` gibi sözlük erişimini `s.shares` olarak değiştireceksiniz.

Fonksiyonlarınızı eskisi gibi çalıştırabilmelisiniz:

```python
>>> import pcost
>>> pcost.portfolio_cost('Data/portfolio.csv')
44671.15
>>> import report
>>> report.portfolio_report('Data/portfolio.csv', 'Data/prices.csv')
      Name     Shares      Price     Change
---------- ---------- ---------- ----------
        AA        100       9.22     -22.98
       IBM         50     106.28      15.18
       CAT        150      35.46     -47.98
      MSFT        200      20.89     -30.34
        GE         95      13.48     -26.89
      MSFT         50      20.89     -44.21
       IBM        100     106.28      35.84
>>>
```

[Contents](../Contents.md) \| [Previous (3.6 Design discussion)](../03_Program_organization/06_Design_discussion.md) \| [Next (4.2 Inheritance)](02_Inheritance.md)
