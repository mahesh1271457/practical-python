[Contents](../Contents.md) \| [Previous (1.6 Files)](06_Files.md) \| [Next (2.0 Working with Data)](../02_Working_with_data/00_Overview.md)

# 1.7 Fonksiyonlar

Bir programcı olarak daha büyüğünü istiyorsanız, düzenli olmayı isteyebilirsiniz.  
Bu bölümde modüller ve kütüphanelerden bahsedeceğiz.
İstisnalar ve hata işlemeyi de tanıtacağız.

###  Özel Fonksiyonlar

Bir fonksiyonu kodunuzda tekrar kullanmak isteyebilirsiniz. İşte fonksiyon tanımlama:

```python
def sumcount(n):
    '''
    ilk n tane sayının toplamını döndürür
    '''
    total = 0
    while n > 0:
        total += n
        n -= 1
    return total
```

Fonksiyonu çağırmak için.

```python
a = sumcount(100)
```
Bir fonksiyon bir dizi işlemi gerçekleştiren ve bir sonuç döndüren bir dizi ifadedir.
`return` anahtar sözcüğü fonksiyonun dönüş değerini açıkça belirtmek için gereklidir.

### Kütüphane Fonksiyonları

Python, büyük bir standart kütüphane ile gelir.
Kütüphane modüllerine erişmek için `import` kullanılır.
Örneğin:

```python
import math
x = math.sqrt(10)

import urllib.request
u = urllib.request.urlopen('http://www.python.org/')
data = u.read()
```

Kütüphaneleri ve modülleri daha sonra daha ayrıntılı olarak ele alacağız.

### Hatalar ve İstisnalar

Fonksiyonlar, hataları istisna olarak bildirir. Bir istisna, bir işlevin durdurulmasına
neden olup tüm programınızın durmasına neden olabilir.

Bunu kendi python REPL'nizde deneyin.

```python
>>> int('N/A')
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
ValueError: invalid literal for int() with base 10: 'N/A'
>>>
```


Hata ayıklama amacıyla bu mesaj neler olduğunu açıklıyor, hatanın nerede gerçekleştiğini
ve diğer başarısızlığa yol açan fonksiyonları gösteren bir geri bildirim gerçekleştiriyor.

### İstisnaları Yakalama ve Yönetme

İstisnalar yakalanabilir ve ele alınabilir.

Yakalamak için `try - except` ifadesini kullanırız.

```python
for line in f:
    fields = line.split()
    try:
        shares = int(fields[1])
    except ValueError:
        print("Couldn't parse", line)
    ...
```

`ValueError` yakalamaya çalıştığınız hatayla eşleşmelidir .

Ne tür hataların meydana gelebileceğini ve gerçekleştirilmekte olan 
işleme bağlı olarak tam olarak bilmek genellikle zordur. Daha iyi veya kötü, 
istisna işleme genellikle program beklenmedik bir şekilde çöktükten sonra eklenir.
 (i.e., "oh hayır,hatayı yakalamayı unuttuk. Bunu halletmeliyiz!").

### İstisnaları Fırlatmak

Bir istisnayı fırlatmak için  `raise` ifadesini kullanırız.

```python
raise RuntimeError('What a kerfuffle')
```

Bu programın bir istisna ile geri dönüşünün iptaline neden olur,
tabi bir `try-except` bloğu tarafından yakalanmadıkça.

```bash
% python3 foo.py
Traceback (most recent call last):
  File "foo.py", line 21, in <module>
    raise RuntimeError("What a kerfuffle")
RuntimeError: What a kerfuffle
```

## Alıştırmalar

### Alıştırma 1.29: Bir fonksiyon tanımlama

Basit bir fonksiyon tanımlamaya çalışalım:

```python
>>> def greeting(name):
        'Issues a greeting'
        print('Hello', name)

>>> greeting('Guido')
Hello Guido
>>> greeting('Paula')
Hello Paula
>>>
```

Bir fonksiyonun ilk ifadesi dizeyse dokümantasyon görevi görür.
Görüntülemek için `help(greeting)` gibi bir fonksiyon yazmayı deneyin.

### Alıştırma 1.30: Bir betiği(script) işleve dönüştürmek

`pcost.py` için yazdığınız kodu alalım [Alıştırma 1.27](06_Files.md)
ve içine `portfolio_cost(filename)` diye bir fonksiyon yazalım. Bu fonksiyon dosya adını girdi olarak alsın, içindeki 
portfölye verisini okusun ve toplam maliyeti float olarak döndürsün.

Fonksiyonu kullanmak için, programı değiştirelim ve aşağıdaki gibi görünsün:

```python
def portfolio_cost(filename):
    ...
    # Your code here
    ...

cost = portfolio_cost('Data/portfolio.csv')
print('Total cost:', cost)
```

Bu programı çalıştırdığında önceki seferkiyle aynı sonucu elde ediceksin.
Programı çalıştırdıktan sonra ,fonksiyonunuzu yazarak etkileşimli olarak çağırabilirsiniz:

```bash
bash $ python3 -i pcost.py
```

Bu size fonksiyonunuzu etkileşimli moddan çağırmanıza izin verecektir.

```python
>>> portfolio_cost('Data/portfolio.csv')
44671.15
>>>
```

Kodunuzla etkileşimli olarak denemeler yapabilecek ve test ve hata ayıklamanızı sağlayacaktır.

### Alıştırma 1.31: Hata Yönetimi

İçerisinde bazı eksik alanlar olan dosyayı fonksiyonunuzda denerseniz ne olur?

```python
>>> portfolio_cost('Data/missing.csv')
Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
    File "pcost.py", line 11, in portfolio_cost
    nshares    = int(fields[1])
ValueError: invalid literal for int() with base 10: ''
>>>
```

Bu noktada bir karar vermek durumundasınız. Programınızın çalışmasını sağlamak için
ya orjinal girdi dosyanızı bozuk satırlarından arındırırsınız ya da kodunuzu bozuk satırları 
işlemek için değiştirirsiniz.

`pcost.py`ı düzenleyip, programın istisnaları yakalamasını ve bir uyarı mesajı yazdırmasını,
dosyanın diğer kısımlarında işlemlerine devam etmesini sağlayın.

### Alıştırma 1.32: Bir kütüphane fonksiyonunu kullanmak

Python bir sürü kullanışlı fonksiyon barındıran büyük bir standart kütüphaneye sahiptir.
Bize burda yararlı olabilecek 'CSV' modülüdür. CSV veri dosyalarıyla çalışıyorsanız bunu kullanmalısınız.
İşte nasıl çalıştığına dair bir örnek:

```python
>>> import csv
>>> f = open('Data/portfolio.csv')
>>> rows = csv.reader(f)
>>> headers = next(rows)
>>> headers
['name', 'shares', 'price']
>>> for row in rows:
        print(row)

['AA', '100', '32.20']
['IBM', '50', '91.10']
['CAT', '150', '83.44']
['MSFT', '200', '51.23']
['GE', '95', '40.37']
['MSFT', '50', '65.10']
['IBM', '100', '70.44']
>>> f.close()
>>>
```

CSV modülü ile ilgili güzel bir şey çeşitli alıntı ve uygun virgül 
ayırma gibi düşük seviyeli işlerdir. Yukarıdaki çıktıda, çift tırnak 
işaretlerini ilk sütundaki adlardan ayırdığını göreceksiniz.

 `pcost.py` programını `csv` modülünü kullanarakayrıştırma için değiştirin ve
önceki örnekleri çalıştırmayı deneyin.

### Alıştırma 1.33: Komut satırından okuma

 `pcost.py` programında dosyanın adı doğrudan koda bağlanmıştır:

```python
# pcost.py

def portfolio_cost(filename):
    ...
    # Your code here
    ...

cost = portfolio_cost('Data/portfolio.csv')
print('Total cost:', cost)
```

Bu öğrenme aşaması ve test için yeterlidir, fakat gerçek programlarda böyle birşeyi 
istemeyebilirsiniz.

Bunun yerine dosyanın adını bir argüman olarak iletebilirsiniz. 
Programın alt kısmını aşağıdaki şekilde değiştirmeyi deneyin:

```python
# pcost.py
import sys

def portfolio_cost(filename):
    ...
    # Your code here
    ...

if len(sys.argv) == 2:
    filename = sys.argv[1]
else:
    filename = 'Data/portfolio.csv'

cost = portfolio_cost(filename)
print('Total cost:', cost)
```

`sys.argv` komut satırındaki (varsa) bağımsız değişkenler listeler.

Programınızı Python terminalinden çalıştırmanız gerekir.

Örnek olarak, Unix üzerindeki bash:

```bash
bash % python3 pcost.py Data/portfolio.csv
Total cost: 44671.15
bash %
```

[Contents](../Contents.md) \| [Previous (1.6 Files)](06_Files.md) \| [Next (2.0 Working with Data)](../02_Working_with_data/00_Overview.md)
