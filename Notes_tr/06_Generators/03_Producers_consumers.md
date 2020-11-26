# 6.3 Üreticiler, Tüketiciler ve Pipeline’lar

Generator’lar, çeşitli üretici / tüketici problemlerini ve veri akışı pipeline’larını belirlemek için yararlı bir araçtır. Bu bölümde bunu tartışacağız.
 
### Üretici-Tüketici Problemleri

Generator’lar, *üretici(producer)-tüketici(consumer)* sorunlarının çeşitli biçimleri ile yakından ilgilidir.

```python
# Producer
def follow(f):
    ...
    while True:
        ...
        yield line        # Aşağıdaki `line` içinde değer üretir.
        ...

# Consumer
for line in follow(f):    # Yukarıdaki `yield` den değer tüketir.
    ...
```
 `for` un tükettiği değerleri `yield` üretir.

### Generator Pipeline’ları

Üreticilerin bu yönünü işleme pipeline’larını (Unix pipe’ları gibi) kurmak için kullanabilirsiniz.

*üretici(producer)* &rarr; *işleme(processing)* &rarr; *işleme(processing)* &rarr; *tüketici(consumer)*

İşleme pipe’larının bir ilk veri üreticisi, bazı ara işleme aşamaları ve son bir tüketicisi vardır.

**üretici** &rarr; *işleme* &rarr; *işleme* &rarr; *tüketici*

```python
def producer():
    ...
    yield item
    ...
```

Üretici tipik olarak bir generator’dur. Yine de başka bir dizinin listesi olabilir. `yield` verileri pipeline’ı besler.

*üretici* &rarr; *işleme* &rarr; *işleme* &rarr; **tüketici**

```python
def consumer(s):
    for item in s:
    ...
```

Tüketici bir for döngüsüdür. Öğeleri alır ve onlarla bir şeyler yapar.

*üretici* &rarr; **işleme** &rarr; **işleme** &rarr; *tüketici*

```python
def processing(s):
    for item in s:
        ...
        yield newitem
      ...
```

Ara işlem aşamaları eşzamanlı olarak öğeleri tüketir ve üretir. Veri akışını değiştirebilirler. Ayrıca filtreleyebilirler (öğeleri atarak).

*üretici* &rarr; *işleme* &rarr; *işleme* &rarr; *tüketici*

```python
def producer():
    ...
    yield item          # "processing" tarafından alınan öğeyi verir
    ...

def processing(s):
    for item in s:      # "producer" dan geliyor
        ...
        yield newitem   # yeni bir ürün verir
        ...

def consumer(s):
    for item in s:      # “processing” den geliyor
    ...
```

Pipeline’ı oluşturmak için gerekli kod,

```python
a = producer()
b = processing(a)
c = consumer(b)
```

Verilerin aşamalı olarak farklı fonksiyonlardan geçtiğini fark edeceksiniz.

## Alıştırmalar

Bu alıştırma için `stocksim.py` programı hala arka planda çalışıyor olmalıdır. Önceki alıştırmada yazdığınız `follow()` fonksiyonunu kullanacaksınız.

### Alıştırma 6.8: Basit bir pipeline kurmak

Pipeline fikrini uygulamalı görelim. Şu fonksiyonu yazın:

```python
>>> def filematch(lines, substr):
        for line in lines:
            if substr in line:
                yield line

>>>
```

Bu fonksiyon, önceki alıştırmadaki ilk oluşturucu örneğiyle neredeyse tamamen aynıdır, ancak artık bir dosyayı açmıyor - yalnızca kendisine argüman olarak verilen bir dizi satır üzerinde çalışıyor. Şimdi şunu deneyin:
```
>>> lines = follow('Data/stocklog.csv')
>>> ibm = filematch(lines, 'IBM')
>>> for line in ibm:
        print(line)

... wait for output ...
```

Çıktının görünmesi biraz zaman alabilir, ancak sonunda IBM için veri içeren bazı satırlar görmeniz gerekir.

### Alıştırma 6.9: biraz daha karmaşık bir pipeline kurmak

Daha fazla eylem gerçekleştirerek pipeline fikrini birkaç adım ileriye taşıyın.

```
>>> from follow import follow
>>> import csv
>>> lines = follow('Data/stocklog.csv')
>>> rows = csv.reader(lines)
>>> for row in rows:
        print(row)

['BA', '98.35', '6/11/2007', '09:41.07', '0.16', '98.25', '98.35', '98.31', '158148']
['AA', '39.63', '6/11/2007', '09:41.07', '-0.03', '39.67', '39.63', '39.31', '270224']
['XOM', '82.45', '6/11/2007', '09:41.07', '-0.23', '82.68', '82.64', '82.41', '748062']
['PG', '62.95', '6/11/2007', '09:41.08', '-0.12', '62.80', '62.97', '62.61', '454327']
    ...
```
 
Burda ilginç bir şey var. Burada gördüğünüz şey, `follow()` fonksiyonun çıktısının `csv.reader()` fonksiyonuna aktarılmış olmasıdır. Şimdi bir dizi bölünmüş satır elde ediyoruz.

### Alıştırma 6.10: Daha fazla pipeline bileşeni yaratmak

Tüm fikri daha büyük bir pipeline’a genişletelim. Ayrı bir dosyada `ticker.py`, yukarıda yaptığınız gibi bir CSV dosyasını okuyan bir fonksiyon oluşturarak başlayın:

```python
# ticker.py

from follow import follow
import csv

def parse_stock_data(lines):
    rows = csv.reader(lines)
    return rows

if __name__ == '__main__':
    lines = follow('Data/stocklog.csv')
    rows = parse_stock_data(lines)
    for row in rows:
        print(row)
```

Belirli sütunları seçen yeni bir fonksiyon yazın:

```
# ticker.py
...
def select_columns(rows, indices):
    for row in rows:
        yield [row[index] for index in indices]
...
def parse_stock_data(lines):
    rows = csv.reader(lines)
    rows = select_columns(rows, [0, 1, 4])
    return rows
‘’’
Programınızı yeniden çalıştırın. Çıktının şu şekilde daraldığını görmelisiniz:
‘’’
['BA', '98.35', '0.16']
['AA', '39.63', '-0.03']
['XOM', '82.45','-0.23']
['PG', '62.95', '-0.12']
    ...
```


Veri türlerini dönüştüren ve sözlükler oluşturan generator fonksiyonları yazın. Örneğin:

```python
# ticker.py
...

def convert_types(rows, types):
    for row in rows:
        yield [func(val) for func, val in zip(types, row)]

def make_dicts(rows, headers):
    for row in rows:
        yield dict(zip(headers, row))
...
def parse_stock_data(lines):
    rows = csv.reader(lines)
    rows = select_columns(rows, [0, 1, 4])
    rows = convert_types(rows, [str, float, float])
    rows = make_dicts(rows, ['name', 'price', 'change'])
    return rows
    ...
```

Programınızı yeniden çalıştırın. Şimdi bunun gibi bir sözlük akışı yapmalısınız:

```
{ 'name':'BA', 'price':98.35, 'change':0.16 }
{ 'name':'AA', 'price':39.63, 'change':-0.03 }
{ 'name':'XOM', 'price':82.45, 'change': -0.23 }
{ 'name':'PG', 'price':62.95, 'change':-0.12 }
    ...
```

### Alıştırma 6.11: Veriyi filtreleme

Verileri filtreleyen bir fonksiyon yazın. Örneğin:

```python
# ticker.py
...

def filter_symbols(rows, names):
    for row in rows:
        if row['name'] in names:
            yield row
```

Sadece portfolyonuzdaki stock’ları filtrelemek için bunu kullanın:

```
import report
portfolio = report.read_portfolio('Data/portfolio.csv')
rows = parse_stock_data(follow('Data/stocklog.csv'))
rows = filter_symbols(rows, portfolio)
for row in rows:
    print(row)
```

### Alıştırma 6.12: Hepsini bir araya koymak

`ticker.py` programında, belirli bir portfolyo, günlük dosyası ve tablo biçiminden gerçek zamanlı bir stock takibi oluşturan bir fonksiyon `ticker(portfile, logfile, fmt)` yazın. Örneğin:

```python
>>> from ticker import ticker
>>> ticker('Data/portfolio.csv', 'Data/stocklog.csv', 'txt')
      Name      Price     Change
---------- ---------- ----------
        GE      37.14      -0.18
      MSFT      29.96      -0.09
       CAT      78.03      -0.49
        AA      39.34      -0.32
...

>>> ticker('Data/portfolio.csv', 'Data/stocklog.csv', 'csv')
Name,Price,Change
IBM,102.79,-0.28
CAT,78.04,-0.48
AA,39.35,-0.31
CAT,78.05,-0.47
    ...
```

### Tartışma

Alınan bazı dersler: Çeşitli generator fonksiyonları oluşturabilir ve veri akışı pipeline’larını içeren işlemleri gerçekleştirmek için bunları birbirine bağlayabilirsiniz. Ek olarak, bir dizi işlem pipieline’ını tek bir fonksiyon çağrısında paketleyen fonksiyonlar (örneğin, `parse_stock_data()` fonksiyonu) oluşturabilirsiniz.
