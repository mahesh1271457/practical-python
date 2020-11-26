[Contents](../Contents.md) \| [Previous (5.1 Dictionaries Revisited)](01_Dicts_revisited.md) \| [Next (6 Generators)](../06_Generators/00_Overview.md)

# 5.2 Sınıflar ve Kapsülleme

Sınıfları yazarken, iç detayları denemek ve kapsüllemek yaygındır. 
Bu bölüm, özel değişkenler ve özellikler de dahil olmak üzere bunun 
için birkaç Python programlama deyimini tanıtır.

### Private vs Public.

Bir sınıfın birincil rollerinden biri, bir nesnenin verilerini ve dahili 
uygulama ayrıntılarını kapsüllemektir. Bununla birlikte, bir sınıf aynı zamanda dış
dünyanın nesneyi işlemek için kullanması gereken bir *public* arabirimi de tanımlar. 
Uygulama ayrıntıları ile genel arayüz arasındaki bu ayrım önemlidir.

### Bir Problem

Python'da, sınıflar ve nesneler hakkında neredeyse her şey *açıktır*.

* Nesne iç kısımlarını kolayca inceleyebilirsiniz.
* Bir şeyleri istediğiniz zaman değiştirebilirsiniz.
* Güçlü bir erişim kontrolü kavramı yoktur (yani, private sınıf üyeleri).

*İç uygulamanın* ayrıntılarını izole etmeye çalışırken bu bir sorundur.

### Python Kapsamlama

Python, bir şeyin amaçlanan kullanımını belirtmek için programlama kurallarına güvenir. 
Bu kurallar isimlendirmeye dayanmaktadır. Kurallara uymanın dil tarafından uygulanmasına
karşın programcıya bağlı olduğuna dair genel bir tutum vardır.

### Private Özellikler

Başında `_` bulunan herhangi bir özellik adı *private* olarak kabul edilir.

```python
class Person(object):
    def __init__(self, name):
        self._name = 0
```

Daha önce de belirtildiği gibi, bu yalnızca bir programlama stilidir. 
Yine de erişebilir ve değiştirebilirsiniz.

```python
>>> p = Person('Guido')
>>> p._name
'Guido'
>>> p._name = 'Dave'
>>>
```
Genel bir kural olarak, başında `_` bulunan herhangi bir ad, ister değişken, 
ister fonksiyon, ister metod adı olsun, dahili uygulama olarak kabul edilir. 
Kendinizi bu tür isimleri doğrudan kullanırken bulursanız, muhtemelen yanlış bir şeyler yapıyorsunuzdur.
Daha yüksek düzeyde fonksiyonellik arayın.

### Tekli Özellikler

Aşağıdaki sınıfı düşünün.

```python
class Stock:
    def __init__(self, name, shares, price):
        self.name = name
        self.shares = shares
        self.price = price
```
Şaşırtıcı bir özellik, nitelikleri herhangi bir değere ayarlayabilmenizdir:

```python
>>> s = Stock('IBM', 50, 91.1)
>>> s.shares = 100
>>> s.shares = "hundred"
>>> s.shares = [1, 0, 0]
>>>
```
Şuna bakabilir ve fazladan kontroller istediğinizi düşünebilirsiniz.

```python
s.shares = '50'     # Raise a TypeError, this is a string
```
Nasıl yapardınız?

### Yönetilen Özellikler

Bir yaklaşım: erişimci metodlarını tanıtmak.

```python
class Stock:
    def __init__(self, name, shares, price):
        self.name = name
	self.set_shares(shares)
	self.price = price

    # Function that layers the "get" operation
    def get_shares(self):
        return self._shares

    # Function that layers the "set" operation
    def set_shares(self, value):
        if not isinstance(value, int):
            raise TypeError('Expected an int')
        self._shares = value
```

Bunun mevcut tüm kodumuzu bozması çok kötü. `s.shares = 50`,` s.set_shares (50) `olur.

### Properties

Önceki modele alternatif bir yaklaşım var.

```python
class Stock:
    def __init__(self, name, shares, price):
        self.name = name
        self.shares = shares
        self.price = price

    @property
    def shares(self):
        return self._shares

    @shares.setter
    def shares(self, value):
        if not isinstance(value, int):
            raise TypeError('Expected int')
        self._shares = value
```
Normal nitelik erişimi artık `@property` ve `@shares.setter` altındaki getter ve setter metodlarını tetikliyor.

```python
>>> s = Stock('IBM', 50, 91.1)
>>> s.shares         # Triggers @property
50
>>> s.shares = 75    # Triggers @shares.setter
>>>
```
Bu modelle, kaynak kodunda *değişiklik yapılması* gerekmez. Yeni *setter*, sınıf içinde, `__init__()` metodu dahil olmak üzere bir atama olduğunda da çağrılır.

```python
class Stock:
    def __init__(self, name, shares, price):
        ...
        # This assignment calls the setter below
        self.shares = shares
        ...

    ...
    @shares.setter
    def shares(self, value):
        if not isinstance(value, int):
            raise TypeError('Expected int')
        self._shares = value
```
Genellikle bir property ile özel isim kullanımı arasında bir karışıklık vardır. 
Bir property dahili olarak `_shares` gibi özel bir ad kullansa da, 
sınıfın geri kalanı (property değil) `shares` gibi bir ad kullanmaya devam edebilir.

Property'ler ayrıca hesaplanan veri nitelikleri için de kullanışlıdır.

```python
class Stock:
    def __init__(self, name, shares, price):
        self.name = name
        self.shares = shares
        self.price = price

    @property
    def cost(self):
        return self.shares * self.price
    ...
```
Bu, aslında bir metod olduğu gerçeğini gizleyerek fazladan parantezleri kaldırmanıza olanak tanır:

```python
>>> s = Stock('GOOG', 100, 490.1)
>>> s.shares # Instance variable
100
>>> s.cost   # Computed Value
49010.0
>>>
```
### Tek Tip Erişim

Son örnek, bir nesneye nasıl daha düzgün bir arayüzün yerleştirileceğini gösterir. 
Bunu yapmazsanız, bir nesnenin kullanımı kafa karıştırıcı olabilir:

```python
>>> s = Stock('GOOG', 100, 490.1)
>>> a = s.cost() # Method
49010.0
>>> b = s.shares # Data attribute
100
>>>
```
Neden property için `()` gerekli ama shares için gerekmiyor? Bir property bunu düzeltebilir.

### Dekoratör Söz Dizimi

`@`  Sözdizimi "dekorasyon" olarak bilinir. Hemen ardından gelen fonksiyon tanımına uygulanan bir değiştiriciyi belirtir.

```python
...
@property
def cost(self):
    return self.shares * self.price
```
[Section 7]'de daha fazla ayrıntı verilmiştir. (../07_Advanced_Topics/00_Overview).

### `__slots__` Niteliği

Nitelik adlarını kısıtlayabilirsiniz.

```python
class Stock:
    __slots__ = ('name','_shares','price')
    def __init__(self, name, shares, price):
        self.name = name
        ...
```

Diğer nitelikler için bir hata oluşturacaktır.

```python
>>> s.price = 385.15
>>> s.prices = 410.2
Traceback (most recent call last):
File "<stdin>", line 1, in ?
AttributeError: 'Stock' object has no attribute 'prices'
```

Bu, hataları önlese ve nesnelerin kullanımını kısıtlasa da, 
aslında performans için kullanılır ve Python'un belleği daha verimli kullanmasını sağlar.

### Kapsülleme ile İlgili Son Yorumlar

Nitelikler, property'ler, slot'lar vb. ile denize girmeyin.
Bunlar belirli bir amaca hizmet eder ve bunları diğer Python kodunu okurken görebilirsiniz. 
Ancak, çoğu günlük kodlama için gerekli değildir.

## Egzersizler

###  Egzersiz 5.6: Tekli Property

Property'ler bir nesneye "hesaplanan nitelikler" eklemenin kullanışlı bir yoludur. 
`stock.py` de `Stock`" nesnesi oluşturdunuz. Nesnenizde, farklı veri türlerinin 
çıkarılma şekliyle ilgili küçük bir tutarsızlık olduğuna dikkat edin:

```python
>>> from stock import Stock
>>> s = Stock('GOOG', 100, 490.1)
>>> s.shares
100
>>> s.price
490.1
>>> s.cost()
49010.0
>>>
```
Özellikle, ekstra () 'i `cost` e nasıl eklemeniz gerektiğine dikkat edin çünkü bu bir yöntemdir.

Bir mülke dönüştürürseniz, `cost()` üzerindeki ekstra () 'dan kurtulabilirsiniz. 
`Stock` sınıfınızı alın ve maliyet hesaplamasının şu şekilde çalışması için değiştirin:

```python
>>> ================================ RESTART ================================
>>> from stock import Stock
>>> s = Stock('GOOG', 100, 490.1)
>>> s.cost
49010.0
>>>
```
`s.cost()` u bir fonksiyon olarak çağırmayı deneyin ve `cost` bir property olarak tanımlandığından artık çalışmadığını gözlemleyin.

```python
>>> s.cost()
... fails ...
>>>
```
Bu değişikliği yapmak büyük olasılıkla önceki `pcost.py` programınızı bozacaktır. 
Geri dönüp `cost()` yönteminde `()` işaretinden kurtulmanız gerekebilir.

### Egzersiz 5.7: Property ve Setter

`shares` niteliğini, değer özel bir nitelikte saklanacak ve her zaman bir tamsayı 
değerine ayarlanmasını sağlamak için bir çift property fonksiyonu kullanılacak şekilde değiştirin. 
İşte beklenen davranışın bir örneği:

```python
>>> ================================ RESTART ================================
>>> from stock import Stock
>>> s = Stock('GOOG',100,490.10)
>>> s.shares = 50
>>> s.shares = 'a lot'
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: expected an integer
>>>
```
### Egzersiz 5.8: Slot Eklemek

`Stock` sınıfını `__slots__`  özelliğine sahip olacak şekilde değiştirin. Ardından, yeni özelliklerin eklenemediğini doğrulayın:

```python
>>> ================================ RESTART ================================
>>> from stock import Stock
>>> s = Stock('GOOG', 100, 490.10)
>>> s.name
'GOOG'
>>> s.blah = 42
... see what happens ...
>>>
```

`__slots__` kullandığınızda, Python nesnelerin daha verimli bir iç temsilini kullanır. Yukarıdaki `s` nin altında yatan sözlüğü incelemeye çalışırsanız ne olur?


```python
>>> s.__dict__
... see what happens ...
>>>
```

`__slots__` un en çok veri yapıları olarak hizmet eden sınıflar üzerinde bir optimizasyon olarak kullanıldığına dikkat edilmelidir. Slot'ları kullanmak, bu tür programların çok daha az bellek kullanmasına ve biraz daha hızlı çalışmasına neden olur. Bununla birlikte, muhtemelen diğer sınıfların çoğunda  `__slots__` dan kaçınmalısınız.

[Contents](../Contents.md) \| [Previous (5.1 Dictionaries Revisited)](01_Dicts_revisited.md) \| [Next (6 Generators)](../06_Generators/00_Overview.md)

