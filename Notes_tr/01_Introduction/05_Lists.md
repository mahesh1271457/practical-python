[Contents](../Contents.md) \| [Previous (1.4 Strings)](04_Strings.md) \| [Next (1.6 Files)](06_Files.md)

# 1.5 Listeler

Bu bölümde listeleri anlatacağız, Python'da değerleri düzenli ve bir arada olarak tutmak için kullanılır.

### Bir liste oluşturma

Liste oluştururken köşeli parantez kullanırız:

```python
names = [ 'Elwood', 'Jake', 'Curtis' ]
nums = [ 39, 38, 42, 65, 111]
```

Bazen listeler farklı metotlarda oluşturulabilir.  Örneğin, stringler split() fonksiyonu ile 
parçalanarak bir liste haline gelir :

```python
>>> line = 'GOOG,100,490.10'
>>> row = line.split(',')
>>> row
['GOOG', '100', '490.10']
>>>
```

### Liste operatörleri

Listeler herhangi tipte değerleri tutabilirler. `append()` ile listeye yeni elemanlar ekleyebiliriz:

```python
names.append('Murphy')    # sona ekler
names.insert(2, 'Aretha') # ortaya ekler
```

`+` ile listeleri birleştirebiliriz:

```python
s = [1, 2, 3]
t = ['a', 'b']
s + t           # [1, 2, 3, 'a', 'b']
```

Listeler integerlar ile indexlenir . 0 'dan başlayarak.

```python
names = [ 'Elwood', 'Jake', 'Curtis' ]

names[0]  # 'Elwood'
names[1]  # 'Jake'
names[2]  # 'Curtis'
```

Negatif indexler sondan saymaya başlar.

```python
names[-1] # 'Curtis'
```

Listedeki elemanları değiştirebilirsiniz.

```python
names[1] = 'Joliet Jake'
names                     # [ 'Elwood', 'Joliet Jake', 'Curtis' ]
```

Listenin uzunluğu.

```python
names = ['Elwood','Jake','Curtis']
len(names)  # 3
```

Üyelik testi (dahil olma durumu) (`in`, `not in`).

```python
'Elwood' in names       # True
'Britney' not in names  # True
```

Çoğaltma (`s * n`).

```python
s = [1, 2, 3]
s * 3   # [1, 2, 3, 1, 2, 3, 1, 2, 3]
```

### Listede Iterasyon ve Arama

Listenin elemanlarına erişmek için `for` ifadesini kullabiliriz.

```python
for name in names:
    # use name
    # e.g. print(name)
    ...
```

Bu diğer programlama dillerindeki `foreach` ifadesine benzer.

Bir ifadenin hızlıca konumuna erişmek için `index()` kullanın.

```python
names = ['Elwood','Jake','Curtis']
names.index('Curtis')   # 2
```

Eğer aradığımız eleman birden fazla varsa, `index()` fonksiyonu ilk sıradakini gösterir.

Eğer eleman bulunmuyorsa,  `ValueError` hatasını alacaksınız.

### Listeden Çıkartma

Elementi değerine veya indexine göre listeden çıkartabilirsiniz:

```python
# Using the value
names.remove('Curtis')

# Using the index
del names[1]
```

Çıkartma işlemi listede bir boşluk açığa çıkartmaz.  Diğer elemanlar
o boşluğu doldurmak için yer değiştireceklerdir. Eğer kaldırmak istediğimiz eleman birden fazla ise
`remove()` sadece ilk sıradakini kaldıracaktır.

### Liste Sıralama

Liste "yerine" göre sıralanabilir.

```python
s = [10, 1, 7, 3]
s.sort()                    # [1, 3, 7, 10]

# ters çevirme 
s = [10, 1, 7, 3]
s.sort(reverse=True)        # [10, 7, 3, 1]

# Bu herhangi bir sıralanabilir verilerde çalışabilir.
s = ['foo', 'bar', 'spam']
s.sort()                    # ['bar', 'foo', 'spam']
```

 `sorted()` kullanarak yeni bir liste oluşturmak istiyorsanız:

```python
t = sorted(s)               # s değiştirilmemiş, t ise sıralanmış veriyi tutar
```

### Listeler ve Matematik işlemleri

*Uyarı: Listeler matematik işlemleri için tasarlanmamıştır.*

```python
>>> nums = [1, 2, 3, 4, 5]
>>> nums * 2
[1, 2, 3, 4, 5, 1, 2, 3, 4, 5]
>>> nums + [10, 11, 12, 13, 14]
[1, 2, 3, 4, 5, 10, 11, 12, 13, 14]
```

Özellikle, listeler vektörler/matrisler için değildir . (MATLAB, Octave, R, etc.)
Ancak bunun için bazı paketler ve modüller varıdr. (e.g. [numpy](https://numpy.org)).

## Alıştırmalar

Bu alıştırmalarda, Python'ın liste şekillerini inceliyeceğiz. Bu bölümde,
hisse kısaltmaları olan 'symbols' stringini ele alacağız.

```python
>>> symbols = 'HPQ,AAPL,IBM,MSFT,YHOO,DOA,GOOG'
```

`split()` fonksiyonu ile stringi parçalayacağız:

```python
>>> symlist = symbols.split(',')
```

### Alıştırma 1.19: Liste öğelerini ayıklama ve yeniden atama

Göz atamayı deneyelim:

```python
>>> symlist[0]
'HPQ'
>>> symlist[1]
'AAPL'
>>> symlist[-1]
'GOOG'
>>> symlist[-2]
'DOA'
>>>
```

Yeniden değer atamayı deneyelim:

```python
>>> symlist[2] = 'AIG'
>>> symlist
['HPQ', 'AAPL', 'AIG', 'MSFT', 'YHOO', 'DOA', 'GOOG']
>>>
```

Bir miktar ayıralım:

```python
>>> symlist[0:3]
['HPQ', 'AAPL', 'AIG']
>>> symlist[-2:]
['DOA', 'GOOG']
>>>
```

Boş bir liste oluşturup ekleme yapalım.

```python
>>> mysyms = []
>>> mysyms.append('GOOG')
>>> mysyms
['GOOG']
```

Listenin bir bölümünü başka bir listeye atayabilirsiniz .Örneğin:

```python
>>> symlist[-2:] = mysyms
>>> symlist
['HPQ', 'AAPL', 'AIG', 'MSFT', 'YHOO', 'GOOG']
>>>
```

Bunu yaptığında, sol taraftaki liste (`symlist`) diğeri (`mysyms`) gibi olmak için yeniden boyutlandırılacaktır .
Örneğin, yukarıdaki örnekte, `symlist` son 2 sıradaki elemanı, `mysyms` deki tek eleman ile değiştirildi.

### Alıştırma 1.20: Liste üzerinde döngü

'for' döngüsü listenin elemanları üzerinde hareket eder.
Bu gerçekleşirken neler olduğuna bir göz atalım:

```python
>>> for s in symlist:
        print('s =', s)
# çıktıya bakın
```

### Alıştırma 1.21: Üyelik(dahil olma) testi

`in` veya `not in` operatörlerini kullanarak kontrol edin.

```python
>>> # Is 'AIG' IN the `symlist`?
True
>>> # Is 'AA' IN the `symlist`?
False
>>> # Is 'CAT' NOT IN the `symlist`?
True
>>>
```

### Alıştırma 1.22: Eleman ekleme ve silme

Eleman eklemek için `append()` metodunu kullanarak `'RHT'` elemanını `symlist` listesinin sonuna ekleyin.

```python
>>> # append 'RHT'
>>> symlist
['HPQ', 'AAPL', 'AIG', 'MSFT', 'YHOO', 'GOOG', 'RHT']
>>>
```

 `insert()` metodunu kullanarak `'AA'` elemanını ikinci sıraya ekleyin.

```python
>>> # Insert 'AA' as the second item in the list
>>> symlist
['HPQ', 'AA', 'AAPL', 'AIG', 'MSFT', 'YHOO', 'GOOG', 'RHT']
>>>
```

`remove()` metodunu kullanarak `'MSFT'` elemanını listeden çıkartın.

```python
>>> # Remove 'MSFT'
>>> symlist
['HPQ', 'AA', 'AAPL', 'AIG', 'YHOO', 'GOOG', 'RHT']
>>>
```

`'YHOO'` elemanının kopyasını listenin sonuna ekleyin.

*Not: Bu değerleri kopyalamak için iyi bir yöntem.*

```python
>>> # Append 'YHOO'
>>> symlist
['HPQ', 'AA', 'AAPL', 'AIG', 'YHOO', 'GOOG', 'RHT', 'YHOO']
>>>
```

`index()` metodunu kullanarak ilk sıradaki`'YHOO'` elemanına erişin.

```python
>>> # Find the first index of 'YHOO'
4
>>> symlist[4]
'YHOO'
>>>
```

Listede kaç tane `'YHOO'` elemanı olduğuna bakın:

```python
>>> symlist.count('YHOO')
2
>>>
```

İlk sıradaki `'YHOO'` elemanını kaldırınız.

```python
>>> # Remove first occurrence 'YHOO'
>>> symlist
['HPQ', 'AA', 'AAPL', 'AIG', 'GOOG', 'RHT', 'YHOO']
>>>
```

Bilinki listedeki istediğimiz tüm elemanları bulan veya onları kaldıran bir metod bulunmuyor.
Ancak ikinci bölümde bunu yapmanın zarif bir yolunu göreceğiz.

### Alıştırma 1.23: Sıralama

Listeyi sıralamak mı istiyorsun?  `sort()` metodunu kullan. Deneyelim:

```python
>>> symlist.sort()
>>> symlist
['AA', 'AAPL', 'AIG', 'GOOG', 'HPQ', 'RHT', 'YHOO']
>>>
```

Ters olarak sıralmak mı istiyorsun? Şunu dene:

```python
>>> symlist.sort(reverse=True)
>>> symlist
['YHOO', 'RHT', 'HPQ', 'GOOG', 'AIG', 'AAPL', 'AA']
>>>
```

Not: Liste sıralamada 'in-place' diye bir ifade vardır.  Yani, listenin öğeleri karıştırılır  ama sonuç olarak yeni bir liste oluşturulmaz.

### Alıştırma 1.24: Hepsini bir araya getirmek

Stringlerin listesi alıp tekrar onları bir stringde bileştirmek mi istiyorsun?
`join()` metodunu kullan (not: başta biraz komik gelebilir).

```python
>>> a = ','.join(symlist)
>>> a
'YHOO,RHT,HPQ,GOOG,AIG,AAPL,AA'
>>> b = ':'.join(symlist)
>>> b
'YHOO:RHT:HPQ:GOOG:AIG:AAPL:AA'
>>> c = ''.join(symlist)
>>> c
'YHOORHTHPQGOOGAIGAAPLAA'
>>>
```

### Alıştırma 1.25: Herhangi bir şeyin listesi

Herhangi bir objenin listesini yapabilirsin, buna başka listelerde dahil. (örneğin iç içe geçmiş listeler).
Şunu deneyelim:

```python
>>> nums = [101, 102, 103]
>>> items = ['spam', symlist, nums]
>>> items
['spam', ['YHOO', 'RHT', 'HPQ', 'GOOG', 'AIG', 'AAPL', 'AA'], [101, 102, 103]]
```

Yukarıdaki çıktıya dikkat edin. `items` listesinin 3 elamanıda bir listede.
İlk eleman bir string ama diğer elemanlar bir liste.

İç içe listelerde birden fazla defa index sorgusu ile erişebilirsin.

```python
>>> items[0]
'spam'
>>> items[0][0]
's'
>>> items[1]
['YHOO', 'RHT', 'HPQ', 'GOOG', 'AIG', 'AAPL', 'AA']
>>> items[1][1]
'RHT'
>>> items[1][1][2]
'T'
>>> items[2]
[101, 102, 103]
>>> items[2][1]
102
>>>
```

Teknik olarak çok karmaşık bir liste yapmak mümkün olsa da
, genel bir kural olarak, işleri basit tutmak isteyebilirsiniz. Örneğin
 tamamen sayılardan veya bir metin listesinden oluşan bir liste. Aynı listede farklı türdeki 
verileri bir araya getirmek genellikle kafanızı patlatmanın iyi bir yoludur, 
bu yüzden en iyisi bundan kaçınmaktır.

[Contents](../Contents.md) \| [Previous (1.4 Strings)](04_Strings.md) \| [Next (1.6 Files)](06_Files.md)
