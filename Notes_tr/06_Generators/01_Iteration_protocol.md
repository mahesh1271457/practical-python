[Contents](../Contents.md) \| [Previous (5.2 Encapsulation)](../05_Object_model/02_Classes_encapsulation.md) \| [Next (6.2 Customizing Iteration)](02_Customizing_iteration.md)

# 6.1 Iteration Protocol(İterasyon Protokolü)

Bu bölümde iteration'nın işleyişine göz atacağız.

### Iteration Heryerde

Birçok farklı obje iteration'nu kullanımını destekler.

```python
a = 'hello'
for c in a: # Loop over characters in a
    ...

b = { 'name': 'Dave', 'password':'foo'}
for k in b: # Loop over keys in dictionary
    ...

c = [1,2,3,4]
for i in c: # Loop over items in a list/tuple
    ...

f = open('foo.txt')
for x in f: # Loop over lines in a file
    ...
```

### Iteration: Protocol

`for` ifadesini düşünelim.

```python
for x in obj:
    # statements
```

Bunun altında neler yatıyor?

```python
_iter = obj.__iter__()        # Get iterator object
while True:
    try:
        x = _iter.__next__()  # Get next item
    except StopIteration:     # No more items
        break
    # statements ...
```

`for-loop` ile çalışabilen tüm objeler bu düşük seviyeli iteration'protokolüne uyar.

Örnek: Listeler üzerinde manual iteration .

```python
>>> x = [1,2,3]
>>> it = x.__iter__()
>>> it
<listiterator object at 0x590b0>
>>> it.__next__()
1
>>> it.__next__()
2
>>> it.__next__()
3
>>> it.__next__()
Traceback (most recent call last):
File "<stdin>", line 1, in ? StopIteration
>>>
```

### Supporting Iteration (iterasyonu destekleme)

Iteration eğer kendi objenizi eklemek istediğinizde kullanışlı olduğunu biliyorsunuz.
Örnek, kendi container'nızı yapmak.

```python
class Portfolio:
    def __init__(self):
        self.holdings = []

    def __iter__(self):
        return self.holdings.__iter__()
    ...

port = Portfolio()
for s in port:
    ...
```

## Alıştırmalar

### Alıştırma 6.1: Iteration Illustrated

Aşağıdaki listeyi takip edin:

```python
a = [1,9,4,25,16]
```

Manuel olarak listeyi iterate(yineleme) etme .  Bir iterator'ı `__iter__()` ile çağıralım
ve `__next__()` metodu ile ardışık öğeleri elde edelim.

```python
>>> i = a.__iter__()
>>> i
<listiterator object at 0x64c10>
>>> i.__next__()
1
>>> i.__next__()
9
>>> i.__next__()
4
>>> i.__next__()
25
>>> i.__next__()
16
>>> i.__next__()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
StopIteration
>>>
```

`next()` yerleşik fonksiyonu, `__next__()` metodunu iterator olarak çağırmak için  
bir kısayoldur.Bunu bir dosya üzerinde deneyelim:

```python
>>> f = open('Data/portfolio.csv')
>>> f.__iter__()    # Note: This returns the file itself
<_io.TextIOWrapper name='Data/portfolio.csv' mode='r' encoding='UTF-8'>
>>> next(f)
'name,shares,price\n'
>>> next(f)
'"AA",100,32.20\n'
>>> next(f)
'"IBM",50,91.10\n'
>>>
```

Dosyanın sonuna gelene kadar `next(f)` fonksiyonunu çağırmaya devam edelim. 
Neler olduğuna beraber bakalım.

### Alıştırma 6.2: Supporting Iteration(iterasyonu desteklemek)

Ara sıra, iteration'ı destekleyen kendi objelerinizi oluşturmak isteyebilirsiniz
--özellikle objeniz bir liste veya diğer iterate edilebilen ifade ise .  `portfolio.py`adlı yeni dosyamızda 
aşağıdaki gibi class (sınıf) tanımlayalım:

```python
# portfolio.py

class Portfolio:

    def __init__(self, holdings):
        self._holdings = holdings

    @property
    def total_cost(self):
        return sum([s.cost for s in self._holdings])

    def tabulate_shares(self):
        from collections import Counter
        total_shares = Counter()
        for s in self._holdings:
            total_shares[s.name] += s.shares
        return total_shares
```


Bu class  liste etrafındaki bir katman olması amaçlanmıştır ama `total_cost` gibi birkaç ekstra metotla beraber. 
`report.py` içindeki `read_portfolio()` fonksiyonunu düzenleyerek
 `Portfolio` şeklinde bir şey yaratmış olduk :

```
# report.py
...

import fileparse
from stock import Stock
from portfolio import Portfolio

def read_portfolio(filename):
    '''
    Read a stock portfolio file into a list of dictionaries with keys
    name, shares, and price.
    '''
    with open(filename) as file:
        portdicts = fileparse.parse_csv(file,
                                        select=['name','shares','price'],
                                        types=[str,int,float])

    portfolio = [ Stock(d['name'], d['shares'], d['price']) for d in portdicts ]
    return Portfolio(portfolio)
...
```

`report.py` programını çalıştrımayı deneyin. 'Portfolio' nın iteration yapamayacağı gerçeğini 
ve muhteşem bir şekilde başarısız olduğunu göreceksiniz.

```python
>>> import report
>>> report.portfolio_report('Data/portfolio.csv', 'Data/prices.csv')
... crashes ...
```

 `Portfolio` classsını iteration desteğiyle beraber düzelterek sorunu çözün:

```python
class Portfolio:

    def __init__(self, holdings):
        self._holdings = holdings

    def __iter__(self):
        return self._holdings.__iter__()

    @property
    def total_cost(self):
        return sum([s.shares*s.price for s in self._holdings])

    def tabulate_shares(self):
        from collections import Counter
        total_shares = Counter()
        for s in self._holdings:
            total_shares[s.name] += s.shares
        return total_shares
```

Değişikliği yaptıktan sonra,  `report.py` programını yeniden çaıştırın. Bunu yaparken , `pcost.py` programınızı
`Portfolio` objesini kullancak şekilde düzeltin. Şunun gibi:

```python
# pcost.py

import report

def portfolio_cost(filename):
    '''
    Computes the total cost (shares*price) of a portfolio file
    '''
    portfolio = report.read_portfolio(filename)
    return portfolio.total_cost
...
```

Çalıştığından emin olmak için test edin:

```python
>>> import pcost
>>> pcost.portfolio_cost('Data/portfolio.csv')
44671.15
>>>
```

### Alıştırma 6.3: Daha uygun bir container(kap) yapmak

Eğer container classı yapıyorsanız,genellikle daha fazla iteration yapmak istersiniz.
`Portfolio` classını düzenleyip daha başka özel metotlar kullanırsın,buna benzer:

```python
class Portfolio:
    def __init__(self, holdings):
        self._holdings = holdings

    def __iter__(self):
        return self._holdings.__iter__()

    def __len__(self):
        return len(self._holdings)

    def __getitem__(self, index):
        return self._holdings[index]

    def __contains__(self, name):
        return any([s.name == name for s in self._holdings])

    @property
    def total_cost(self):
        return sum([s.shares*s.price for s in self._holdings])

    def tabulate_shares(self):
        from collections import Counter
        total_shares = Counter()
        for s in self._holdings:
            total_shares[s.name] += s.shares
        return total_shares
```

Şimdi bu yeni classı kullanarak bazı denemelerde bulunun:

```
>>> import report
>>> portfolio = report.read_portfolio('Data/portfolio.csv')
>>> len(portfolio)
7
>>> portfolio[0]
Stock('AA', 100, 32.2)
>>> portfolio[1]
Stock('IBM', 50, 91.1)
>>> portfolio[0:3]
[Stock('AA', 100, 32.2), Stock('IBM', 50, 91.1), Stock('CAT', 150, 83.44)]
>>> 'IBM' in portfolio
True
>>> 'AAPL' in portfolio
False
>>>
```

Bununla ilgili önemli bir gözlem, Python'un diğer bölümlerinin normalde nasıl çalıştığına dair ortak kelime dağarcığından bahsediliyorsa kod genellikle 
"Pythonic" olarak kabul edilir. Container objeleri için destekli iteration, indeksleme, kapsama ve diğer tür operatörleri desteklemek bunun önemli bir parçasıdır.

[Contents](../Contents.md) \| [Previous (5.2 Encapsulation)](../05_Object_model/02_Classes_encapsulation.md) \| [Next (6.2 Customizing Iteration)](02_Customizing_iteration.md)
