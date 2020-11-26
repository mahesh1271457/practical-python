[Contents](../Contents.md) \| [Previous (4.4 Exceptions)](../04_Classes_objects/04_Defining_exceptions.md) \| [Next (5.2 Encapsulation)](02_Classes_encapsulation.md)

# 5.1 Yeniden Ziyaret Edilen Sözlükler

Python nesne sistemi, büyük ölçüde sözlükleri içeren bir uygulamaya dayanmaktadır. Bu bölüm bunu ele alıyor.

### Sözlükler, Terar Ziyaret Etme

Bir sözlüğün adlandırılmış değerlerden oluşan bir koleksiyon olduğunu unutmayın.

```python
stock = {
    'name' : 'GOOG',
    'shares' : 100,
    'price' : 490.1
}
```

Sözlükler genellikle basit veri yapıları için kullanılır. Ancak, yorumlayıcının kritik bölümleri için kullanılırlar ve *Python'daki en önemli veri türü* olabilirler.


### Sözlükler ve Modüller

Bir modül içinde, bir sözlük tüm genel değişkenleri ve fonksiyonları tutar.

```python
# foo.py

x = 42
def bar():
    ...

def spam():
    ...
```

`foo .__ dict__` veya `globals ()` 'i incelerseniz, sözlüğü görürsünüz.

```python
{
    'x' : 42,
    'bar' : <function bar>,
    'spam' : <function spam>
}
```


### Sözlükler ve Nesneler

Kullanıcı tanımlı nesneler ayrıca hem örnek verileri hem de sınıflar için sözlükler kullanır. Aslında, tüm nesne sistemi çoğunlukla sözlüklerin üstüne yerleştirilen ekstra bir katmandır.

Bir sözlük, `__dict__` nesne verilerini tutar.

```python
>>> s = Stock('GOOG', 100, 490.1)
>>> s.__dict__
{'name' : 'GOOG', 'shares' : 100, 'price': 490.1 }
```
`self` atarken bu sözlüğü (ve nesneyi) doldurursunuz.

```python
class Stock:
    def __init__(self, name, shares, price):
        self.name = name
        self.shares = shares
        self.price = price
```
`self.__dict__` nesne verisi şunun gibidir:

```python
{
    'name': 'GOOG',
    'shares': 100,
    'price': 490.1
}
```
**Her nesne kendi özel sözlüğüne sahip olur**

```python
s = Stock('GOOG', 100, 490.1)     # {'name' : 'GOOG','shares' : 100, 'price': 490.1 }
t = Stock('AAPL', 50, 123.45)     # {'name' : 'AAPL','shares' : 50, 'price': 123.45 }
```

Bir sınıfın 100  nesnesini oluşturduysanız, etrafta veri tutan 100 sözlük vardır.


### Sınıf Üyeleri

Ayrı bir sözlük de metodları içerir.

```python
class Stock:
    def __init__(self, name, shares, price):
        self.name = name
        self.shares = shares
        self.price = price

    def cost(self):
        return self.shares * self.price

    def sell(self, nshares):
        self.shares -= nshares
```

Sözlük `Stock.__dict__`içindedir.

```python
{
    'cost': <function>,
    'sell': <function>,
    '__init__': <function>
}
```

### Nesne ve Sınıflar

Nesneler ve sınıflar birbirlerine bağlıdır.

 `__class__` özelliği sınıfa geri döndürüyor.

```python
>>> s = Stock('GOOG', 100, 490.1)
>>> s.__dict__
{ 'name': 'GOOG', 'shares': 100, 'price': 490.1 }
>>> s.__class__
<class '__main__.Stock'>
>>>
```

Nesne sözlüğü her nesne için benzersiz verileri tutarken, sınıf sözlüğü *tüm* nesneler tarafından toplu olarak paylaşılan verileri tutar.

### Nitelik Erişimi

Nesnelerle çalıştığınız zaman, veri ve metodlara `.` operatörünü kullanarak erişirsiniz.

```python
x = obj.name          # Getting
obj.name = value      # Setting
del obj.name          # Deleting
```
Bu işlemler doğrudan kapakların altında oturan sözlüklere bağlıdır.

### Nesneleri Değiştirme

Bir nesneyi değiştiren işlemler, temeldeki sözlüğü günceller.

```python
>>> s = Stock('GOOG', 100, 490.1)
>>> s.__dict__
{ 'name':'GOOG', 'shares': 100, 'price': 490.1 }
>>> s.shares = 50       # Setting
>>> s.date = '6/7/2007' # Setting
>>> s.__dict__
{ 'name': 'GOOG', 'shares': 50, 'price': 490.1, 'date': '6/7/2007' }
>>> del s.shares        # Deleting
>>> s.__dict__
{ 'name': 'GOOG', 'price': 490.1, 'date': '6/7/2007' }
>>>
```

### Okuma Nitelikleri

Bir nesnedeki bir özniteliği okuduğunuzu varsayalım.

```python
x = obj.name
```
Bu nitelik iki yerde bulunabilir:

* Yerel nesne sözlüğü.
* Sınıf sözlüğü.

Her iki sözlük de kontrol edilmeli. Önce yerel __dict__ olarak kontrol edin. Eğer bulunmazsa __class__ üzerinden __dict__ sınıfına bakın.

```python
>>> s = Stock(...)
>>> s.name
'GOOG'
>>> s.cost()
49010.0
>>>
```
Bu arama şeması, bir *sınıfın* üyelerinin tüm nesneler tarafından nasıl paylaşıldığıdır.

### Miras alma nasıl çalışır?

Sınıflar diğer sınıflardan miras alabilir.

```python
class A(B, C):
    ...
```

Temel sınıflar, her sınıfta bir tuple içinde saklanır.

```python
>>> A.__bases__
(<class '__main__.B'>, <class '__main__.C'>)
>>>
```
Bu, üst sınıflara bir bağlantı sağlar.

### Miras Alma ile Nitelikleri Okuma

Mantıksal olarak, bir nitelik bulma süreci aşağıdaki gibidir. İlk olarak yerel `__dict__` olarak kontrol edin. Eğer bulunmazsa `__class__` üzerinden `__dict__` sınıfına bakın. Sınıfta bulunmazsa, `__bases__` üzerinde temel sınıflara bakın. Ancak, bunun aşağıda tartışılan bazı ince yönleri vardır.

### Tekli Miras Alma ile Nitelikleri Okuma

Miras alma hiyerarşisinde, nitelikler miras ağacında sırayla gezinerek bulunur.

```python
class A: pass
class B(A): pass
class C(A): pass
class D(B): pass
class E(D): pass
```
Tek kalıtımla, tepeye giden tek yol vardır. İlk eşleşmede durur.

### Method Resolution Order (Method Çözüm Sırası) veya MRO 

Python, bir miras alma zincirini önceden hesaplar ve bunu sınıftaki *MRO* özniteliğinde saklar. Görüntüleyebilirsiniz.

```python
>>> E.__mro__
(<class '__main__.E'>, <class '__main__.D'>,
 <class '__main__.B'>, <class '__main__.A'>,
 <type 'object'>)
>>>
``` 
Bu zincire **Method Resolution Order (Metod Çözüm Sırası)** denir. Python, bir nitelik bulmak için MRO'yu sırayla gezer. İlk eşleşme kazanır.

### MRO'da Çoklu Miras Alma

Çoklu miras alma ile, tepeye giden tek yol yoktur. Hadi bir örneğe bakalım.

```python
class A: pass
class B: pass
class C(A, B): pass
class D(B): pass
class E(C, D): pass
```
Bir niteliğe eşitiğinizde ne olur?

```python
e = E()
e.attr
```
Bir nitelik arama işlemi gerçekleştirilir, ancak sıra nedir? Bu bir sorun.

Python, sınıf sıralamasıyla ilgili bazı kurallara uyan *işbirliğine dayalı çoklu miras* kullanır.

* Çocuklar her zaman ebeveynlerinden sonra kontrol edilir.
* Ebeveynler (birden fazla ise) her zaman listelenen sırada kontrol edilir.

MRO, bir hiyerarşideki tüm sınıfların bu kurallara göre sıralanmasıyla hesaplanır.

```python
>>> E.__mro__
(
  <class 'E'>,
  <class 'C'>,
  <class 'A'>,
  <class 'D'>,
  <class 'B'>,
  <class 'object'>)
>>>
```
Temel algoritmaya "C3 Doğrusallaştırma Algoritması" denir. Bir sınıf hiyerarşisinin, eviniz yanıyorsa ve tahliye etmeniz gerekiyorsa - önce çocuklar, ardından ebeveynler - uymanız gereken kurallara uyduğunu hatırladığınız sürece, kesin ayrıntılar önemli değildir.

### Tek Kodun Yeniden Kullanımı (Çoklu Miras İçeren)

Tamamen alakasız iki nesneyi düşünün:

```python
class Dog:
    def noise(self):
        return 'Bark'

    def chase(self):
        return 'Chasing!'

class LoudDog(Dog):
    def noise(self):
        # Code commonality with LoudBike (below)
        return super().noise().upper()
```

ve

```python
class Bike:
    def noise(self):
        return 'On Your Left'

    def pedal(self):
        return 'Pedaling!'

class LoudBike(Bike):
    def noise(self):
        # Code commonality with LoudDog (above)
        return super().noise().upper()
```
`LoudDog.noise()` ve `LoudBike.noise()` uygulamasında bir ortak kod vardır. Aslında kod tamamen aynıdır. Doğal olarak, böyle bir kod yazılım mühendislerini cezbetmek zorundadır.

### "Mixin" Modeli

*Mixin* kalıbı, bir kod parçası içeren bir sınıftır.

```python
class Loud:
    def noise(self):
        return super().noise().upper()
```

Bu sınıf tek başına kullanılamaz.
Miras yoluyla diğer sınıflarla karışır.

```python
class LoudDog(Loud, Dog):
    pass

class LoudBike(Loud, Bike):
    pass
```
Mucizevi bir şekilde, loudness şimdi yalnızca bir kez uygulandı ve tamamen ilgisiz iki sınıfta yeniden kullanıldı. Bu tür bir numara, Python'daki çoklu mirasın birincil kullanımlarından biridir.

### Neden `super()`

Metotları override yaparken her zaman `super()` kullanırız.

```python
class Loud:
    def noise(self):
        return super().noise().upper()
```

MRO'daki *sonraki sınıfı* `super()` temsil eder. 

İşin zor yanı, onun ne olduğunu bilmemenizdir. Özellikle çoklu miras kullanılıyorsa bunun ne olduğunu bilmiyorsunuz.

### Bazı Dikkat Edilecek Noktalar

Çoklu miras, güçlü bir araçtır. Unutmayın ki sorumluluk güçle gelir. Framework'ler / kitaplıklar bazen bunu bileşenlerin bileşimini içeren gelişmiş özellikler için kullanır. Şimdi, bunu gördüğünü unut.

## Egzersizler

Bölüm 4'te, bir hisse senedi holdingini temsil eden bir `Stock` sınıfı tanımladınız. Bu egzersizde bu sınıfı kullanacağız. Yorumlayıcıyı yeniden başlatın ve birkaç örnek yapın:

```python
>>> ================================ RESTART ================================
>>> from stock import Stock
>>> goog = Stock('GOOG',100,490.10)
>>> ibm  = Stock('IBM',50, 91.23)
>>>
```

### Egzersiz 5.1:  Nesnelerin Temsili

Etkileşimli shell'de, oluşturduğunuz iki nesnenin temelindeki sözlükleri inceleyin:

```python
>>> goog.__dict__
... look at the output ...
>>> ibm.__dict__
... look at the output ...
>>>
```

### Egzersiz 5.2: Nesne Verisinin Değişimi

Yukarıdaki nesnelerden birinde yeni bir nitelik ayarlamayı deneyin:

```python
>>> goog.date = '6/11/2007'
>>> goog.__dict__
... look at output ...
>>> ibm.__dict__
... look at output ...
>>>
```
Yukarıdaki çıktıda, `goog` nesnesinin `date` niteliğine sahip olduğunu, ancak `ibm` nesnesinin olmadığını farkedeceksiniz. Python'un niteliklere gerçekten herhangi bir kısıtlama getirmediğini unutmamak önemlidir. Mesela, bir nesnenin nitelikleri, `__init__()`  yönteminde ayarlananlarla sınırlı değildir.

Bir nitelik ayarlamak yerine, doğrudan `__dict__` nesnesine yeni bir değer yerleştirmeyi deneyin:

```python
>>> goog.__dict__['time'] = '9:45am'
>>> goog.time
'9:45am'
>>>
```
Burada, bir örneğin bir sözlüğün üstündeki bir katman olduğunu gerçekten fark ediyorsunuz. Not: Sözlüğün doğrudan manipülasyonunun nadir olduğu vurgulanmalıdır - (.) Sözdizimini kullanmak için her zaman kodunuzu yazmalısınız.

### Egzersiz 5.3: Sınıfların Rolü

Bir sınıf tanımını oluşturan tanımlar, o sınıfın tüm nesneleri tarafından paylaşılır. Tüm nesnelerin ilişkili sınıflarına bir geri bağlantısı olduğuna dikkat edin:

```python
>>> goog.__class__
... look at output ...
>>> ibm.__class__
... look at output ...
>>>
```
Nesnelerden bir metod çağırmayı deneyin:

```python
>>> goog.cost()
49010.0
>>> ibm.cost()
4561.5
>>>
```

'cost' adının `goog.__dict__` veya `ibm.__dict__` içinde tanımlanmadığına dikkat edin. Bunun yerine, sınıf sözlüğü tarafından sağlanıyor. Bunu deneyin:

```python
>>> Stock.__dict__['cost']
... look at output ...
>>>
```
Doğrudan sözlük aracılığıyla `cost()` metodunu çağırmayı deneyin:

```python
>>> Stock.__dict__['cost'](goog)
49010.0
>>> Stock.__dict__['cost'](ibm)
4561.5
>>>
```
Sınıf tanımında tanımlanan fonksiyonu nasıl çağırdığınıza ve `self` argümanının nesneyi nasıl aldığına dikkat edin.

`Stock` sınıfına yeni özellikler eklemeyi deneyin:

```python
>>> Stock.foo = 42
>>>
```

Bu yeni niteliği artık tüm nesnelerde nasıl göründüğüne dikkat edin:

```python
>>> goog.foo
42
>>> ibm.foo
42
>>>
```

Ancak, nesne sözlüğünün bir parçası olmadığına dikkat edin:

```python
>>> goog.__dict__
... look at output and notice there is no 'foo' attribute ...
>>>
```
Nesnelerde "foo" niteliğine erişebilmenizin nedeni, nesnein kendisinde bir şey bulamazsa Python'un her zaman sınıf sözlüğünü kontrol etmesidir.

Not: Alıştırmanın bu kısmı, sınıf değişkeni olarak bilinen bir şeyi göstermektedir. Örneğin şöyle bir sınıfınız olduğunu varsayalım:

```python
class Foo(object):
     a = 13                  # Class variable
     def __init__(self,b):
         self.b = b          # Instance variable
```

Bu sınıfta, sınıfın gövdesinde atanan `a` değişkeni bir "sınıf değişkeni" dir. Oluşturulan tüm nesneler tarafından paylaşılır. Örneğin:

```python
>>> f = Foo(10)
>>> g = Foo(20)
>>> f.a          # Inspect the class variable (same for both instances)
13
>>> g.a
13
>>> f.b          # Inspect the instance variable (differs)
10
>>> g.b
20
>>> Foo.a = 42   # Change the value of the class variable
>>> f.a
42
>>> g.a
42
>>>
```

### Egzersiz 5.4: Bağlı Metodlar

Python'un ince bir özelliği, bir metodu çağırmanın aslında iki adımı ve bağlı metod olarak bilinen bir şeyi içermesidir. Örneğin:

```python
>>> s = goog.sell
>>> s
<bound method Stock.sell of Stock('GOOG', 100, 490.1)>
>>> s(25)
>>> goog.shares
75
>>>
```
Bağlı metodlar aslında bir metodu çağırmak için gereken tüm parçaları içerir. Örneğin, metodu uygulayan fonksiyonun bir kaydını tutarlar:

```python
>>> s.__func__
<function sell at 0x10049af50>
>>>
```
Bu, `Stock` sözlüğünde bulunan değerin aynısıdır.

```python
>>> Stock.__dict__['sell']
<function sell at 0x10049af50>
>>>
```
Bağlı metodlar, `self`argümanı olan nesneyi de kaydeder.

```python
>>> s.__self__
Stock('GOOG',75,490.1)
>>>
```

`()` fonksiyonu kullanarak çağırdığınızda tüm parçalar bir araya gelir. Örneğin, `s(25)` çağrısı aslında şunu yapar:

```python
>>> s.__func__(s.__self__, 25)    # Same as s(25)
>>> goog.shares
50
>>>
```

### Egzersiz 5.5: Miras Alma

`Stock`'dan miras alan yeni bir sınıf oluşturun.

```
>>> class NewStock(Stock):
        def yow(self):
            print('Yow!')

>>> n = NewStock('ACME', 50, 123.45)
>>> n.cost()
6172.50
>>> n.yow()
Yow!
>>>
```
Miras alma, nitelikler için arama sürecini genişleterek uygulanır. `__bases__` niteliği, yakın üst öğelerin bir tuple'ına sahiptir:

```python
>>> NewStock.__bases__
(<class 'stock.Stock'>,)
>>>
```

`__mro__` niteliği, nitelikler için aranacakları sırayla tüm üst öğelerin bir tuple'ına sahiptir.

```python
>>> NewStock.__mro__
(<class '__main__.NewStock'>, <class 'stock.Stock'>, <class 'object'>)
>>>
```

Yukarıdaki `n` nesnesinin `cost()` metodu şu şekilde bulunur:

```python
>>> for cls in n.__class__.__mro__:
        if 'cost' in cls.__dict__:
            break

>>> cls
<class '__main__.Stock'>
>>> cls.__dict__['cost']
<function cost at 0x101aed598>
>>>
```

[Contents](../Contents.md) \| [Previous (4.4 Exceptions)](../04_Classes_objects/04_Defining_exceptions.md) \| [Next (5.2 Encapsulation)](02_Classes_encapsulation.md)
