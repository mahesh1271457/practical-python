[Contents](../Contents.md) \| [Previous (7.2 Anonymous Functions)](02_Anonymous_function.md) \| [Next (7.4 Decorators)](04_Function_decorators.md)

# 7.3 Fonksiyonlarda Return

Bu bölüm, fonksiyon kullanarak başka fonksiyonlar oluşturma kavramına giriş yapmaktadır.


### Giriş

Aşağıdaki fonksiyonu inceleyin.

```python
def ekle(x, y):
    def topla():
        print('Toplanıyor', x, y)
        return x + y
    return topla
```

Bu fonksiyon; başka bir fonksiyonu return eden, yani döndürerek o fonksiyonun sonucunu çıkaran bir fonksiyondur.

```python
>>> a = ekle(3,4)
>>> a
<function topla at 0x6a670>
>>> a()
Toplanıyor 3 4
7
```

### Yerel Değişkenler

İç kısımdaki fonksiyonun, dış kısımdaki fonksiyon tarafından tanımlanmış değişkenleri nasıl kullandığını inceleyin.

```python
def ekle(x, y):
    def topla():
        # `x` ve `y` değişkenleri, `ekle(x, y)` kısmında tanımlanmıştı.
        print('Toplanıyor', x, y)
        return x + y
    return topla
```

Bu değişkenler, `topla()` fonksiyonu bittikten sonra da tutulur.

```python
>>> a = ekle(3,4)
>>> a
<function topla at 0x6a670>
>>> a()
Adding 3 4      # Bu değerler nereden geliyor?
7
```

### Closure Kavramı

İç kısımdaki fonksiyon bir sonuç olarak döndürüldüğünde, bu iç fonksiyon bir *closure* olarak, yani bir *kapatma* olarak tanımlanır.

```python
def ekle(x, y):
    # Burada `topla` bir closure, yani bir kapatmadır.
    def topla():
        print('Toplanıyor', x, y)
        return x + y
    return topla
```

*Önemli not: Bir kapatma, fonksiyonun daha sonra da sorunsuzca çalışabilmesi için gereken tüm değişken değerlerini içinde tutar.*   Bir kapatma’yı, kendisi için gerekli değişken değerlerini barındıran bir fonksiyon olarak da düşünebiliriz.

### Kapatmaları Kullanma

Kapatmalar Python için önemli bir özelliktir ve genellikle şeffaf bir şekilde kullanılırlar.
Genel kullanımlar:

* Callback (Geri çağırma) fonksiyonları
* Ertelenmiş yürütmeler.
* Dekoratör fonksiyonlar (daha sonra).

### Ertelenmiş yürütmeler

Aşağıdaki fonksiyonu inceleyiniz.

```python
def sonra(saniye, func):
    time.sleep(saniye)
    func()
```

Örnek kullanım:

```python
def selamla():
    print('Merhaba Dünya')

sonra(30, selamla)
```

Bu örnekte `sonra` fonksiyonu, içinde verilen fonksiyonu daha sonra çalıştırmaktadır, tıpkı isminin de belirttiği gibi.

Kapatmalar ek bilgiler taşır.

```python
def ekle(x, y):
    def topla():
        print(f'Toplanıyor {x} + {y} -> {x+y}')
    return topla

def sonra(saniye, func):
    time.sleep(saniye)
    func()

sonra(30, topla(2, 3))
# `topla` fonksiyonu, x -> 2 ve y -> 3 referanslarına sahip.
```

### Kod Tekrarı

Kapatmalar, kod tekrarlarının önüne geçmek için bir teknik olarak da kullanılabilir.
Kod yazan fonksiyonlar yaratabilirsiniz.

## Alıştırmalar

### Alıştırma 7.7: Kod Tekrarını Önlemek İçin Kapatma Kullanma

Kapatmaların önemli özelliklerinden biri, tekrarlayan kod üretmedeki kullanımlarıdır. Tür denetimli özellik tanımlama için [Alıştırma
5.7](../05_Object_model/02_Classes_encapsulation) alıştırmasına göz atabilirsiniz.

```python
class Stock:
    def __init__(self, name, shares, price):
        self.name = name
        self.shares = shares
        self.price = price
    ...
    @property
    def shares(self):
        return self._shares

    @shares.setter
    def shares(self, value):
        if not isinstance(value, int):
            raise TypeError('Expected int')
        self._shares = value
    ...
```

Aynı kodu tekrar tekrar yazmak yerine, kapatma kullanarak bu kodları otomatik olarak yaratabilirsiniz.

`typedproperty.py` isimli bir dosya oluşturup aşağıdaki kodu dosya içinde kullanın:

```python
# typedproperty.py

def typedproperty(name, expected_type):
    private_name = '_' + name
    @property
    def prop(self):
        return getattr(self, private_name)

    @prop.setter
    def prop(self, value):
        if not isinstance(value, expected_type):
            raise TypeError(f'Expected {expected_type}')
        setattr(self, private_name, value)

    return prop
```

Aşağıdaki gibi bir kod oluştururak bunu deneyin:

```python
from typedproperty import typedproperty

class Stock:
    name = typedproperty('name', str)
    shares = typedproperty('shares', int)
    price = typedproperty('price', float)

    def __init__(self, name, shares, price):
        self.name = name
        self.shares = shares
        self.price = price
```

Bir instance (örnek) oluşturmayı deneyin ve tür denetiminin çalıştığından emin olun.


```python
>>> s = Stock('IBM', 50, 91.1)
>>> s.name
'IBM'
>>> s.shares = '100'
... TypeError ...
>>>
```

### Alıştırma 7.8: Fonksiyon Çağırmayı Kolaylaştırma

Yukarıdaki örnekte kullanıcılar, `typedproperty('shares', int)` gibi kısımları fazla ayrıntılı bulabilirler--özellikle birçok kez tekrar edilmişse. Lütfen aşağıdaki tanımlamaları `typedproperty.py` dosyasına ekleyin:

```python
String = lambda name: typedproperty(name, str)
Integer = lambda name: typedproperty(name, int)
Float = lambda name: typedproperty(name, float)
```

Şimdi aşağıdaki fonksiyonları kullanarak `Stock` class’ını yeniden yazın.

```python
class Stock:
    name = String('name')
    shares = Integer('shares')
    price = Float('price')

    def __init__(self, name, shares, price):
        self.name = name
        self.shares = shares
        self.price = price
```

Bu biraz daha iyi oldu. Buradan çıkarılacak ana sonuç; kapatmalar ve `lambda` kullanımı kodu sadeleştirir ve kod tekrarı ihtimalini ortadan kaldırır, ki bu genellikle iyi bir şeydir.

### Alıştırma 7.9: İşi Pratiğe Dökmek

`stock.py` dosyasındaki `Stock` class’ını, önceden bahsedilmiş özellikleri gösterilen şekilde kullanacak biçimde yeniden yazın.

[Contents](../Contents.md) \| [Previous (7.2 Anonymous Functions)](02_Anonymous_function.md) \| [Next (7.4 Decorators)](04_Function_decorators.md)

