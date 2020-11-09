# 4.3 Özel Yöntemler

Python'ın davranışlarının çeşitli bölümleri özel veya sözde "sihirli" denilebilecek yöntemlerle özelleştirilebilir. 
Bu bölümde bu fikir tanıtılır. Ayrıca dinamik özellik erişimi ve bağlı yöntemler tartışılır.


### Giriş

Sınıflar özel yöntemler tanımlayabilir. Bu özel yöntemlerin Python yorumlayıcısı için özel anlamı vardır.
Bunlar her zaman önce gelir ve devamlarında `__` olur. Örneğin `__init__`.

```python
class Stock(object):
    def __init__(self):
        ...
    def __repr__(self):
        ...
```
Düzinelerce özel yöntem var, ama biz sadece birkaç özel örneğe bakacağız.

### Karakter Dizileri (String) Dönüşümleri İçin Özel Yöntemler

Nesnelerin iki karakter dizisi(string) gösterimi vardır. 


```python
>>> from datetime import date
>>> d = date(2012, 12, 21)
>>> print(d)
2012-12-21
>>> d
datetime.date(2012, 12, 21)
>>>
```

Düzgün bir çıktı için 'str()' fonksiyonu kullanılır.

```python
>>> str(d)
'2012-12-21'
>>>
```

Programcılar daha detaylı bir gösterim oluşturmak için `repr()` fonksiyonu kullanılır.

```python
>>> repr(d)
'datetime.date(2012, 12, 21)'
>>>
```

Bu fonksiyonlar, `str()` ve `repr()`, çıktısı alıncak string'i oluşturmak için sınıfın içinde bir çift özel metot
kullanırlar.


```python
class Date(object):
    def __init__(self, year, month, day):
        self.year = year
        self.month = month
        self.day = day

    # Used with `str()`
    def __str__(self):
        return f'{self.year}-{self.month}-{self.day}'

    # Used with `repr()`
    def __repr__(self):
        return f'Date({self.year},{self.month},{self.day})'
```

*Not:Burdaki uyum, `__repr__()` fonksiyonunun döndürdüğü string'i, `eval()` fonksiyonuna verdiğimizde, temel nesneyi tekrar
oluşturmasıdır. Eğer bu mümkün değilse,bu nesne yerine kolay okunabilecek bir temsil kullanılır.*

### Matematik İçin Özel Yöntemler
Matematiksel operatörler aşağıdaki metotlar için çağrı içerir.

```python
a + b       a.__add__(b)
a - b       a.__sub__(b)
a * b       a.__mul__(b)
a / b       a.__truediv__(b)
a // b      a.__floordiv__(b)
a % b       a.__mod__(b)
a << b      a.__lshift__(b)
a >> b      a.__rshift__(b)
a & b       a.__and__(b)
a | b       a.__or__(b)
a ^ b       a.__xor__(b)
a ** b      a.__pow__(b)
-a          a.__neg__()
~a          a.__invert__()
abs(a)      a.__abs__()
```

### Öğelere Erişmek İçin Özel Yöntemler
Bu özel metotlarla container'lar oluşturulur.

```python
len(x)      x.__len__()
x[a]        x.__getitem__(a)
x[a] = v    x.__setitem__(a,v)
del x[a]    x.__delitem__(a)
```

Bunları sınıflarınızda kullanabilirsiniz.


```python
class Sequence:
    def __len__(self):
        ...
    def __getitem__(self,a):
        ...
    def __setitem__(self,a,v):
        ...
    def __delitem__(self,a):
        ...
```

### Yöntem Çağırma
Bir yöntemi çağırmak iki adımlı bir işlemdir.
1. Arama: `.` operatörü
2. Method çağırma: `()` operatörü

```python
>>> s = Stock('GOOG',100,490.10)
>>> c = s.cost  # Lookup
>>> c
<bound method Stock.cost of <Stock object at 0x590d0>>
>>> c()         # Method call
49010.0
>>>
```

### Bağlı Yöntemler
Fonksiyon çağrısı operatörü `()` taradından çağrılmamış yöntemlere *bağlı yöntem* denir.
Kullanıldığı örnek üzerinde çalışır.


```python
>>> s = Stock('GOOG', 100, 490.10) >>> s
<Stock object at 0x590d0>
>>> c = s.cost
>>> c
<bound method Stock.cost of <Stock object at 0x590d0>>
>>> c()
49010.0
>>>
```

Bağlı metotlar genellikle dikkatsizce yapılmış, belli olmayan hataların kaynaklarıdır.
Örneğin:

```python
>>> s = Stock('GOOG', 100, 490.10)
>>> print('Cost : %0.2f' % s.cost)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: float argument required
>>>
```
Ya da hata ayıklaması zor olan aldatıcı davranışların kaynaklarıdır.

```python
f = open(filename, 'w')
...
f.close     # Oops, Didn't do anything at all. `f` still open.
```

Bu iki örnekte de, hatanın sebebi sondaki parantezi yazmayı unutmaktır. Örneğin:
`s.cost()` veya `f.close()`.

### Özellik Erişimi
Özelliklere erişmek, işlemek ve yönetmek için alternatif bir yol vardır.
 
```python
getattr(obj, 'name')          # obj.name ile aynı
setattr(obj, 'name', value)   # obj.name = value ile aynı
delattr(obj, 'name')          # del obj.name ile aynı
hasattr(obj, 'name')          # özellik var mı diye bakar
```

Örnek:
```python
if hasattr(obj, 'x'):
    x = getattr(obj, 'x'):
else:
    x = None
```
*Not: Ayrıca `getattr()` yararlı bir varsayılan değere sahiptir *arg*.

```python
x = getattr(obj, 'x', None)
```

## Egzersizler
### Egzersiz 4.9: Nesneler için daha iyi çıktı
`stock.py`da tanımladığınız `Stock` nesnesini `__repr__()` metoduyla daha kullanışlı
bir çıktı vermesi için modifiye edin. Örneğin:


```python
>>> goog = Stock('GOOG', 100, 490.1)
>>> goog
Stock('GOOG', 100, 490.1)
>>>
```
Bu değişiklikleri yaptıktan sonra hisse senetleri portföyünü okuyun ve sonrasında oluşan 
listeye bakın. Örneğin:

```
>>> import report
>>> portfolio = report.read_portfolio('Data/portfolio.csv')
>>> portfolio
... see what the output is ...
>>>
```
### Egzersiz 4.10 getattr() kullanımında örnek

`getattr()`özellikler okumak için alernatif bir yöntemdir. Son derece esnek kodlar yazmak için 
kullanılabilir. Başlangıç olarak bu örneği deneyin:

 ```python
>>> import stock
>>> s = stock.Stock('GOOG', 100, 490.1)
>>> columns = ['name', 'shares']
>>> for colname in columns:
        print(colname, '=', getattr(s, colname))

name = GOOG
shares = 100
>>>
```

Çıktı datasının, `columns` değişkeninde listelenen özellik isimleri tarafından belirlediğini
dikkatle gözlemleyin.

`tableformat.py` dosyasındaki fikri alın, ve işlevini kullanıcı tarafından
belirtilen özelliklerde olan, rastgele nesnelerin listesini çıktı alıcak `print_table()` fonksiyonuna 
genişletin. Daha önceki `print_report()` fonksiyonu gibi, `print_table()` fonksiyonu da çıktı
biçimini denetlemek için `TableFormatter`kullanmalıdır. Bu şekilde çalışmalıdır:

```python
>>> import report
>>> portfolio = report.read_portfolio('Data/portfolio.csv')
>>> from tableformat import create_formatter, print_table
>>> formatter = create_formatter('txt')
>>> print_table(portfolio, ['name','shares'], formatter)
      name     shares
---------- ----------
        AA        100
       IBM         50
       CAT        150
      MSFT        200
        GE         95
      MSFT         50
       IBM        100

>>> print_table(portfolio, ['name','shares','price'], formatter)
      name     shares      price
---------- ---------- ----------
        AA        100       32.2
       IBM         50       91.1
       CAT        150      83.44
      MSFT        200      51.23
        GE         95      40.37
      MSFT         50       65.1
       IBM        100      70.44
>>>
```

