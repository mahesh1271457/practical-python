[Contents](../Contents.md) \| [Previous (2.1 Datatypes)](01_Datatypes.md) \| [Next (2.3 Formatting)](03_Formatting.md)

# 2.2 Containers

Bu bölümde listeler, sözlükler ve sets(diziler/kümeler) den bahsedeceğiz.

### Genel Bakış

Programlar genellikle birçok objeyle çalışmak zorundadır.

* Hisse senedi portföyü
* Hisse senedi fiyatlarının tablosu

Kullanılacak üç ana seçenek vardır.

* Listeler, (sıralı veriler).
* Sözlükler, (sırasız veriler).
* Sets(diziler), (sırasız eşsiz nesne koleksiyonu).

### Containers olarak listeler

Düzenli veriler konu olduğunda listeyi kullan. Unutma listeler herhangi tipte 
nesneleri tutabilirler. Örneğin ,tupleların listesi.

```python
portfolio = [
    ('GOOG', 100, 490.1),
    ('IBM', 50, 91.3),
    ('CAT', 150, 83.44)
]

portfolio[0]            # ('GOOG', 100, 490.1)
portfolio[2]            # ('CAT', 150, 83.44)
```

### Listenin Yapısı

Bir liste oluşturalım.

```python
records = []  # boş liste tanımı

#  .append() kullanarak eleman ekleme
records.append(('GOOG', 100, 490.10))
records.append(('IBM', 50, 91.3))
...
```

Bir dosyayı okurken ki örnek.

```python
records = []  # Initial empty list

with open('Data/portfolio.csv', 'rt') as f:
    next(f) # Skip header
    for line in f:
        row = line.split(',')
        records.append((row[0], int(row[1]), float(row[2])))
```

### Containers olarak sözlükler

Sözlükler hızlıca içeriğe göz atmak için kullanışlıdır (anahtar kullanarak). Örneğin 
hisse fiyatları için:

```python
prices = {
   'GOOG': 513.25,
   'CAT': 87.22,
   'IBM': 93.37,
   'MSFT': 44.12
}
```

İşte birkaç basit bakış daha:

```python
>>> prices['IBM']
93.37
>>> prices['GOOG']
513.25
>>>
```

### Sözlük Yapısı

Örnek olarak bir sözlük tanımlama.

```python
prices = {} # boş sözlük

# yeni objeler ekleyelim
prices['GOOG'] = 513.25
prices['CAT'] = 87.22
prices['IBM'] = 93.37
```

Bir örnek ,dosyadan sözlük içeriğini doldurmak.

```python
prices = {} # Initial empty dict

with open('Data/prices.csv', 'rt') as f:
    for line in f:
        row = line.split(',')
        prices[row[0]] = float(row[1])
```

Not: Eğer bunu `Data/prices.csv` dosyasında deniyorsan, tam çalıştığını düşünürken
dosyanın sonunda bir satır boşluk olduğu için program hata verecektir .Bunun için 
kodu bir miktar düzenlemen gerekecektir. (Alıştırma 2.6 ye göz atın).

### Sözlük Sorguları

Anahtar(key) ile sözlüğü test edebilirsin.

```python
if key in d:
    # YES
else:
    # NO
```

Var olmayan bir değeri aratabilir ve bu durumda karşına gelmesi için bir
varsayılan değer tanımlayabilirsin.

```python
name = d.get(key, default)
```

Bir örnek:

```python
>>> prices.get('IBM', 0.0)
93.37
>>> prices.get('SCOX', 0.0)
0.0
>>>
```

### Birleşik Anahtarlar

Python'da hemen hemen her tür değer bir sözlük anahtarı olarak kullanılabilir. Bir sözlük anahtarı değişmez tipte olmalıdır.
Örneğin, tuplelar:

```python
holidays = {
  (1, 1) : 'New Years',
  (3, 14) : 'Pi day',
  (9, 13) : "Programmer's day",
}
```

Sonra ulaşmayı deneyelim :

```python
>>> holidays[3, 14]
'Pi day'
>>>
```

*Bir liste,sets(dizi/küme) veya başka bir sözlük ,sözlük anahtarı olarak kullanılamaz çünkü liste ve sözlükler değiştirilemezdir.*

### Sets(dizi/küme)

Setler düzenlenmemiş sıradışı öğeler koleksiyonudur.

```python
tech_stocks = { 'IBM','AAPL','MSFT' }
# Alternative syntax
tech_stocks = set(['IBM', 'AAPL', 'MSFT'])
```

Setler üyelik(dahil olma) testlerinde başarılıdır.

```python
>>> tech_stocks
set(['AAPL', 'IBM', 'MSFT'])
>>> 'IBM' in tech_stocks
True
>>> 'FB' in tech_stocks
False
>>>
```

Setler ayrıca kopyaları elemek içinde kullanışlıdır.

```python
names = ['IBM', 'AAPL', 'GOOG', 'IBM', 'GOOG', 'YHOO']

unique = set(names)
# unique = set(['IBM', 'AAPL','GOOG','YHOO'])
```

Bazı set işlemleri:

```python
names.add('CAT')        # ekleme
names.remove('YHOO')    # kaldırma
s1 | s2                 # Set birleşimi
s1 & s2                 # Set kesişimi
s1 - s2                 # Set farkı
```

## Alıştırmalar

Bu alıştırmalarda, kursun geri kalanında kullanılan ana programlardan birini oluşturmaya başlıyorsunuz.
Çalışmaların `Work/report.py` dizisinde.

### Alıştırma 2.4: Tuple Listesi

`Data/portfolio.csv` dizinde hisse portföylerinin olduğu bir liste bulunuyor.
Bu alıştırmada [Alıştırma 1.30](../01_Introduction/07_Functions.md),
bir fonksiyon yazalım, `portfolio_cost(filename)` bu fonksiyon dosyayı okusun ve basit
hesaplamalar yapsın.

Kodunuz şu şeklide görünecektir:

```python
# pcost.py

import csv

def portfolio_cost(filename):
    '''Computes the total cost (shares*price) of a portfolio file'''
    total_cost = 0.0

    with open(filename, 'rt') as f:
        rows = csv.reader(f)
        headers = next(rows)
        for row in rows:
            nshares = int(row[1])
            price = float(row[2])
            total_cost += nshares * price
    return total_cost
```

Bu kodu yol gösterici olarak kullanırsak, `report.py` isimli yeni bir dosya açalım. Bu dosyada,
`read_portfolio(filename)`adlı bir fonksiyon tanımlayalım ,bu portföyü okusun ve bize bir tuple listesi versin. 
Bunu yapmak için,yukarıdaki kodunuzda birkaç değişiklik yapacaksınız.

İlk olarak,bir değişken tanımlayalım `total_cost = 0`ve bir boş liste tanımlıcaksınız.
Örneğin:

```python
portfolio = []
```

Ardından ,tüm maliyeti toplamk yerine,her satırı tıpkı son alıştırmada yaptığınız 
gibi bir tuple'a çevirecek ve bunu listeye ekleyeceksiniz. Örneğin:

```python
for row in rows:
    holding = (row[0], int(row[1]), float(row[2]))
    portfolio.append(holding)
```

Son olarak, sonucu `portfolio` listesinde bulacaksın.

Fonksiyonunuzda denemeler yapın(hatırlatma olarak:bunu yapmak için,
 `report.py` dosyasını komut istemcisinde çalıştırman gerekir):

*İpucu: `-i` komutu ile terminalden dosyayı çalıştırılır*

```python
>>> portfolio = read_portfolio('Data/portfolio.csv')
>>> portfolio
[('AA', 100, 32.2), ('IBM', 50, 91.1), ('CAT', 150, 83.44), ('MSFT', 200, 51.23),
    ('GE', 95, 40.37), ('MSFT', 50, 65.1), ('IBM', 100, 70.44)]
>>>
>>> portfolio[0]
('AA', 100, 32.2)
>>> portfolio[1]
('IBM', 50, 91.1)
>>> portfolio[1][1]
50
>>> total = 0.0
>>> for s in portfolio:
        total += s[1] * s[2]

>>> print(total)
44671.15
>>>
```

Tuple listesi ,oluşturabileceğimiz 2-D(2 boyutlu)arrayler gibidir. 
Örenğin,istediğiniz satır ve sütuna ulaşmak için şunun gibi `portfolio[row][column]`ifade kullanırız
(row ve column integer tiplidir).

Bu bahsedileni, for-loop u kullanarak tekrar yazabilirsiniz.Şu şekilde:

```python
>>> total = 0.0
>>> for name, shares, price in portfolio:
            total += shares*price

>>> print(total)
44671.15
>>>
```

### Alıştırma 2.5: Sözlüklerin Listesi

Alıştırma 2.4  kullanılan fonksiyonu alalım ve portföydeki her bir hisseyi tuple yerine sözlükle tanımlayalım.
Bu sözlükte parça isimleri olarak "name", "shares" ve "price" kullanalım ,bunlar farklı sütünları
temsil edecektir.

Bu yeni fonksiyonu alıştırma 2.4 deki ile aynı şekilde deneyin..

```python
>>> portfolio = read_portfolio('Data/portfolio.csv')
>>> portfolio
[{'name': 'AA', 'shares': 100, 'price': 32.2}, {'name': 'IBM', 'shares': 50, 'price': 91.1},
    {'name': 'CAT', 'shares': 150, 'price': 83.44}, {'name': 'MSFT', 'shares': 200, 'price': 51.23},
    {'name': 'GE', 'shares': 95, 'price': 40.37}, {'name': 'MSFT', 'shares': 50, 'price': 65.1},
    {'name': 'IBM', 'shares': 100, 'price': 70.44}]
>>> portfolio[0]
{'name': 'AA', 'shares': 100, 'price': 32.2}
>>> portfolio[1]
{'name': 'IBM', 'shares': 50, 'price': 91.1}
>>> portfolio[1]['shares']
50
>>> total = 0.0
>>> for s in portfolio:
        total += s['shares']*s['price']

>>> print(total)
44671.15
>>>
```

Burada, her giriş için farklı alanların olduğunu fark edeceksiniz,bunlara
sayısal sütun numaraları yerine anahtar(key) adlarıyla erişilir. Genellikle 
kodun sonucunu okunmasını kolaylaştırdığı için tercih edilir.

Büyük sözlük ve listelerin görünüşü dağınık olabilir. Bu karmaşıklığı 
temizlemek için `pprint`fonksiyonunu kullanabilirsin.

```python
>>> from pprint import pprint
>>> pprint(portfolio)
[{'name': 'AA', 'price': 32.2, 'shares': 100},
    {'name': 'IBM', 'price': 91.1, 'shares': 50},
    {'name': 'CAT', 'price': 83.44, 'shares': 150},
    {'name': 'MSFT', 'price': 51.23, 'shares': 200},
    {'name': 'GE', 'price': 40.37, 'shares': 95},
    {'name': 'MSFT', 'price': 65.1, 'shares': 50},
    {'name': 'IBM', 'price': 70.44, 'shares': 100}]
>>>
```

### Alıştırma 2.6: Containers olarak Sözlükler

Sözlük, öğeleri istediğiniz yerde takip etmek için yararlı bir yapıdır . Python shell(kabuk)unda,
sözlüklerle uğraşmayı deneyelim:

```python
>>> prices = { }
>>> prices['IBM'] = 92.45
>>> prices['MSFT'] = 45.12
>>> prices
... look at the result ...
>>> prices['IBM']
92.45
>>> prices['AAPL']
... look at the result ...
>>> 'AAPL' in prices
False
>>>
```

Dosya (`Data/prices.csv` ) hisse senedi fiyatlarını içeren satırları içerir.
Dosya şu şekilde görünecektir:

```csv
"AA",9.22
"AXP",24.85
"BA",44.85
"BAC",11.27
"C",3.72
...
```

`read_prices(filename)` şeklinde bir fonksiyon yazalım ,veriyi okusun hisseler için bir sözlük oluştursun,
anahtar olarak(key) hisse isimleri ve değerlerini(value)
içeren bir sözlük olsun.

Bunu yaparken, boş bir sözlük olarak başlayalım ve değerleri ekleyelim.
Ancak değerleri dosyadan okuyoruz.

Hisse adı ve değerlerine bakmak için daha hızlı veri yapısı kullancağız.

Bu bölüm için ihtiyaç duyacağınız birkaç küçük ipucu. `csv` modülünü kullandığınızdan emin olun
burada tekerleği yeniden icat etmenize gerek yok.

```python
>>> import csv
>>> f = open('Data/prices.csv', 'r')
>>> rows = csv.reader(f)
>>> for row in rows:
        print(row)


['AA', '9.22']
['AXP', '24.85']
...
[]
>>>
```

Diğer bir karmaşıklık `Data/prices.csv` dosyası boş satırlar içeriyor olabilir.
Farkına varmak zor değil bunun anlamı bu satırda herhangi bir verinin olmadığıdır.

Bunun programınızın düzgün çalışmasını engelleme ihtimali vardır. `try`- `except` ifadesini kullanarak
bu hataları yakalayabilirsiniz.  Düşünce : Bozuk verilere karşı 'if' ifadesini kullanmak daha mı yararlı olurdu ?

`read_prices()` fonksiyonun yazdıktan sonra, interaktif komut istemcinizde çalıştırarak
çalıştığını kontrol edin :

```python
>>> prices = read_prices('Data/prices.csv')
>>> prices['IBM']
106.28
>>> prices['MSFT']
20.89
>>>
```

### Alıştırma 2.7: Emekli olup olmayacağınızı öğrenmek

 `report.py`programınıza birkaç ifade ekleyerek tüm çalışmayı birbirine bağlayın.  Bu ifadeler size Alıştırma 2.5 deki hisseleri , 
Alıştırma 2.6 daki sözlükteki fiyatları ve mevcut portföydeki değerleri ile bir kazanç/kayıp hesabı yapsın.

[Contents](../Contents.md) \| [Previous (2.1 Datatypes)](01_Datatypes.md) \| [Next (2.3 Formatting)](03_Formatting.md)
