[Contents](../Contents.md) \| [Previous (6.1 Iteration Protocol)](01_Iteration_protocol.md) \| [Next (6.3 Producer/Consumer)](03_Producers_consumers.md)

# 6.2 Iteration Özelleştirme

Bu bölümde generator fonksiyonunu kullanarak iteration özelleştirmeyi göreceğiz.

### Bir problem

Özel iteration kalıbınızı yapmanız gerekebilir.

Örnek olarak, bir geri sayım.

```python
>>> for x in countdown(10):
...   print(x, end=' ')
...
10 9 8 7 6 5 4 3 2 1
>>>
```

Bu yapmanın kolay bir yolu vardır.

### Generators 

Generator ,iteration tanımlayan bir fonksiyondur.

```python
def countdown(n):
    while n > 0:
        yield n
        n -= 1
```

Örneğin:

```python
>>> for x in countdown(10):
...   print(x, end=' ')
...
10 9 8 7 6 5 4 3 2 1
>>>
```

Generator `yield` ifadesini kullanan herhangi bir fonksiyondur.

Generators davranışı normal bir fonksiyondan farklıdır.
Generator fonksiyonunu çağırmak ,bir generator objesi oluşturur. Bu 
fonksiyonu hemen çalıştırmaz

```python
def countdown(n):
    # Added a print statement
    print('Counting down from', n)
    while n > 0:
        yield n
        n -= 1
```

```python
>>> x = countdown(10)
# There is NO PRINT STATEMENT
>>> x
# x is a generator object
<generator object at 0x58490>
>>>
```

Fonksiyon sadece `__next__()` çağrısından sonra çalışır.

```python
>>> x = countdown(10)
>>> x
<generator object at 0x58490>
>>> x.__next__()
Counting down from 10
10
>>>
```

`yield` bir değer üretir ama fonksiyonun yürütülmesini askıya alır.
Fonksiyon sonraki  `__next__()` çağrısında devam eder.

```python
>>> x.__next__()
9
>>> x.__next__()
8
```

Generator sonuncu kez döndüğünde,iteration hataya neden olur.

```python
>>> x.__next__()
1
>>> x.__next__()
Traceback (most recent call last):
File "<stdin>", line 1, in ? StopIteration
>>>
```

*Gözlem: Generator fonksiyonu ,liste ,tuple ,sözlük ,dosyalar vb. ifadelerde for ifadesi gibi
düşük seviyeli protokol uygular.*

## Alıştırmalar

### Alıştırma 6.4: Basit Bir Generator

Eğer iteration özelleştirmek ile ilgili bişeyler arıyorsanız,her zaman 
generator fonksiyonunu düşünmelisiniz.  İşte yazması kolay ---istenilen iteration 
mantığını gerçekleştiren bir fonksiyon ve değerleri yaymak için  `yield`
ifadesi.

Örneğin, bu generator'ı deneyin.Dosyayı arayan ve satırları alt dize olarak eşleyen bir generator:

```python
>>> def filematch(filename, substr):
        with open(filename, 'r') as f:
            for line in f:
                if substr in line:
                    yield line

>>> for line in open('Data/portfolio.csv'):
        print(line, end='')

name,shares,price
"AA",100,32.20
"IBM",50,91.10
"CAT",150,83.44
"MSFT",200,51.23
"GE",95,40.37
"MSFT",50,65.10
"IBM",100,70.44
>>> for line in filematch('Data/portfolio.csv', 'IBM'):
        print(line, end='')

"IBM",50,91.10
"IBM",100,70.44
>>>
```

Bu biraz ilginç bir fikir, bir parça özel fonksiyonunuzu  saklayabilir ve 
for-loop u beslemek için kullanabilirsiniz.
Sonraki örnek daha alışılmamış olacak.

### Alıştırma 6.5: Akış halindeki veri kaynağını izleme

Generators gerçek zamanlı veri kaynaklarını izlemenin ilginç bir yolu olabilir,örneğin
log dosyaları veya hisse senet fiyatları gibi. Bu bölümde,bu fikri göreceğiz.  
Başlamak için sonraki talimatları dikkatlice izleyin.

`Data/stocksim.py` programı borsayı simile edecektir.Çıktı olarak,gerçek zamanlı olarak `Data/stocklog.csv` 
dosyasına veriyi yazacaktır.  Ayrı iki komut penceresinde `Data/`dizine gidip programı çalıştırınız:

```bash
bash % python3 stocksim.py
```

Eğer windows üzerindeyseniz , `stocksim.py` programını bulun ve
çift tıklayarak çalıştırın.  Şimdi programı unutun. (sadece çalıştırın). 
Başka bir pencerede, `Data/stocklog.csv` dosyasına bakın ,dosyayı simüle etmeye başlamış mı. 
Her saniye yeni satırlara yazılar eklendiğini göreceksiniz.  Tekrar,
programı arkaplanda çalıştıralım---birkaç saat çalışacaktır.
(bunu dert etmenize gerek yok).

Yukarıdaki program çalıştıktan sonra, 
hadi dosyayı açan küçük bir program yazalım, sona kadar arayan ve 
yeni çıktıları gözlemleyen.  Yeni bir dosya oluşturalım `follow.py` ve  
aşağıdaki kodu içine koyalım:

```python
# follow.py
import os
import time

f = open('Data/stocklog.csv')
f.seek(0, os.SEEK_END)   # Move file pointer 0 bytes from end of file

while True:
    line = f.readline()
    if line == '':
        time.sleep(0.1)   # Sleep briefly and retry
        continue
    fields = line.split(',')
    name = fields[0].strip('"')
    price = float(fields[1])
    change = float(fields[4])
    if change < 0:
        print(f'{name:>10s} {price:>10.2f} {change:>10.2f}')
```

Programı çalıştrıdığınızda, gerçek zamanlı hisse senetlerini göreceksiniz.  Bunun altında,
bu kod bir çeşit Unix gibi `tail -f` komutuyla bir log dosyasını izlemek için kullanılır.

Not: Bu örnekte readline() yönteminin kullanılması, bir dosyadan satır okumanın olağan yolu olmadığı
için biraz sıra dışıdır (normalde sadece bir for-döngüsü kullanırsınız). Ancak bu durumda 'readline()''ı 
daha fazla veri eklenip eklenmediğini görmek için dosyanın sonunu tekrar tekrar incelemek için kullanıyoruz.
(readline() ya yeni veri ya da boş bir string döndürür).

### Alıştırma 6.6: Generator kullanarak veriyi indirgeme

Eğer Alıştırma 6.5'teki koda bakarsak, kodun ilk bölümü veri satırları üretirken 'while' döngüsünün sonundaki ifadeler verileri tüketiyor.,
Generator'un önemli bir özelliği, tüm veri üretim kodunu yeniden kullanılabilir bir fonskiyona taşıyabilmenizdir.

Alıştırma 6.5 daki kodunuzu bir generator fonksiyonu  `follow(filename)` tarafından dosya okuması için düzenleyin . 
Aşağıdaki  işlemleri yapın:

```python
>>> for line in follow('Data/stocklog.csv'):
          print(line, end='')

... Should see lines of output produced here ...
```

Hisse senedi kodunu böyle görünecek şekilde değiştirin:


```python
if __name__ == '__main__':
    for line in follow('Data/stocklog.csv'):
        fields = line.split(',')
        name = fields[0].strip('"')
        price = float(fields[1])
        change = float(fields[4])
        if change < 0:
            print(f'{name:>10s} {price:>10.2f} {change:>10.2f}')
```

### Alıştırma 6.7: Portföyünüzü İzleyin

follow.py programını, hisse senedi verilerinin akışını izleyecek ve yalnızca portföydeki 
hisse senetleri için bilgi gösteren bir kayan yazı şeridi yazdıracak şekilde değiştirin. Örneğin:

```python
if __name__ == '__main__':
    import report

    portfolio = report.read_portfolio('Data/portfolio.csv')

    for line in follow('Data/stocklog.csv'):
        fields = line.split(',')
        name = fields[0].strip('"')
        price = float(fields[1])
        change = float(fields[4])
        if name in portfolio:
            print(f'{name:>10s} {price:>10.2f} {change:>10.2f}')
```

 Not: Bunun işe yaraması için, sizin Portfolio class'ınız 'in' operatörünü desteklemek zorunda. [Alıştırma 6.3](01_Iteration_protocol) e
 bakın `__contains__()` operatörünü uyguladığından emin olun.

### Discussion

Az önce burada çok güçlü bir şey oldu. 
İlginç bir iteration modelini (bir dosyanın sonundaki satırları okuyarak) kendi küçük fonksiyonunuza
taşıdınız. follow() fonksiyonu  artık herhangi bir programda kullanabileceğiniz tamamen genel
amaçlı yardımcı programdır. Örneğin, server loglarını, debugging loglarını ve diğer benzer veri kaynaklarını izleyebilirsiniz.

[Contents](../Contents.md) \| [Previous (6.1 Iteration Protocol)](01_Iteration_protocol.md) \| [Next (6.3 Producer/Consumer)](03_Producers_consumers.md)