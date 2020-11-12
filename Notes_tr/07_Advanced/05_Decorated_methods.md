[Contents](../Contents.md) \| [Previous (7.4 Decorators)](04_Function_decorators.md) \| [Next (8 Testing and Debugging)](../08_Testing_debugging/00_Overview.md)


# 7.5 Dekore Edilmiş Metotlar

Bu bölüm, metot tanımlamalarında kombine olarak kullanılan ve halihazırda tanımlanmış olan birkaç dekoratörden bahsetmektedir.

### Önden Tanımlanmış Dekoratörler

Class tanımlamalarındaki özel metot türlerini belirlemek için kullanılan ve önden tanımlanmış bazı dekoratörler vardır.

```python
class Foo:
    def bar(self,a):
        ...

    @staticmethod
    def spam(a):
        ...

    @classmethod
    def grok(cls,a):
        ...

    @property
    def name(self):
        ...
```

Teker teker ilerleyelim;

### Statik Metotlar

`@staticmethod` *statik* metotları tanımlamak için kullanılır. Statik metot, class’ın bir parçası olan fakat instance’lar(örnekler) üzerinde işlem yapmayan fonksiyonlardır.

```python
class Foo(object):
    @staticmethod
    def bar(x):
        print('x =', x)

>>> Foo.bar(2) x=2
>>>
```

Statik metotlar, bazen class içine destek kodu eklemek amacıyla da kullanılırlar. Örnek olarak oluşturulan instance’ların(örneklerin) yönetimine yardım kodu eklenmesi verilebilir. (hafıza yönetimi, sistem kaynakları, süreklilik, kilitleme vs.). Bazı tasarım örüntülerinde de kullanılabilirler.

### Class (Sınıf) Metotları

`@classmethod` metodu class metotları tanımlamak için kullanılır. Class metodu, ilk parametre olarak bir instance yerine *class* objesini alan bir metotdur.

```python
class Foo:
    def bar(self):
        print(self)

    @classmethod
    def spam(cls):
        print(cls)

>>> f = Foo()
>>> f.bar()
<__main__.Foo object at 0x971690>   # `f` örneği
>>> Foo.spam()
<class '__main__.Foo'>              # `Foo` class’ı
>>>
```

Class metotları genellikle alternatif constructorlar (yapıcı fonksiyon/metotlar) tanımlamak için bir araç olarak kullanılır.


```python
class Date:
    def __init__(self,year,month,day):
        self.year = year
        self.month = month
        self.day = day

    @classmethod
    def today(cls):
        # Class bir argüman olarak aktarılıyor.
        tm = time.localtime()
        # Ve yeni bir instance oluşturmak için kullanılıyor.
        return cls(tm.tm_year, tm.tm_mon, tm.tm_mday)

d = Date.today()
```

Class metotları inheritance(kalıtım) gibi özelliklerin kafa karıştırıcı sorunlarını çözebilir.

```python
class Date:
    ...
    @classmethod
    def today(cls):
        # Doğru class’ı alır (örneğin `NewDate`)
        tm = time.localtime()
        return cls(tm.tm_year, tm.tm_mon, tm.tm_mday)

class NewDate(Date):
    ...

d = NewDate.today()
```

## Alıştırmalar

### Alıştırma 7.11: Class Metotlarını Pratiğe Dökmek

`report.py` ve `portfolio.py` dosyalarında, `Portfolio` objesinin oluşturulması biraz sorunlu gibi görünüyor. Örneğin `report.py` dosyasının kodları şu şekilde:

```python
def read_portfolio(filename, **opts):
    '''
Bir stok portfolyo dosyasını; isim(name), hisse(shares) ve fiyat(price) anahtarlarına sahip bir sözlük listesine aktarma.
    '''
    with open(filename) as lines:
        portdicts = fileparse.parse_csv(lines,
                                        select=['name','shares','price'],
                                        types=[str,int,float],
                                        **opts)

    portfolio = [ Stock(**d) for d in portdicts ]
    return Portfolio(portfolio)
```

`portfolio.py` dosyası `Portfolio()` objesini şu şekilde tanımlıyor:

```python
class Portfolio:
    def __init__(self, holdings):
        self.holdings = holdings
    ...
```

Burada sorumluluk zinciri deseni biraz kafa karıştırıcı olabilir çünkü kod çok dağınık durumda. `Portfolio` class’ının `Stock` örneklerinden oluşan bir listeyi içermesi gerekiyor, belki class şu şekilde biraz değiştirilebilir:

```python
# portfolio.py

import stock

class Portfolio:
    def __init__(self):
        self.holdings = []

    def append(self, holding):
        if not isinstance(holding, stock.Stock):
            raise TypeError('Expected a Stock instance')
        self.holdings.append(holding)
    ...
```

Bir CSV dosyasından portfolyo okumak istiyorsanız, bunun için de bir class yaratmanız gerekebilir:

```python
# portfolio.py

import fileparse
import stock

class Portfolio:
    def __init__(self):
        self.holdings = []

    def append(self, holding):
        if not isinstance(holding, stock.Stock):
            raise TypeError('Expected a Stock instance')
        self.holdings.append(holding)

    @classmethod
    def from_csv(cls, lines, **opts):
        self = cls()
        portdicts = fileparse.parse_csv(lines,
                                        select=['name','shares','price'],
                                        types=[str,int,float],
                                        **opts)

        for d in portdicts:
            self.append(stock.Stock(**d))

        return self
```

Bu yeni Portfolio class’ını kullanabilmek için şöyle bir kod yazabilirsiniz:

```
>>> from portfolio import Portfolio
>>> with open('Data/portfolio.csv') as lines:
...     port = Portfolio.from_csv(lines)
...
>>>
```

Bu değişiklikleri `Portfolio` class’ı üzerinde uygulayın ve `report.py` kodlarını bu class metodunu kullanacak şekilde düzenleyin.

[Contents](../Contents.md) \| [Previous (7.4 Decorators)](04_Function_decorators.md) \| [Next (8 Testing and Debugging)](../08_Testing_debugging/00_Overview.md)


