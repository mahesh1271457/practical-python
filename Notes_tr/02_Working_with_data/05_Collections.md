[Contents](../Contents.md) \| [Previous (2.4 Sequences)](04_Sequences.md) \| [Next (2.6 List Comprehensions)](06_List_comprehension.md)

# 2.5 Collections Module (koleksiyon modülü)

`collections` modulü veriyi ele almak için kullanışlı objeler sağlar.
Bu bölüm, bu özelliklerden bazılarını kısaca tanıtır.

### Örneğin: Bir şeyleri saymak

Her hisse senedinin ,toplam hisselerini tablo haline getirmek istediğinizi varsayalım.

```python
portfolio = [
    ('GOOG', 100, 490.1),
    ('IBM', 50, 91.1),
    ('CAT', 150, 83.44),
    ('IBM', 100, 45.23),
    ('GOOG', 75, 572.45),
    ('AA', 50, 23.15)
]
```

Burada iki tane `IBM` ve iki tane `GOOG` girdisi var . Bunlar bir şekilde birleştirilmelidir.

### Sayaçlar (Counter)

Çözüm: Bir sayaç(counter) kullanmak.

```python
from collections import Counter
total_shares = Counter()
for name, shares, price in portfolio:
    total_shares[name] += shares

total_shares['IBM']     # 150
```

### Örneğin: One-Many Mappings (Bir-Çok Eşleme)

Problem: Bir anahtarı(key) birden çok değerle eşlemek istiyorsunuz.

```python
portfolio = [
    ('GOOG', 100, 490.1),
    ('IBM', 50, 91.1),
    ('CAT', 150, 83.44),
    ('IBM', 100, 45.23),
    ('GOOG', 75, 572.45),
    ('AA', 50, 23.15)
]
```

Önceki örnekteki gibi,  `IBM` anahtarını iki farklı tuple içeriyordu.

Çözüm:  `defaultdict` kullanmak.

```python
from collections import defaultdict
holdings = defaultdict(list)
for name, shares, price in portfolio:
    holdings[name].append((shares, price))
holdings['IBM'] # [ (50, 91.1), (100, 45.23) ]
```

 `defaultdict` bir anahtara her eriştiğinizde varsayılan bir değer almanızı sağlar

### Örnek: Bir geçmiş tutmak

Problem: N tane şeyin önceki hallerini tutmak istiyoruz.
Çözüm: `deque` kullanın.

```python
from collections import deque

history = deque(maxlen=N)
with open(filename) as f:
    for line in f:
        history.append(line)
        ...
```

## Alıştırmalar

`collections` modulü tablo oluşturma ve indeksleme gibi veriyle ilgili 
problemlerde en yararlı kütüphanelerden biridir.

Bu alıştırmada, basit birkaç örneğe göz atacağız. `report.py` adlı programınızı çalıştırarak başlayın 
böylece hisse senedlerine ait portföy interaktif modda yüklenecektir.

```bash
bash % python3 -i report.py
```

### Aliştirma 2.18: Sayaçlarla Tablo Oluşturma

Her hisse senedinin toplam hisse sayısını tablo haline getirmek istediğinizi varsayalım.
 `Counter` objesini kullanarak kolay. Şunu deneyin:

```python
>>> portfolio = read_portfolio('Data/portfolio.csv')
>>> from collections import Counter
>>> holdings = Counter()
>>> for s in portfolio:
        holdings[s['name']] += s['shares']

>>> holdings
Counter({'MSFT': 250, 'IBM': 150, 'CAT': 150, 'AA': 100, 'GE': 95})
>>>
```

`MSFT` ve `IBM` nin birden fazla girişi olduğunu dikkatlice gözlemleyiniz.
`portfolio` 'de tek bir girişte birleştirilir.

Sözlük gibi bir tek tek değerleri almak için bir Sayaç(counter) kullanabilirsiniz:

```python
>>> holdings['IBM']
150
>>> holdings['MSFT']
250
>>>
```

Eğer değerleri sıralamak istiyorsanız, şunu deneyin:

```python
>>> # Get three most held stocks
>>> holdings.most_common(3)
[('MSFT', 250), ('IBM', 150), ('CAT', 150)]
>>>
```

Başka bir hisse senedi portföyü alalım ve yeni bir sayaç (Counter) yapalım:

```python
>>> portfolio2 = read_portfolio('Data/portfolio2.csv')
>>> holdings2 = Counter()
>>> for s in portfolio2:
          holdings2[s['name']] += s['shares']

>>> holdings2
Counter({'HPQ': 250, 'GE': 125, 'AA': 50, 'MSFT': 25})
>>>
```

Son olarak, basit bir işlem yaparak hepsini birleştirelim:

```python
>>> holdings
Counter({'MSFT': 250, 'IBM': 150, 'CAT': 150, 'AA': 100, 'GE': 95})
>>> holdings2
Counter({'HPQ': 250, 'GE': 125, 'AA': 50, 'MSFT': 25})
>>> combined = holdings + holdings2
>>> combined
Counter({'MSFT': 275, 'HPQ': 250, 'GE': 220, 'AA': 150, 'IBM': 150, 'CAT': 150})
>>>
```

Bu, sayaçların sağladığının sadece küçük bir tadıydı. Ancak siz kendiniz 
değerleri tablolamak isterseniz kullanmayı düşünmelisiniz.

### Yorum: collections module(koleksiyon modülü)

`collections` modulü tüm Pythondaki en kullanışlı kütüphanelerdendir. 
Gerçektende bu konu hakkında uzatılmış bir içerik yapabilirdik. 
Ancak, bunu yapmak şu an için dikkat dağıtıcı olacaktır. Şimdilik `collections` modülunü
yatmadan önce okunacaklara koyalım.

[Contents](../Contents.md) \| [Previous (2.4 Sequences)](04_Sequences.md) \| [Next (2.6 List Comprehensions)](06_List_comprehension.md)
