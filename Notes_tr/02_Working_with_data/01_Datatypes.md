[Contents](../Contents.md) \| [Previous (1.6 Files)](../01_Introduction/06_Files.md) \| [Next (2.2 Containers)](02_Containers.md)

# 2.1 Veri Tipleri ve Veri Yapıları

Bu bölümde veri yapılarından tuple'lar(demetler) ve sözlüklerden bahsedeceğiz.

### İlkel Veri Türleri

Python'da birkaç ilkel veri tipi vardır:

* Integers
* Floating point numbers
* Strings (text)

Biz bunları 1. bölümde görmüştük.

### None type (belirli bir tipi olmayanlar)

```python
email_address = None
```

`None` genellikle yer rezerve etmek için veya kayıp değerler olduğunda kullanılır.
Bu koşullu ifadelerde 'FALSE' olarak değerlendirilir.

```python
if email_address:
    send_email(email_address, msg)
```

### Veri Yapıları

Gerçek programlarda daha karmaşık veriler bulunur. Örneğin hisse senedleri ile ilgili bilgiler:

```code
100 shares of GOOG at $490.10
```

Bu nesne ("object") üç parçadan oluşuyor:

* Hissenin ismi veya sembolü olarak ("GOOG", string)
* Hissenin miktarı olarak (100, integer)
* Fiyat olarak (490.10 a float)

### Tuples(Demetler)

Tuple,değerleri gruplanmış şeklide tutan bir koleksiyondur.

Örneğin:

```python
s = ('GOOG', 100, 490.1)
```

Bazen `()` ifadesi syntax(söz dizimi)de atlanır.

```python
s = 'GOOG', 100, 490.1
```

Özel Durumlar (0-tuple, 1-tuple).

```python
t = ()            # boş tuple
w = ('GOOG', )    # 1 elemanlı tuple
```

Tuple genellikle  *basit* kayıtları veya yapıları temsil etmek için kullanılır .
Tipik olarak, bir *object* (obje)nin birden çok parçasını tutar. İyi bir benzetme: *Tuplelar veri tablosundaki tek bir satır gibidir*

Tuple içerikleri düzen içindedir.(arrayler gibi).

```python
s = ('GOOG', 100, 490.1)
name = s[0]                 # 'GOOG'
shares = s[1]               # 100
price = s[2]                # 490.1
```

Ancak,bu içerikler düzenlenemez.

```python
>>> s[1] = 75
TypeError: object does not support item assignment
```

Ancak yeni elaman ile beraber mevcut tuple'ı yeni bir tuple olarak tanımlayabilirsiniz.

```python
s = (s[0], 75, s[2])
```

### Tuple  Paketleme

Tuplelar ilgili öğeleri tek bir *varlık* ta birarada paketlemekle ilgilidir.

```python
s = ('GOOG', 100, 490.1)
```

Tuplelarda programın diğer bölümlerine tek bir nesne olarak ulaşmak kolaydır .

### Tuple Paketi Açma (Unpacking)

Tuplelar heryerde kullanılabilir, parçalarını değişkenlere ayırabilirsiniz.

```python
name, shares, price = s
print('Cost', shares * price)
```

Atayacağınız değişkenlerin sayısı,tuple'daki yapıların sayısıyla eşit olmalıdır.

```python
name, shares = s     # ERROR
Traceback (most recent call last):
...
ValueError: too many values to unpack
```

### Tuple vs. Liste

Tuplelar salt-okunur bir liste gibi görünebilir. Ancak, tuple'lar *tek bir öğenin* farklı parçaları için kullanılır.  
Listeler genellikle farklı öğelerden oluşan bir koleksiyondur (genellikle aynı tipli).

```python
record = ('GOOG', 100, 490.1)       # A tuple representing a record in a portfolio

symbols = [ 'GOOG', 'AAPL', 'IBM' ]  # A List representing three stock symbols
```

### Dictionaries (Sözlükler)

Sözlükler anahtar ve ona ait değerlerden oluşur.  Bazen, karma tablo (hash table) veya
ilişkilendirlebilir dizi  olarakta anılırlar(associative array). Anahtarlar indexler gibi değere erişmek için kullanılır.

```python
s = {
    'name': 'GOOG',
    'shares': 100,
    'price': 490.1
}
```

### Yaygın İşlemler

Sözlüklerde değeri(value) almak için anahtarı(key) kullanırız.

```python
>>> print(s['name'], s['shares'])
GOOG 100
>>> s['price']
490.10
>>>
```

Eklemek veya değiştirmek için değerin ,anahtar karşılığı kullanılır.

```python
>>> s['shares'] = 75
>>> s['date'] = '6/6/2007'
>>>
```

Değeri silmek için `del` ifadesi kullanılır.

```python
>>> del s['date']
>>>
```

### Neden Sözlükler?

Sözlükler *birçok* farklı değer ve bu değerler değiştirilebilir olması gerektiğinde kullanışlıdır .
Sözlükler kodunuzu daha okunabilir kılar.

```python
s['price']
# vs
s[2]
```

## Alıştırmalar

Son alıştırmalarda, `Data/portfolio.csv` dizisindeki veri ile çalıştık.
`csv` modülü ile veriyi sıra sıra okumak kolaydır.

```python
>>> import csv
>>> f = open('Data/portfolio.csv')
>>> rows = csv.reader(f)
>>> next(rows)
['name', 'shares', 'price']
>>> row = next(rows)
>>> row
['AA', '100', '32.20']
>>>
```

Genel olarak veriyi okumak kolaydır ama siz veriyi okumaktan daha fazlasını
isteyebilirsiniz. Örneğin bazen veriyi saklamak ve üzerinde işlemler yapmak isteyebilirsiniz.
Malesef ham bir "dizi" size çalışmak için yerince imkan sağlamayabilir.
Örneğin basit bir matematik hesaplaması bile çalışmıyor:

```python
>>> row = ['AA', '100', '32.20']
>>> cost = row[1] * row[2]
Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
TypeError: can't multiply sequence by non-int of type 'str'
>>>
```

Ham verilerle çalışmak için onları daha kullanışlı nesnelere çevirip daha sonra işleyebilirsiniz.
Burada iki farklı opsiyon bulunuyor, tuple ve sözlükler.

### Alıştırma 2.1: Tuples

İnteraktif komut satırında, aşağıdaki gibi bir tuple tanımlayalım,
ama numerik sütünlar uygun tiplere dönüştürülmeli:

```python
>>> t = (row[0], int(row[1]), float(row[2]))
>>> t
('AA', 100, 32.2)
>>>
```

Bunu kullanarak, hisse miktarı ve fiyatını kullanarak toplam maliyeti hesaplayabilirsiniz:

```python
>>> cost = t[1] * t[2]
>>> cost
3220.0000000000005
>>>
```

Python' da matematik hatalı mı? Neden cevap böyle birşey çıktı
3220.0000000000005?

Bu ,bilgisayarınnızdaki kayan nokta(float) donanımının bir sonucudur, 
ondalık sayıları 10 tabanı yerinine 2 tabanında kullandığı için.
10 tabanlı ondalık sayıları içeren basit hesaplamada bile,
küçük hatalar ortaya çıkar.Bu normal ancak daha önce 
görmediyseniz şaşırtıcı olabilir.

Bu, kayan nokta kullanan tüm programlama dillerinde olur
ama yazdırırken gizlenirler. Örneğin:

```python
>>> print(f'{cost:0.2f}')
3220.00
>>>
```

Tuple'lar salt okunurdur. Aşağıda veriyi değiştirmeye çalışarak bunu kanıtlayalım.

```python
>>> t[1] = 75
Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
TypeError: 'tuple' object does not support item assignment
>>>
```

Görüldüğü gibi tuple içerikleri değiştirilemez, sadece eski tuple dan yeni bir tuple
elde ederken içeriğini değiştireblirsiniz.

```python
>>> t = (t[0], 75, t[2])
>>> t
('AA', 75, 32.2)
>>>
```

Mevcut bir değişkenin adını bunun gibi yeniden atadığınızda, eski
değer ortadan kalkar.  Yukarıdaki örnekte size tuple'ın içeriğini değiştirdik gibi görünsede
aslında yeni bir tuple oluşturarak eskisini yok ediyoruz.
Tuplelar genellikle değişkenler paket halinde tutmak için kullanılır. Şunu deneyelim:

```python
>>> name, shares, price = t
>>> name
'AA'
>>> shares
75
>>> price
32.2
>>>
```

Yukarıdaki değişkenleri alın ve bir tuple'ın içine koyun.

```python
>>> t = (name, 2*shares, price)
>>> t
('AA', 150, 32.2)
>>>
```

### Alıştırma 2.2: Veri Yapısı Olarak Sözlükler

Tuplelara bir alternatif olarak sözlükler kulllanılabilir.

```python
>>> d = {
        'name' : row[0],
        'shares' : int(row[1]),
        'price'  : float(row[2])
    }
>>> d
{'name': 'AA', 'shares': 100, 'price': 32.2 }
>>>
```

Bunları tutarken hesaplamaları yapın:

```python
>>> cost = d['shares'] * d['price']
>>> cost
3220.0000000000005
>>>
```

Bu örneği önceki tuple örneği ile karşılaştırın. 
Hisse miktarını(shares) 75 ile değiştirin.

```python
>>> d['shares'] = 75
>>> d
{'name': 'AA', 'shares': 75, 'price': 32.2 }
>>>
```

Tupleların aksine sözlükler değiştirilebilirdir(modify). 
Biraz değişiklik ekleyin:

```python
>>> d['date'] = (6, 11, 2007)
>>> d['account'] = 12345
>>> d
{'name': 'AA', 'shares': 75, 'price':32.2, 'date': (6, 11, 2007), 'account': 12345}
>>>
```

### Alıştırma 2.3: Bazı sözlük işlemleri


Eğer sözlüğü bir tuple a dönüştürüseniz,tüm anahtar(key)leri elde edersiniz:

```python
>>> list(d)
['name', 'shares', 'price', 'date', 'account']
>>>
```

`for` ifadesini kullanarak yaparsanız yine aynı sonucu alırsınız:

```python
>>> for k in d:
        print('k =', k)

k = name
k = shares
k = price
k = date
k = account
>>>
```

Bu ifadeyi denerken yukardakiyle farkına bakın:

```python
>>> for k in d:
        print(k, '=', d[k])

name = AA
shares = 75
price = 32.2
date = (6, 11, 2007)
account = 12345
>>>
```

Tabi ki tüm anahtarları `keys()` metodu ile de elde edebilirsiniz:

```python
>>> keys = d.keys()
>>> keys
dict_keys(['name', 'shares', 'price', 'date', 'account'])
>>>
```

`keys()` biraz alışılmadık ifadedir, `dict_keys` öğelerini döndürürken.

Bu her zaman geçerli ,anahtar veya değeri değişse bile .
Örneğin şunu deneyelim:

```python
>>> del d['account']
>>> keys
dict_keys(['name', 'shares', 'price', 'date'])
>>>
```

Dikkat edin `'account'`  ,`keys`(anahtar) kısmından kaybolur 
eğer `d.keys()` i tekrar çağırmadıysanız.

Anahtar ve değerleriyle çalışmanın daha zarif bir yolu ,
`items()` metodudur. Bu size `(key, value)` tuple'nı verir:

```python
>>> items = d.items()
>>> items
dict_items([('name', 'AA'), ('shares', 75), ('price', 32.2), ('date', (6, 11, 2007))])
>>> for k, v in d.items():
        print(k, '=', v)

name = AA
shares = 75
price = 32.2
date = (6, 11, 2007)
>>>
```

`items` gibi bir tuplenız varsa, `dict()` fonksiyonunu kullanarak bir sözlük yapabilirsiniz.
Deneyelim :

```python
>>> items
dict_items([('name', 'AA'), ('shares', 75), ('price', 32.2), ('date', (6, 11, 2007))])
>>> d = dict(items)
>>> d
{'name': 'AA', 'shares': 75, 'price':32.2, 'date': (6, 11, 2007)}
>>>
```

[Contents](../Contents.md) \| [Previous (1.6 Files)](../01_Introduction/06_Files.md) \| [Next (2.2 Containers)](02_Containers.md)
