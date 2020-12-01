[Contents](../Contents.md) \| [Previous (1.5 Lists)](05_Lists.md) \| [Next (1.7 Functions)](07_Functions.md)

# 1.6 Dosya Yönetimi

Çoğu programlar okumak için bir yerlerden girdiye(input) almaya ihtiyaç duyar. Bu bölümde dosya erişimi anlatılmaktadır.

### Dosya Girdi Ve Çıktısı

Bir dosyayı açalım.

```python
f = open('foo.txt', 'rt')     # Okumak için açıyoruz (text)
g = open('bar.txt', 'wt')     # Yazmak için açıyoruz (text)
```

Tüm veriyi oku.

```python
data = f.read()

# Sadece 'maxbytes' a kadar olan baytları oku
data = f.read([maxbytes])
```

Biraz yazı yaz.

```python
g.write('buraya herhangi birşey yazabiliriz')
```

İşimiz bittiğinde kapatalım.

```python
f.close()
g.close()
```

Dosyaları düzgün bir biçimde kapatmalıyız ve bu unutulması kolay bir adımdır.
Bu nedenle tercih edilen yaklaşım ,`with` ifadesi ile aşağıdaki şekilde kullanılmaktadır.

```python
with open(filename, 'rt') as file:
    # Dosyayı `file` şeklinde kullanalım
    ...
    # Açıkça kapatmaya gerek yok.
...statements
```

Bu yaklaşım ,dosyayı girintili kod bloğunu terk ettiğinde otomatik olarak kapatacaktır.

### Dosya Verilerini Okumak İçin Yaygın Kullanımlar

Tüm dosyayı bir 'string' şeklinde okumak.

```python
with open('foo.txt', 'rt') as file:
    data = file.read()
    # `data` değişkeni `foo.txt` dosyasındaki tüm yazıları içeren bir string tipinde bir değişkendir
```

Bir dosyayı satır-satır okumak.

```python
with open(filename, 'rt') as file:
    for line in file:
        # İşlem satırı
```

### Dosyaya Yazmak İçin Yaygın Kullanımlar 

String verisi yazmak.

```python
with open('outfile', 'wt') as out:
    out.write('Hello World\n')
    ...
```

print fonksiyonunu yeniden yönlendirin.

```python
with open('outfile', 'wt') as out:
    print('Hello World', file=out)
    ...
```

## Alıştırmalar

Bu alıştırmaları  `Data/portfolio.csv` dosyasıyla yapacağız. Bu dosya,
hisse senedi portföyü hakkında bilgi içeren satırların bir listesini içerir.
`practical-python/Work/` bu dizinde çalıştığınız varsayılmaktadır.
Eğer emin değil iseniz, aşağıdaki kod bloğunu çalıştırarak 
hangi dizinde olduğunuzu bulabilirsiniz:

```python
>>> import os
>>> os.getcwd()
'/Users/beazley/Desktop/practical-python/Work' # bu çıktı sizde farklılık gösterecektir
>>>
```

### Alıştırma 1.26: Dosya Ön Hazırlıkları

Öncelikle tüm dosyayı büyük bir string olarak okuyalım:

```python
>>> with open('Data/portfolio.csv', 'rt') as f:
        data = f.read()

>>> data
'name,shares,price\n"AA",100,32.20\n"IBM",50,91.10\n"CAT",150,83.44\n"MSFT",200,51.23\n"GE",95,40.37\n"MSFT",50,65.10\n"IBM",100,70.44\n'
>>> print(data)
name,shares,price
"AA",100,32.20
"IBM",50,91.10
"CAT",150,83.44
"MSFT",200,51.23
"GE",95,40.37
"MSFT",50,65.10
"IBM",100,70.44
>>>
```
Bu örnekte, Python iki farklı şekilde çıktı verebilir. 
İlk olarak komut istemine `data` yazdığınızda, Python
size tırnak işaretleri ve kaçış dizilerinin de dahil olduğu ham dizeyi gösterir.
`print(data)` şeklinde yazdığınızda ise, biçimlendirilmiş bir dizenin çıktısını
size verir.

Bir dosyanın hepsini tek seferde okumak basit olsa da genellikle bunu yapmanın en uygun 
yolu değildir , özellikle de dosya çok büyük veya tek seferde işlem yapmak 
istediğiniz satırları içerdiği zaman.

Bir dosyayı satır-satır okumak için aşağıdaki örnekteki gibi for-loop (for döngüsü) kullanırız:

```python
>>> with open('Data/portfolio.csv', 'rt') as f:
        for line in f:
            print(line, end='')

name,shares,price
"AA",100,32.20
"IBM",50,91.10
...
>>>
```

Bu kodu kullandığınızda ,satırlar dosyanın sonuna kadar okunur ve
bu noktada da döngü durur.

Belirli durumlarda, bir satırı atlamak veya sadece belirli bir satırı okumak isteyebilirsiniz
(Örneğin sütunların başlıklarının olduğu satırı atlamak isteyebilirsiniz.).

```python
>>> f = open('Data/portfolio.csv', 'rt')
>>> headers = next(f) 
>>> headers
'name,shares,price\n'
>>> for line in f:
    print(line, end='')

"AA",100,32.20
"IBM",50,91.10
...
>>> f.close()
>>>
```

`next()` dosyada bulunan bir sonraki satırdaki yazıyı döner. Tekrar tekrar kullanırsak ardışık satırları elde ederdik.
Ancak bildiğiniz gibi for döngüsü verilerinizi elde etmek için `next()` kullanır.
Bu nedenle, tek bir satırı açıkça atlamaya veya okumaya çalışmadığınız sürece normalde onu doğrudan kullanmazsınız.

Bir dosyanın satırlarını okuduktan sonra ayırma gibi daha fazla işlemleri uygulamaya başlayabilirsiniz.
Örnek olarak şunu deneyelim:

```python
>>> f = open('Data/portfolio.csv', 'rt')
>>> headers = next(f).split(',')
>>> headers
['name', 'shares', 'price\n']
>>> for line in f:
    row = line.split(',')
    print(row)

['"AA"', '100', '32.20\n']
['"IBM"', '50', '91.10\n']
...
>>> f.close()
```

*Not: Bu örneklerde `f.close()` kullanıyoruz çünkü `with` ifadesini kullanmadık.*

### Alıştırma 1.27: Veri Dosyasını Okumak

Artık bir dosyanın nasıl okunacağını bildiğinize göre, basit bir hesaplama yapmak için bir program yazalım.

`portfolio.csv` dosyasındaki sütunlar hisse senedi isimlerini, payların sayısına,
ve tek bir hisse senedinin fiyatını tutar. `pcost.py` adında bir program yazalım, bu dosyayı açsın,
tüm satırları okusun ve portföydeki tüm payların ne kadar tutacağını hesaplasın.

*İpucu: string bir ifadeyi integer a çevirmek için `int(s)` kullanın. stringi bir ifadeyi float a çevirmek için `float(s)`.*

Programınız aşağıdaki gibi bir çıktı yazdırmalıdır:

```bash
Toplam maliyet 44671.15
```

### Alıştırma 1.28: Diğer Dosya Türleri

Ya text dosyası yerine gzip gibi sıkıştırılmış bir veri dosyası okumak isteseydik?
`open()` fonksiyonu bu sefer sana yardımcı olamazdı, ama
Python `gzip` adında bir kütüphane, sıkıştırlımış gzip dosyalarını okumana 
yardımıcı olur.

Şunu deneyelim:

```python
>>> import gzip
>>> with gzip.open('Data/portfolio.csv.gz', 'rt') as f:
    for line in f:
        print(line, end='')

... çıktıya bakın ...
>>>
```

Not:`'rt'` dosya modunun dahil edilmesi burda kritik önem taşır. Eğer bunu unutursan,
normal metin dizeleri yerine bayt dizeleri elde edersin.

### Yorum: Bunun için Pandas kullanmamız gerekmez mi?

Veri Bilimciler bu noktada hızlı kütüphaneleri tercih ederler,örneğin
[Pandas](https://pandas.pydata.org) CSV dosyalarını okumak için halihazırda bir fonksiyon içeriyor.
Bu doğru ve gerçekten güzel çalışıyor.
Ancak ,bu Pandas öğrenmek için bir kurs değil. Dosyaları okumak,
CSV dosyalarından daha genel bir problemdir.
Bir CSV dosyasıyla çalışmamızın ana nedeni, çoğu kodlayıcı için bilindik bir format
olması ve doğrudan Python özellikleriyle çalışmanın kolay olmasıdır.
Sonuç olarak işinizde elbette Pandas kullanın. Bu kursun geri kalanı için biz standart
Python işlevselliğine sadık kalacağız.

[Contents](../Contents.md) \| [Previous (1.5 Lists)](05_Lists.md) \| [Next (1.7 Functions)](07_Functions.md)
