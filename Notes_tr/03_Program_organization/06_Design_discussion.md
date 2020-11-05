[Contents](../Contents.md) \| [Previous (3.5 Main module)](05_Main_module.md) \| [Next (4 Classes)](../04_Classes_objects/00_Overview.md)

# 3.6 Tasarım Tartışması

Bu bölümde, daha önce verilen bir tasarım kararını yeniden ele alacağız.

### Dosya Adları ve Yinelemeler

Aynı çıktıyı veren bu iki programı karşılaştırın.

```python
# Bir dosya adı sağlayın
def read_data(filename):
    records = []
    with open(filename) as f:
        for line in f:
            ...
            records.append(r)
    return records

d = read_data('file.csv')
```

```python
# Satırlar sağlayın
def read_data(lines):
    records = []
    for line in lines:
        ...
        records.append(r)
    return records

with open('file.csv') as f:
    d = read_data(f)
```

* Bu fonksiyonlardan hangisini tercih edersiniz? Neden?
* Bu fonksiyonlardan hangisi daha esnektir?

### Derin Fikir: "Duck Typing"

[Duck Typing](https://en.wikipedia.org/wiki/Duck_typing), bir nesnenin belirli bir
amaç için kullanılıp kullanılamayacağını belirleyen bir proglamlama konseptidir.
[duck test](https://en.wikipedia.org/wiki/Duck_test)'in bir uygulamasıdır.

> Ördeğe benziyorsa, ördek gibi yüzüyorsa ve ördek gibi vaklıyorsa, muhtemelen ördektir.

Yukarıdaki `read_data()`'nın ikinci versiyonunda, fonksiyon herhangi bir yinelenebilir nesneyi bekler.
Sadece bir dosyanın satırlarını değil.

```python
def read_data(lines):
    records = []
    for line in lines:
        ...
        records.append(r)
    return records
```

Bu, onu diğer *satırlarla* kullanabileceğimiz anlamına gelir.

```python
# CSV dosyası
lines = open('data.csv')
data = read_data(lines)

# Zip dosyası
lines = gzip.open('data.csv.gz','rt')
data = read_data(lines)

# Standard Input
lines = sys.stdin
data = read_data(lines)

# Dizelerin listesi
lines = ['ACME,50,91.1','IBM,75,123.45', ... ]
data = read_data(lines)
```

Bu tasarımda kaydadeğer bir esneklik var.

*Soru: Bu esnekliği benimsemeli miyiz yoksa onunla savaşmalı mıyız?*

### Kütüphane Tasarımında En İyi Uygulamalar

Kod kütüphaneleri, esnekliği benimseyerek genellikle daha iyi sunulur. Seçeneklerinizi kısıtlamayın.
Büyük esneklikle birlikte büyük güç gelir!

## Egzersizler

### Egzersiz 3.17: Dosya adlarından dosya benzeri nesnelere

Şimdi `parse_csv()` fonksiyonunu içeren bir `fileparse.py` dosyası oluşturdunuz.
Fonksiyon şu şekilde çalıştı:

```python
>>> import fileparse
>>> portfolio = fileparse.parse_csv('Data/portfolio.csv', types=[str,int,float])
>>>
```

Şu anda, fonksiyon bir dosya adı aktarılmasını bekliyor. Ancak, kodu daha esnek bir hale getirebilirsiniz.
Fonksiyonu herhangi bir dosya benzeri/yinelenebilir nesne ile çalışacak şekilde değiştirin. Örneğin:

```
>>> import fileparse
>>> import gzip
>>> with gzip.open('Data/portfolio.csv.gz', 'rt') as file:
...      port = fileparse.parse_csv(file, types=[str,int,float])
...
>>> lines = ['name,shares,price', 'AA,100,34.23', 'IBM,50,91.1', 'HPE,75,45.1']
>>> port = fileparse.parse_csv(lines, types=[str,int,float])
>>>
```

Bu yeni kodda, daha önce olduğu gibi bir dosya adı geçirirseniz ne olur?

```
>>> port = fileparse.parse_csv('Data/portfolio.csv', types=[str,int,float])
>>> port
... çıktıya bakın (çılgınca olmalı) ...
>>>
```

Evet, dikkatli olmalısın. Bundan kaçınmak için bir güvenlik kontrolü ekleyebilir misiniz?

### Egzersiz 3.18: Mevcut fonksiyonların düzeltilmesi

`report.py` dosyasındaki `read_portfolio()` ve `read_prices()` fonksiyonlarını, `parse_csv()`'nin değiştirilmiş
versiyonu ile çalışacak şekilde düzeltin. Bu sadece küçük bir değişiklik içermelidir. Daha sonra
`report.py` ve `pcost.py` programlarınız her zaman çalıştıkları gibi çalışmalıdır.


[Contents](../Contents.md) \| [Previous (3.5 Main module)](05_Main_module.md) \| [Next (4 Classes)](../04_Classes_objects/00_Overview.md)