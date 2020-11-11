[Contents](../Contents.md) \| [Previous (2.7 Object Model)](../02_Working_with_data/07_Objects.md) \| [Next (3.2 More on Functions)](02_More_functions.md)

# 3.1 Scripting

Bu bölümde, Python scriptleri yazma pratiğine daha yakından bakıyoruz.

### Script Nedir?

*Script* bir dizi ifadeyi çalıştıran ve durduran bir programdır.

```python
# program.py

statement1
statement2
statement3
...
```

Bu noktaya kadar çoğunlukla scriptler yazıyoruz.

### Problem

Kullanışlı bir script yazarsanız, özellik ve işlevsellik açısından büyür. 
Diğer ilgili sorunlara uygulamak isteyebilirsiniz. Zamanla kritik bir uygulama haline 
gelebilir. Ve eğer dikkat etmezseniz, büyük bir karışıklığa dönüşebilir. 
Öyleyse organize olalım.

### Nesneleri Tanımlamak

İsimler daha sonra kullanılmadan önce mutlaka tanımlanmalıdır.

```python
def square(x):
    return x*x

a = 42
b = a + 2     # `a`'nın tanımlı olmasını gerektirir.

z = square(b) # `square` ve `b`'nin tanımlanmasını gerektirir.
```

**Sıra önemlidir.**
Neredeyse her zaman değişkenlerin ve fonksiyonların tanımlarını en üste yakın koyarsınız.

### Fonksiyonları Tanımlama

Tek bir *görevle* ilgili tüm kodu aynı yere koymak iyi bir fikirdir.
Bir fonksiyon kullanın.

```python
def read_prices(filename):
    prices = {}
    with open(filename) as f:
        f_csv = csv.reader(f)
        for row in f_csv:
            prices[row[0]] = float(row[1])
    return prices
```

Ayrıca, bir fonksiyon tekrarlanan işlemleri de basitleştirir.

```python
oldprices = read_prices('oldprices.csv')
newprices = read_prices('newprices.csv')
```

### Fonksiyon nedir?

Bir fonksiyon, adlandırılmış bir ifade dizisidir.

```python
def funcname(args):
  statement
  statement
  ...
  return result
```

*Herhangi* bir Python ifadesi içeride kullanılabilir.

```python
def foo():
    import math
    print(math.sqrt(2))
    help(math)
```

Python'da *özel* ifadeler yoktur (bu da hatırlamayı kolaylaştırır).

### Fonksiyon Tanımı

Fonksiyonlar herhangi bir sırada *tanımlanabilir*.

```python
def foo(x):
    bar(x)

def bar(x):
    statements

# OR
def bar(x):
    statements

def foo(x):
    bar(x)
```

Fonksiyonlar, program yürütülürken gerçekten kullanılmadan (veya çağrılmadan) önce tanımlanmalıdır.

```python
foo(3)        # foo zaten tanımlanmış olmalı
```

Biçimsel olarak, fonksiyonların *aşağıdan yukarıya* şeklinde tanımlanması muhtemelen daha yaygındır.

### Aşağıdan Yukarıya Şeklinde

Fonksiyonlar, yapı taşları olarak kabul edilir.
Daha küçük/daha basit bloklar önce gelir.

```python
# myprogram.py
def foo(x):
    ...

def bar(x):
    ...
    foo(x)          # Yukarıda tanımlanmış
    ...

def spam(x):
    ...
    bar(x)          # Yukarıda tanımlanmış
    ...

spam(42)            # Fonksiyonları kullanan kod sonda görünür
```

Daha sonraki fonksiyonlar daha önceki fonksiyonlar üzerine inşa edilir.
Yine, bu sadece bir stil noktasıdır. Yukarıdaki programda önemli olan tek şey, `spam(42)` 
çağrısının en sona gitmesidir.


### Fonksiyon Tasarımı

İdeal olarak, fonksiyonlar bir *kara kutu* olmalıdır.
Yalnızca geçirilen girdiler üzerinde çalışmalı, global değişkenlerden ve 
gizemli yan etkilerden kaçınmalıdırlar. Ana hedefleriniz: Modülerlik ve Öngörülebilirlik.

### Doc-string

Belgeleri bir doc-string biçiminde dahil etmek iyi bir uygulamadır.
Doc-string, fonksiyonun adından hemen sonra yazılan dizelerdir.
`help()`, IDE ve diğer araçları beslerler.

```python
def read_prices(filename):
    '''
    Read prices from a CSV file of name,price data
    '''
    prices = {}
    with open(filename) as f:
        f_csv = csv.reader(f)
        for row in f_csv:
            prices[row[0]] = float(row[1])
    return prices
```

Doc-string için iyi bir uygulama, fonksiyonun ne yaptığının kısa bir cümle özetini
yazmaktır. Daha fazla bilgiye ihtiyaç duyulursa, argümanların daha ayrıntılı bir
açıklaması ile birlikte kısa bir kullanım örneği ekleyin.


### Tür Açıklamaları

Fonksiyon tanımlarına opsiyonel tür ipuçları da ekleyebilirsiniz.

```python
def read_prices(filename: str) -> dict:
    '''
    Read prices from a CSV file of name,price data
    '''
    prices = {}
    with open(filename) as f:
        f_csv = csv.reader(f)
        for row in f_csv:
            prices[row[0]] = float(row[1])
    return prices
```

İpuçları operasyonel olarak hiçbir şey yapmaz. Tamamen bilgi amaçlıdırlar.
Ancak, IDE'ler, kod denetleyicileri ve diğer araçlar tarafından daha fazlasını
yapmak için kullanılabilirler.


## Egzersizler


2. bölümde, bir hisse senedi portföyünün performansını gösteren bir rapor yazdıran `report.py`
adlı bir program yazdınız. Bu program bazı fonksiyonlardan oluşuyordu. Örneğin: 


```python
# report.py
import csv

def read_portfolio(filename):
    '''
    Read a stock portfolio file into a list of dictionaries with keys
    name, shares, and price.
    '''
    portfolio = []
    with open(filename) as f:
        rows = csv.reader(f)
        headers = next(rows)

        for row in rows:
            record = dict(zip(headers, row))
            stock = {
                'name' : record['name'],
                'shares' : int(record['shares']),
                'price' : float(record['price'])
            }
            portfolio.append(stock)
    return portfolio
...
```


Bununla birlikte, programın bir dizi komut dosyasıyla hesaplama yapan bölümleri de vardı.
Bu kod programın sonuna yakın bir yerde belirdi. Örneğin:

```python
...

# Output the report

headers = ('Name', 'Shares', 'Price', 'Change')
print('%10s %10s %10s %10s'  % headers)
print(('-' * 10 + ' ') * len(headers))
for row in report:
    print('%10s %10d %10.2f %10.2f' % row)
...
```


Bu alıştırmada, bu programı ele alacağız ve onu fonksiyonların kullanımı konusunda biraz 
daha güçlü bir şekilde düzenleyeceğiz.

### Egzersiz 3.1: Bir Programı Fonksiyonlar Koleksiyonu Olarak Yapılandırma

`report.py` programınızı, hesaplamalar ve çıktılar dahil olmak üzere tüm ana işlemler bir 
fonksiyonlar koleksiyonu tarafından yürütülecek şekilde değiştirin. Özellikle: 

* Raporu yazdıran bir `print_report(report)` fonksiyonu oluşturun.
* Programın son bölümünü, bir dizi fonksiyon çağrısından başka bir şey olmayacak ve başka bir hesaplama olmayacak şekilde değiştirin.

### Egzersiz 3.2: Program Yürütme İçin Üst Düzey Bir Fonksiyon Oluşturma

Programınızın son bölümünü alın ve tek bir fonksiyona paketleyin: `portfolio_report(portfolio_filename, prices_filename)`.

Aşağıdaki fonksiyon çağrısının raporu daha önce olduğu gibi oluşturması için
fonksiyonun çalışmasını sağlayın:

```python
portfolio_report('Data/portfolio.csv', 'Data/prices.csv')
```

Bu son versiyonda, programınız bir dizi fonksiyon tanımından başka bir şey olmayacak ve ardından
en sonunda `portfolio_report()`'a tek bir fonksiyon çağrısı yapacaktır (programda yer alan tüm adımları yürütür).


Programınızı tek bir fonksiyona dönüştürmek, onu farklı girdilerle çalıştırmayı kolay hale getirir.
Örneğin, programınızı çalıştırdıktan sonra şu ifadeleri interaktif olarak deneyin:

```python
>>> portfolio_report('Data/portfolio2.csv', 'Data/prices.csv')
... çıktıya bakın ...
>>> files = ['Data/portfolio.csv', 'Data/portfolio2.csv']
>>> for name in files:
        print(f'{name:-^43s}')
        portfolio_report(name, 'Data/prices.csv')
        print()

... çıktıya bakın ...
>>>
```

### Yorum

Python, içinde bir dizi ifadenin bulunduğu bir dosyaya sahip olduğunuzda nispeten 
yapılandırılmamış script kodu yazmayı çok kolaylaştırır. Büyük resimde, mümkün olduğunca
fonksiyonları kullanmak neredeyse her zaman daha iyidir. Bir noktada, bu senaryo büyüyecek ve 
biraz daha organizasyonunuz olmasını dileyeceksiniz. Ayrıca, az bilinen bir gerçek, fonksiyonları
kullanırsanız Python biraz daha hızlı çalışır.

[Contents](../Contents.md) \| [Previous (2.7 Object Model)](../02_Working_with_data/07_Objects.md) \| [Next (3.2 More on Functions)](02_More_functions.md)