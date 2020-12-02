[Contents](../Contents.md) \| [Previous (6.3 Producer/Consumer)](03_Producers_consumers.md) \| [Next (7 Advanced Topics)](../07_Advanced_Topics/00_Overview.md)

# 6.4 Daha Fazla Generator

Bu bölüm, generator ifadeleri ve itertools modülü dahil olmak üzere, generator’lar ile ilgili birkaç ek konu sunar.

### Generator İfadeleri

Bir list comprehension’un generator hali.

```python
>>> a = [1,2,3,4]
>>> b = (2*x for x in a)
>>> b
<generator object at 0x58760>
>>> for i in b:
...   print(i, end=' ')
...
2 4 6 8
>>>
```

List Comprehension’larla farkları:
*Liste oluşturmaz.
*Tek yararlı amaç yinelemedir.
*Bir kez tüketildikten sonra tekrar kullanılamaz.

Genel syntax.

```python
(<expression> for i in s if <conditional>)
```

Aynı zamanda bir fonksiyon argümanı olarak da hizmet edebilir.

```python
sum(x*x for x in a)
```

Herhangi bir iterable’a uygulanabilir.

```python
>>> a = [1,2,3,4]
>>> b = (x*x for x in a)
>>> c = (-x for x in b)
>>> for i in c:
...   print(i, end=' ')
...
-1 -4 -9 -16
>>>
```

Generator ifadelerinin ana kullanımı, bir dizi üzerinde bazı hesaplamalar gerçekleştiren, ancak sonucu yalnızca bir kez kullanan koddadır. Örneğin, bir dosyadaki tüm yorumları çıkarın.

```python
f = open('somefile.txt')
lines = (line for line in f if not line.startswith('#'))
for line in lines:
    ...
f.close()
```

Generator’larla kod daha hızlı çalışır ve az bellek kullanır. Bir akışa uygulanan filtre gibidir.

### Neden Generator’lar

* Birçok sorun yineleme açısından çok daha net ifade edilir.

  * Bir öğe koleksiyonu üzerinde döngü yapmak ve bir tür işlem gerçekleştirmek (arama, değiştirme, değiştirme vb.).
  
  * İşleme pipeline’ları, çok çeşitli veri işleme sorunlarına uygulanabilir.
  
* Daha iyi bellek verimliliği.

  * Yalnızca gerektiğinde değer üretir.
  
  * Dev listeler oluşturmanın aksidir.
  
  * Akış verileri üzerinde çalışabilir.
  
* Generator’lar kodun yeniden kullanımını teşvik eder.

  * iteration’ u yinelemeyi kullanan koddan ayırır.
  
  * İlginç yineleme fonksiyonlarından ve mix-n-match'tan oluşan bir araç kutusu oluşturabilirsiniz.

### `itertools` modülü

`itertools` iterators/generators’a yardımcı olarak tasarlanmış bir kütüphane modülüdür. 

```python
itertools.chain(s1,s2)
itertools.count(n)
itertools.cycle(s)
itertools.dropwhile(predicate, s)
itertools.groupby(s)
itertools.ifilter(predicate, s)
itertools.imap(function, s1, ... sN)
itertools.repeat(s, n)
itertools.tee(s, ncopies)
itertools.izip(s1, ... , sN)
```

Tüm fonksiyonlar verileri yinelemeli olarak işler. Çeşitli yineleme modellerini uygularlar.
PyCon '08'den [Sistem Programcıları için Generator Püf Noktaları](http://www.dabeaz.com/generators/) eğitiminde daha fazla bilgiye ulaşabilirsiniz.

##Alıştırmalar

Önceki alıştırmalarda, bir günlük dosyasına yazılan satırları izleyen ve bunları bir dizi satıra ayrıştıran bazı kodlar yazdınız. Bu egzersiz bunun üzerine inşa etmeye devam ediyor. `Data/stocksim.py`'nin hala çalıştığından emin olun.

###Alıştırma 6.13: Generator İfadeleri

Generator ifadeleri bir list comprehension’ın generator halidir. Örneğin:

```python
>>> nums = [1, 2, 3, 4, 5]
>>> squares = (x*x for x in nums)
>>> squares
<generator object <genexpr> at 0x109207e60>
>>> for n in squares:
...     print(n)
...
1
4
9
16
25
```

Bir list comprehension’ın aksine, bir generator ifadesi sadece bir kere kullanılabilir. Bu yüzden, başka bir for-loop denerseniz, hiçbir şey elde edemezsiniz:

```python
>>> for n in squares:
...     print(n)
...
>>>
```

### Alıştırma 6.14: Fonksiyon Argümanlarında Generator İfadeleri 

Generator ifadeleri bazen fonksiyon argümanlarına yerleştirilir. İlk başta biraz tuhaf görünüyor, ancak şu deneyi deneyin:

```python
>>> nums = [1,2,3,4,5]
>>> sum([x*x for x in nums])    # A list comprehension
55
>>> sum(x*x for x in nums)      # A generator expression
55
>>>
```

Yukarıdaki örnekte, generator kullanan ikinci versiyon, büyük bir liste işleniyorsa önemli ölçüde daha az bellek kullanırdı.
`portfolio.py` dosyanızda,  list comprehension içeren birkaç hesaplama yaptınız. Bunları generator ifadeleriyle değiştirmeyi deneyin.
 
### Alıştırma 6.15: Kodu basitleştirmek

Generator ifadeleri genellikle küçük generator fonksiyonlarının yerine geçebilir. Örneğin, şöyle bir fonksiyon yazmak yerine:

```python
def filter_symbols(rows, names):
    for row in rows:
        if row['name'] in names:
            yield row
```

Şöyle bir şey yazabilirsiniz:

```python
rows = (row for row in rows if row['name'] in names)
```

Generator ifadelerini uygun şekilde kullanmak için `ticker.py` programını değiştirin.

[Contents](../Contents.md) \| [Previous (6.3 Producer/Consumer)](03_Producers_consumers.md) \| [Next (7 Advanced Topics)](../07_Advanced_Topics/00_Overview.md)

