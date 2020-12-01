[Contents](../Contents.md) \| [Previous (3.1 Scripting)](01_Script.md) \| [Next (3.3 Error Checking)](03_Error_checking.md)

# 3.2 Fonksiyonlar Hakkında Daha Fazlası

Fonksiyonlar daha önce tanıtılmış olsa da, gerçekte nasıl daha derin bir düzeyde çalıştıklarına dair 
çok az ayrıntı verilmiştir. Bu bölüm, bazı boşlukları doldurmayı ve çağrı kuralları, kapsam 
belirleme kuralları ve daha fazlası gibi konuları tartışmayı amaçlamaktadır.

### Fonksiyon Çağırma

Bu fonksiyonu düşünün: 

```python
def read_prices(filename, debug):
    ...
```

Fonksiyonu konumsal argümanlarla çağırabilirsiniz:

```
prices = read_prices('prices.csv', True)
```

Veya fonksiyonu anahtar kelime argümanlarıyla çağırabilirsiniz:

```python
prices = read_prices(filename='prices.csv', debug=True)
```

### Varsayılan Argümanlar


Bazen bir argümanın isteğe bağlı olmasını istersiniz. 
Öyleyse, fonksiyon tanımında varsayılan bir değer atayın.

```python
def read_prices(filename, debug=False):
    ...
```

Varsayılan bir değer atanmışsa, fonksiyon çağrılarında argüman isteğe bağlıdır.

```python
d = read_prices('prices.csv')
e = read_prices('prices.dat', True)
```

*Not: Varsayılanlara sahip argümanlar, argümanlar listesinin sonunda görünmelidir (isteğe bağlı olmayan tüm argümanlar önce gelir).*

### İsteğe bağlı argümanlar için anahtar kelime argümanlarını tercih edin

Bu iki farklı çağırma stilini kıyaslayın:

```python
parse_data(data, False, True) # ?????

parse_data(data, ignore_errors=True)
parse_data(data, debug=True)
parse_data(data, debug=True, ignore_errors=True)
```


Çoğu durumda, anahtar kelime argümanları, özellikle bayrak görevi gören veya isteğe bağlı 
özelliklerle ilgili olan argümanlar için kod netliğini artırır.

### En İyi Tasarım Uygulamaları

Fonksiyon argümanlarına her zaman kısa ama anlamlı isimler verin.

Bir fonksiyonu kullanan biri anahtar sözcük çağırma stilini kullanmak isteyebilir.

```python
d = read_prices('prices.csv', debug=True)
```

Python geliştirme araçları, yardım özelliklerinde ve belgelerde adları gösterecektir.

### Değer Döndürme

`return` ifadesi bir değer döndürür.

```python
def square(x):
    return x * x
```

Hiçbir dönüş değeri verilmemişse veya `return` eksikse, `None` döndürülür. 

```python
def bar(x):
    statements
    return

a = bar(4)      # a = None

# OR
def foo(x):
    statements  # `return` yok

b = foo(4)      # b = None
```

### Çoklu Dönüş Değerleri

Fonksiyonlar yalnızca bir değer döndürebilir. Ancak, bir fonksiyon, bunları bir demet içinde 
döndürerek birden çok değer döndürebilir.


```python
def divide(a,b):
    q = a // b      # Bölüm
    r = a % b       # Kalan
    return q, r     # Demet döndür
```

Kullanım örneği:

```python
x, y = divide(37,5) # x = 7, y = 2

x = divide(37, 5)   # x = (7, 2)
```

### Değişken Kapsamı

Programlar değişkenlere değerler atar.

```python
x = value # Global değişken

def foo():
    y = value # Yerel değişken
```

Değişken atamaları, fonksiyon tanımlarının dışında ve içinde gerçekleşir. Dışarıda tanımlanan
değişkenler evrensel (global) dir. Bir fonksiyonun içindeki değişkenler "yerel" dir.

### Yerel Değişkenler

Fonksiyonların içinde atanan değişkenler özeldir.

```python
def read_portfolio(filename):
    portfolio = []
    for line in open(filename):
        fields = line.split(',')
        s = (fields[0], int(fields[1]), float(fields[2]))
        portfolio.append(s)
    return portfolio
```

Bu örnekte, `filename`, `portfolio`, `line`, `fields` ve `s` yerel değişkenlerdir.
Bu değişkenler, fonksiyon çağrısından sonra tutulmaz veya erişilebilir değildir.

```python
>>> stocks = read_portfolio('portfolio.csv')
>>> fields
Traceback (most recent call last):
File "<stdin>", line 1, in ?
NameError: name 'fields' is not defined
>>>
```

Ayrıca, yerel değişkenler başka yerde bulunan değişkenlerle çakışamaz.

### Global Değişkenler

Fonksiyonlar, aynı dosyada tanımlanan global değişken değerlerine serbestçe erişebilir.

```python
name = 'Dave'

def greeting():
    print('Hello', name)  # `name` global değişkenini kullanmak
```

Ancak, fonksiyonlar global değişkenleri değiştiremez:

```python
name = 'Dave'

def spam():
  name = 'Guido'

spam()
print(name) # 'Dave' print ediyor
```

**Unutmayın: Fonksiyonlardaki tüm atamalar yereldir.**

### Global Değişkenleri Değiştirme

Global bir değişkeni değiştirmeniz gerekiyorsa, onu böyle değiştirmelisiniz:

```python
name = 'Dave'

def spam():
    global name
    name = 'Guido' # Yukarıdaki global adı değiştirir
```

Global değişken deklaresi, kullanımından önce görünmeli ve karşılık gelen değişken, fonksiyonla 
aynı dosyada bulunmalıdır. Bunu gördükten sonra, kötü bir form olarak kabul edildiğini 
bilin. Aslında, yapabiliyorsanız tamamen global değişkenden kaçınmaya çalışın. Fonksiyonun 
dışında bir tür durumu değiştirmek için bir fonksiyona ihtiyacınız varsa, bunun yerine bir 
sınıf kullanmak daha iyidir (bunu daha sonra göreceğiz).


### Argüman Geçirme

Bir fonksiyonu çağırdığınızda, argüman değişkenleri geçirilen değerlere atıfta bulunan
adlardır. Bu değerler kopya DEĞİLDİR ([section 2.7](../02_Working_with_data/07_Objects)). 
Eğer değişken veri türleri aktarılırsa (örn. listeler, kütüphaneler), bunlar *yerinde* değiştirilebilirler.

```python
def foo(items):
    items.append(42)    # Giriş nesnesini değiştirir

a = [1, 2, 3]
foo(a)
print(a)                # [1, 2, 3, 42]
```

**Anahtar nokta: Fonksiyonlar, girdi argümanların bir kopyasını almaz.**

### Yeniden Atama vs Değiştirme

Bir değeri değiştirmek ile bir değişken adını yeniden atamak arasındaki ince farkı
anladığınızdan emin olun.

```python
def foo(items):
    items.append(42)    # Giriş nesnesini değiştirir

a = [1, 2, 3]
foo(a)
print(a)                # [1, 2, 3, 42]

# VS
def bar(items):
    items = [4,5,6]    # Yerel `items` değişkenini farklı bir nesneye işaret edecek şekilde değiştirir

b = [1, 2, 3]
bar(b)
print(b)                # [1, 2, 3]
```

*Hatırlatma: Değişken atama asla hafıza üzerine yazmaz. İsim yalnızca yeni bir değere bağlıdır.*

## Egzersizler

Bu alıştırma seti, kursun belki de en güçlü ve en zor kısmını uygulamanızı sağlar. Çok sayıda
adım var ve geçmiş alıştırmalardan birçok kavram aynı anda bir araya getiriliyor. Nihai çözüm 
yalnızca 25 satırlık bir koddur, ancak acele etmeyin ve her bölümü anladığınızdan emin olun.

`report.py` programınızın merkezi kısmı, CSV dosyalarının okunmasına odaklanır. 
Örneğin, `read_portfolio()` fonksiyonu portföy verisi satırlarını içeren bir dosyayı okur ve
`read_prices()` fonksiyonu fiyat verisi satırlarını içeren bir dosyayı okur. Bu fonksiyonların
her ikisinde de, çok sayıda düşük seviyeli "fiddly" bitler ve benzer özellikler vardır. 
Örneğin ikisi de hem bir dosyayı açar hem de `csv` modülüyle sararlar ve her ikisi de çeşitli
alanları yeni türlere dönüştürürler.

Gerçekten çok fazla dosya ayrıştırma yapıyor olsaydınız, muhtemelen bunun bir kısmını temizlemek 
ve daha genel bir amaç yapmak istersiniz. Hedefimiz bu.

`Work/fileparse.py` adlı dosyayı açarak bu alıştırmayı başlatın. İşimizi burada yapacağız.

### Egzersiz 3.3: CSV Dosyalarını Okuma

Başlangıç olarak, bir CSV dosyasını bir dictionary listesine okuma sorununa odaklanalım. 
`fileparse.py` dosyasında, şuna benzer bir fonksiyon tanımlayın:


```python
# fileparse.py
import csv

def parse_csv(filename):
    '''
    Parse a CSV file into a list of records
    '''
    with open(filename) as f:
        rows = csv.reader(f)

        # Dosya başlıklarını okuyun
        headers = next(rows)
        records = []
        for row in rows:
            if not row:    # Veri içermeyen satırları atla
                continue
            record = dict(zip(headers, row))
            records.append(record)

    return records
```

Bu fonksiyon, dosya açmanın ayrıntılarını gizleyerek, `csv` modülüyle sararak, boş satırları
görmezden gelerek vb., bir CSV dosyasını bir dictionary listesine okur.

Deneyin:

İpucu: `python3 -i fileparse.py`.

```python
>>> portfolio = parse_csv('Data/portfolio.csv')
>>> portfolio
[{'price': '32.20', 'name': 'AA', 'shares': '100'}, {'price': '91.10', 'name': 'IBM', 'shares': '50'}, {'price': '83.44', 'name': 'CAT', 'shares': '150'}, {'price': '51.23', 'name': 'MSFT', 'shares': '200'}, {'price': '40.37', 'name': 'GE', 'shares': '95'}, {'price': '65.10', 'name': 'MSFT', 'shares': '50'}, {'price': '70.44', 'name': 'IBM', 'shares': '100'}]
>>>
```

Bu iyidir, ancak verilerle herhangi bir yararlı hesaplama yapamazsınız çünkü her şey bir dizi
olarak temsil edilir. Bunu kısa süre içinde düzelteceğiz, ancak bunun üzerine inşa etmeye devam 
edelim.

### Egzersiz 3.4: Sütun Seçici Oluşturma

Çoğu durumda, tüm verilerle değil, yalnızca bir CSV dosyasından seçilen sütunlarla ilgilenirsiniz.
`parse_csv()` fonksiyonunu, kullanıcı tanımlı sütunların aşağıdaki gibi isteğe bağlı olarak
seçilmesine izin verecek şekilde değiştirin:


```python
>>> # Tüm verileri okuyun
>>> portfolio = parse_csv('Data/portfolio.csv')
>>> portfolio
[{'price': '32.20', 'name': 'AA', 'shares': '100'}, {'price': '91.10', 'name': 'IBM', 'shares': '50'}, {'price': '83.44', 'name': 'CAT', 'shares': '150'}, {'price': '51.23', 'name': 'MSFT', 'shares': '200'}, {'price': '40.37', 'name': 'GE', 'shares': '95'}, {'price': '65.10', 'name': 'MSFT', 'shares': '50'}, {'price': '70.44', 'name': 'IBM', 'shares': '100'}]

>>> # Sadece bazı verileri okuyun
>>> shares_held = parse_csv('Data/portfolio.csv', select=['name','shares'])
>>> shares_held
[{'name': 'AA', 'shares': '100'}, {'name': 'IBM', 'shares': '50'}, {'name': 'CAT', 'shares': '150'}, {'name': 'MSFT', 'shares': '200'}, {'name': 'GE', 'shares': '95'}, {'name': 'MSFT', 'shares': '50'}, {'name': 'IBM', 'shares': '100'}]
>>>
```

Bir sütun seçici örneği [Egzersiz 2.23](../02_Working_with_data/06_List_comprehension)'de verilmişti.
Bunu yapmanın bir yolu şudur:


```python
# fileparse.py
import csv

def parse_csv(filename, select=None):
    '''
    Parse a CSV file into a list of records
    '''
    with open(filename) as f:
        rows = csv.reader(f)

        # Dosya başlıklarını okuyun
        headers = next(rows)

        # Bir sütun seçici verilmişse, belirtilen sütunların dizinlerini bulun.
        # Ayrıca ortaya çıkan dictionaryler için kullanılan üstbilgi kümesini daraltın
        if select:
            indices = [headers.index(colname) for colname in select]
            headers = select
        else:
            indices = []

        records = []
        for row in rows:
            if not row:    # Veri içermeyen satırları atla
                continue
            # Belirli sütunlar seçilmişse satırı filtreleyin
            if indices:
                row = [ row[index] for index in indices ]

            # dictionary yap
            record = dict(zip(headers, row))
            records.append(record)

    return records
```

Bu bölüm için bir dizi aldatıcı bit var. Muhtemelen en önemlisi, sütun seçimlerinin satır indekslerine 
eşlenmesidir. Örneğin, girdi dosyasının aşağıdaki başlıklara sahip olduğunu varsayalım:

```python
>>> headers = ['name', 'date', 'time', 'shares', 'price']
>>>
```

Şimdi, seçilen sütunların aşağıdaki gibi olduğunu varsayalım:

```python
>>> select = ['name', 'shares']
>>>
```

Doğru seçimi gerçekleştirmek için, seçili sütun adlarını dosyadaki sütun dizinleriyle eşlemeniz 
gerekir. Bu adımın yaptığı şey bu:

```python
>>> indices = [headers.index(colname) for colname in select ]
>>> indices
[0, 3]
>>>
```

Başka bir deyişle, "names" sütun 0 ve "shares" sütun 3'tür. Dosyadan bir veri satırı 
okuduğunuzda, dizinler onu filtrelemek için kullanılır:

```python
>>> row = ['AA', '6/11/2007', '9:50am', '100', '32.20' ]
>>> row = [ row[index] for index in indices ]
>>> row
['AA', '100']
>>>
```

### Egzersiz 3.5: Tür Dönüştürme

`parse_csv()` fonksiyonunu isteğe bağlı olarak, döndürülen verilere tür dönüştürmelerinin uygulanmasına
izin verecek şekilde değiştirin. Örneğin:


```python
>>> portfolio = parse_csv('Data/portfolio.csv', types=[str, int, float])
>>> portfolio
[{'price': 32.2, 'name': 'AA', 'shares': 100}, {'price': 91.1, 'name': 'IBM', 'shares': 50}, {'price': 83.44, 'name': 'CAT', 'shares': 150}, {'price': 51.23, 'name': 'MSFT', 'shares': 200}, {'price': 40.37, 'name': 'GE', 'shares': 95}, {'price': 65.1, 'name': 'MSFT', 'shares': 50}, {'price': 70.44, 'name': 'IBM', 'shares': 100}]

>>> shares_held = parse_csv('Data/portfolio.csv', select=['name', 'shares'], types=[str, int])
>>> shares_held
[{'name': 'AA', 'shares': 100}, {'name': 'IBM', 'shares': 50}, {'name': 'CAT', 'shares': 150}, {'name': 'MSFT', 'shares': 200}, {'name': 'GE', 'shares': 95}, {'name': 'MSFT', 'shares': 50}, {'name': 'IBM', 'shares': 100}]
>>>
```

Bunu zaten [Egzersiz 2.24](../02_Working_with_data/07_Objects)'de keşfetmiştiniz.
Çözümünüze aşağıdaki kod parçasını eklemeniz gerekecek:

```python
...
if types:
    row = [func(val) for func, val in zip(types, row) ]
...
```

### Egzersiz 3.6: Header Olmadan Çalışma

Bazı CSV dosyaları herhangi bir header bilgisi içermez.
Örneğin, `prices.csv` dosyası böyle görünür:

```csv
"AA",9.22
"AXP",24.85
"BA",44.85
"BAC",11.27
...
```

`parse_csv()` fonksiyonunu, bir tuple listesi oluşturarak bu tür dosyalarla çalışabilecek
şekilde değiştirin. Örneğin:

```python
>>> prices = parse_csv('Data/prices.csv', types=[str,float], has_headers=False)
>>> prices
[('AA', 9.22), ('AXP', 24.85), ('BA', 44.85), ('BAC', 11.27), ('C', 3.72), ('CAT', 35.46), ('CVX', 66.67), ('DD', 28.47), ('DIS', 24.22), ('GE', 13.48), ('GM', 0.75), ('HD', 23.16), ('HPQ', 34.35), ('IBM', 106.28), ('INTC', 15.72), ('JNJ', 55.16), ('JPM', 36.9), ('KFT', 26.11), ('KO', 49.16), ('MCD', 58.99), ('MMM', 57.1), ('MRK', 27.58), ('MSFT', 20.89), ('PFE', 15.19), ('PG', 51.94), ('T', 24.79), ('UTX', 52.61), ('VZ', 29.26), ('WMT', 49.74), ('XOM', 69.35)]
>>>
```

Bu değişikliği yapmak için, ilk veri satırının header satırı olarak yorumlanmaması adına kodu 
değiştirmeniz gerekir. Ayrıca, artık anahtarlar için kullanılacak sütun adları olmadığından 
dictionary oluşturmadığınızdan emin olmanız gerekir.

### Egzersiz 3.7: Farklı Bir Sütun Sınırlayıcı Seçme

CSV dosyaları oldukça yaygın olsa da, sekme veya boşluk gibi farklı bir sütun ayırıcı kullanan 
bir dosyayla karşılaşmanız da mümkündür. Örneğin, `Data/portfolio.dat` dosyası şöyle görünür:   

```csv
name shares price
"AA" 100 32.20
"IBM" 50 91.10
"CAT" 150 83.44
"MSFT" 200 51.23
"GE" 95 40.37
"MSFT" 50 65.10
"IBM" 100 70.44
```

`csv.reader()` fonksiyonu, aşağıdaki gibi farklı bir sütun sınırlayıcının verilmesine izin verir:

```python
rows = csv.reader(f, delimiter=' ')
```

`parse_csv()` fonksiyonunuzu, sınırlayıcının değiştirilmesine de izin verecek şekilde değiştirin.

Örneğin:

```python
>>> portfolio = parse_csv('Data/portfolio.dat', types=[str, int, float], delimiter=' ')
>>> portfolio
[{'price': '32.20', 'name': 'AA', 'shares': '100'}, {'price': '91.10', 'name': 'IBM', 'shares': '50'}, {'price': '83.44', 'name': 'CAT', 'shares': '150'}, {'price': '51.23', 'name': 'MSFT', 'shares': '200'}, {'price': '40.37', 'name': 'GE', 'shares': '95'}, {'price': '65.10', 'name': 'MSFT', 'shares': '50'}, {'price': '70.44', 'name': 'IBM', 'shares': '100'}]
>>>
```

### Yorum

Buraya kadar geldiyseniz, gerçekten kullanışlı olan güzel bir kitaplık işlevi oluşturmuşsunuzdur.
Dosyaların veya csv modülünün iç işleyişi hakkında çok fazla endişelenmenize gerek kalmadan 
rastgele `csv` dosyalarını ayrıştırmak, ilgili sütunları seçmek, tür dönüştürmeleri yapmak için 
kullanabilirsiniz.

[Contents](../Contents.md) \| [Previous (3.1 Scripting)](01_Script.md) \| [Next (3.3 Error Checking)](03_Error_checking.md)
