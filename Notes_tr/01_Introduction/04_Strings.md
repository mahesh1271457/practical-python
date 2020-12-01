[Contents](../Contents.md) \| [Previous (1.3 Numbers)](03_Numbers.md) \| [Next (1.5 Lists)](05_Lists.md)

# 1.4 Stringler (diziler)

Bu bölümde yazılarla (text) çalışmayı ele alacağız.

### Değişmez Metni Temsil Etme

Diziler tırnak işaretleri arasına yazılır.

```python
# tek tırnak
a = 'ayrılır mı et tırnaktan...'

# çift tırnak
b = "çok güzel bir string"

# üçlü tırnak
c = '''
Aysel git başımdan ben sana göre değilim
Ölümüm birden olacak seziyorum
Hem kötüyüm karanlığım biraz çirkinim
Aysel git başımdan seni seviyorum
'''
```

Normal stringler sadece bir satır olabilir. Üçlü tırnak işaretleri, tüm biçimlendirme dahil olmak üzere birden çok satırda yer alan tüm metni kapsar.

Tek  (') veya çift  (") tırnak arasında herhangi bir fark yok.
 *Ancak, diziye hangisi ile başlandıysa, onunla bitirilmesi gerekir*.

### String Kaçış Kodları

Kaçış kodları, karakterleri kontrol etmek ve klavyede kolay yapılamayan karekterleri temsil eder.
İşte bazı yaygın kaçış kodları:

```
'\n'      Alt satıra geçme
'\r'      Aynı satır başına döner
'\t'      Tab boşluğu bırakır
'\''      Tek tırnak işaretini tırnak içinde kullanmak için
'\"'      Çift tırnak işaretini tırnak içinde kullanmak için
'\\'      Tırnak içinde \ kullanmak için
```

### String Gösterimi

Stringin içindeki her karakter Unicode "code-point" de bir tam sayı olarak depolanır.

Aşağıdaki gibi kaçış dizilerini kullanarak tam bir kod noktası(code-point) değeri belirtebilirsiniz:

```python
a = '\xf1'          # a = 'ñ'
b = '\u2200'        # b = '∀'
c = '\U0001D122'    # c = '𝄢'
d = '\N{FOR ALL}'   # d = '∀'
```

[Unicode Character Database](https://unicode.org/charts) mevcut tüm karakter kodları için referanstır.  

### Dizi İndeksleme

String'ler her karaktere erişmek için arrayler gibi çalışır. Bir tam sayısı indeks olarak kullanabilirsiniz (0'dan başlayarak).
Negatif indeksler dizenin sonuna göre konumu belirtir.

```python
a = 'Hello world'
b = a[0]          # 'H'
c = a[4]          # 'o'
d = a[-1]         # 'd' (stringin sondan konumu)
```

Spesifik olarak belirli bir aralıktakileri de elde edebilirsiniz. `:` kullanarak.

```python
d = a[:5]     # 'Hello'
e = a[6:]     # 'world'
f = a[3:8]    # 'lo wo'
g = a[-5:]    # 'world'
```

Sondaki index karaktere dahil değildir bir öncekini verir. Eksik indeksler dizinin başlagıcı veya sonunu varsayar.

### String işlemleri

Birleştirme, uzunluk, üyelik(dahillik) ve çoğaltma.

```python
# Birleştirme(+)
a = 'Hello' + 'World'   # 'HelloWorld'
b = 'Say ' + a          # 'Say HelloWorld'

# uzunluk (len)
s = 'Hello'
len(s)                  # 5

# dahillik testi(`in`, `not in`)
t = 'e' in s            # True
f = 'x' in s            # False
g = 'hi' not in s       # True

# çoğaltma (s * n)
rep = s * 5             # 'HelloHelloHelloHelloHello'
```

### String metotları

Stringler'in, çeşitli işlemler gerçekleştiren metotları vardır.

Örnek: öndeki / sondaki beyaz boşluğu silme.

```python
s = '  Hello '
t = s.strip()     # 'Hello'
```

Örnek: Durum dönüşümü.

```python
s = 'Hello'
l = s.lower()     # 'hello'
u = s.upper()     # 'HELLO'
```

Örnek: Yer değiştirme.

```python
s = 'Hello world'
t = s.replace('Hello' , 'Hallo')   # 'Hallo world'
```

**Daha fazla string metodu:**

String'ler, metin(text) verilerini test etmek ve değiştirmek için çok çeşitli başka metodlara sahiptir.
İşte birkaç metot örnekleri:

```python
s.endswith(suffix)     # 'suffix' ile bitip bitmediğini kontrol eder
s.find(t)              # String'deki ilk 't' yi bulur
s.index(t)             # String'deki ilk 't'nin indeksini bulur
s.isalpha()            # Karakterler alfabetik mi kontrol eder
s.isdigit()            # Karekterler numerik mi kontrol eder
s.islower()            # Karekterler küçük harf mi kontrol eder
s.isupper()            # Karekterler büyük harf mi kontrol eder
s.join(slist)          # String'e liste ekler
s.lower()              # Tüm harfleri küçük harfe dönüştürür
s.replace(old,new)     # Yazıyı değiştirir
s.rfind(t)             # Sondan 't'arar
s.rindex(t)            # sondan't' arar
s.split([delim])       # String'i alt string'lere böler
s.startswith(prefix)   # String 'prefix' ile başlıyor mu kontrol eder
s.strip()              # Baştaki ve sondaki boşlukları siler
s.upper()              # Tüm harfleri büyük harfe dönüştürür
```

### String Değişkenliği

String'ler değişmez veya sadece okunur(salt-okunur).
Bir kere yaratılırlar ve değerleri değiştirilemez.

```python
>>> s = 'Hello World'
>>> s[1] = 'a'
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
TypeError: 'str' object does not support item assignment
>>>
```

**String verilerini işleyen tüm işlemler ve metotlar her zaman yeni dizeler oluşturur.**

### String Dönüşümleri

 `str()` kullanarak herhangi bir değeri stringe dönüştürülebilir. Stringin tutuğu sonuç
`print()` ifadesiyle üretilmiş olanla aynı olur .

```python
>>> x = 42
>>> str(x)
'42'
>>>
```

### Byte Strings

Genellikle düşük seviyeli programlama dillerinde 8 bitlik bir bayt dizesi aşağıdaki gibi yazılır:

```python
data = b'Hello World\r\n'
```

İlk tırnaktan önce bir b koyarak, bunun bir metin dizesi yerine bir bayt dizesi olduğunu belirtirsiniz..

Genel string işlemlerinin çoğu çalışır.

```python
len(data)                         # 13
data[0:5]                         # b'Hello'
data.replace(b'Hello', b'Cruel')  # b'Cruel World\r\n'
```

İndeksleme biraz farklıdır çünkü bayt değerlerini tamsayı(integer) olarak döndürür.

```python
data[0]   # 72 (ASCII code for 'H')
```

String'lerden/string'e dönüştürme .

```python
text = data.decode('utf-8') # bytes -> text
data = text.encode('utf-8') # text -> bytes
```

`'utf-8'` argümanı bir karakter kodlamasını belirtir. Diğer ortak
değerler şunları içerir `'ascii'` and `'latin1'`.

### Ham Stringler

Ham stringler ,yorumlanmamış ters slaş(\) a sahip değişmez stringlerdir . İlk tırnaktan önce "r" harfi ile belirtilir.

```python
>>> rs = r'c:\newdata\test' # Raw (uninterpreted backslash)
>>> rs
'c:\\newdata\\test'
```

String, aynen yazıldığı gibi ,içinde yer alan değişmez metindir.
Bu, ters eğik(\) çizginin önemli olduğu durumlarda kullanışlıdır.
Örneğin: dosya adları, düzenli ifade (regex) vb.

### f-Strings

Biçimlendirilmiş bir şekilde olan string.

```python
>>> name = 'IBM'
>>> shares = 100
>>> price = 91.1
>>> a = f'{name:>10s} {shares:10d} {price:10.2f}'
>>> a
'       IBM        100      91.10'
>>> b = f'Cost = ${shares*price:0.2f}'
>>> b
'Cost = $9110.00'
>>>
```

**Not :Python 3.6 veya daha yenisini gerektirir.** .Biçim kodlarının anlamı
daha sonra bakacağız.

## Alıştırmalar

Bu alıştırmada,Python operatörleri ve stirnglerele deneyim yaşayacağız. 
Bunları Python interaktif komut isteminde yaparsanız
sonuçları kolayca görebilirsiniz. Önemli not:

> Bu alıştırmalarda interpreter ile etkileşime girmelisiniz,
> `>>>` bu interpreter istemi Python sizden yeni bir ifade istediğinde görünücek. Bazı egzersizlerde
> birden fazla ifade girmeniz gerkebilir, birkaç kez
> 'return' etmeniz gerekebilir . Sadece hatırlatma: örneklerde çalışırken *SAKIN* 
>  `>>>`  yazmayınız.

String bir hisse senedi kısaltmalarını tutacak şekilde tanımlayalım:

```python
>>> symbols = 'AAPL,IBM,MSFT,YHOO,SCO'
>>>
```

### Alıştırma 1.13: Stringi alt dizelere ayırma

Stringler karakterlerden oluşan arrayler gibidir. Karakterleri çıkarmaya çalışalım:

```python
>>> symbols[0]
?
>>> symbols[1]
?
>>> symbols[2]
?
>>> symbols[-1]        # Son karakter
?
>>> symbols[-2]        # Sondan 2. karakter
?
>>>
```

Python da, stringler salt okunurdur (sadece okunur).

`symbols` ilk karakterini küçük harf yapmayı deneyin ('a').

```python
>>> symbols[0] = 'a'
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'str' object does not support item assignment
>>>
```

### Alıştırma 1.14: String Birleştirme

String verileri salt okunurdur ama her zaman farklı 
yeni bir değişken olarak yaratılabilirler.


Try the following statement which concatenates a new symbol "GOOG" to the end of symbols:

```python
>>> symbols = symbols + 'GOOG'
>>> symbols
'AAPL,IBM,MSFT,YHOO,SCOGOOG'
>>>
```

Oops! Bizim istediğimiz bu değildi. Bunu düzelterek `'AAPL,IBM,MSFT,YHOO,SCO,GOOG'` sonucunu almayı dene.

```python
>>> symbols = ?
>>> symbols
'AAPL,IBM,MSFT,YHOO,SCO,GOOG'
>>>
```

`'HPQ'`'yi string'in başına ekleyelim:

```python
>>> symbols = ?
>>> symbols
'HPQ,AAPL,IBM,MSFT,YHOO,SCO,GOOG'
>>>
```

Bu örneklerde, orjinal string değiştirilmiş gibi görünebilir ,bu da salt-okunur kuralını çalışmıyor gibi gösterebilir.
Ama öyle değil. Her seferinde işlemler gerçekleşirken yeni bir stringe üretilir.
Siz yeniden aynı değişken adını kullandığınızda, tam bu noktada yeni bir string yaratırılır. 
Buradan sonra, eski string değişkeni kullanılmamak üzere yok edilir.

### Alıştırma 1.15: Üyelik Testi (dahillik)

'in' operatörü, alt dizeleri kontrol eder. İnteraktif komut 
satırında şunları deneyelim:

```python
>>> 'IBM' in symbols
?
>>> 'AA' in symbols
True
>>> 'CAT' in symbols
?
>>>
```

*Neden `'AA'` sorgusunda  `True` değeri döndü?*

###  Alıştırma 1.16: String Metotları

Python interaktif komut istemcisinde, bazı string metotlarını deneyelim.

```python
>>> symbols.lower()
?
>>> symbols
?
>>>
```

Unutmayın,stringler salt okunurdur(sadece okunur).  Eğer bir islem sonucunu kaydetmek istiyorsanız onu bir değişkene atmanız gerekir.:

```python
>>> lowersyms = symbols.lower()
>>>
```

Daha fazla işlem deneyelim :


```python
>>> symbols.find('MSFT')
?
>>> symbols[13:17]
?
>>> symbols = symbols.replace('SCO','DOA')
>>> symbols
?
>>> name = '   IBM   \n'
>>> name = name.strip()    # etrafındakı beyaz boşlukları siler
>>> name
?
>>>
```

### Alıştırma 1.17: f-strings

Bazen bir string oluşturmak ve içine değeri gömmek isteyebilirsiniz.

Bunu yapmak için f-string kullanın. Örneğin:

```python
>>> name = 'IBM'
>>> shares = 100
>>> price = 91.1
>>> f'{shares} shares of {name} at ${price:0.2f}'
'100 shares of IBM at $91.10'
>>>
```

`mortgage.py` programını düzenleyelim [Exercise 1.10](03_Numbers.md) f-strings kullanarak çıktı elde etmeye çalışalım.
Çıktının güzel şekilde hizalanmasını sağlayın.


### Alıştırma 1.18: Düzenli İfadeler

Temel dizin işlemlerinin bir sınırlaması ise,
her türlü gelişmiş örüntü eşleştirmesini desteklememesidir.  
Bunun için Python'un 're' modülüne ve düzenli ifadelerine dönmeniz gerekiyor.
Düzenli ifade işleme büyük bir konudur, işte kısa bir örnek:

```python
>>> text = 'Today is 3/27/2018. Tomorrow is 3/28/2018.'
>>> # Find all occurrences of a date
>>> import re
>>> re.findall(r'\d+/\d+/\d+', text)
['3/27/2018', '3/28/2018']
>>> # Replace all occurrences of a date with replacement text
>>> re.sub(r'(\d+)/(\d+)/(\d+)', r'\3-\1-\2', text)
'Today is 2018-3-27. Tomorrow is 2018-3-28.'
>>>
```

 `re` modülü ile ilgili daha fazla bilgi için orjinal dökümantasyona bakınız:
[https://docs.python.org/library/re.html](https://docs.python.org/3/library/re.html).


### Yorum

Interpreter ile denemeye başladığınızda, farklı nesneler tarafından 
desteklenen işlemler hakkında daha fazla bilgi edinin. Örneğin ,hangi işlem
bir string için uygun ?

Python ortamınıza göre değişiklik gösterebilir, hangi metotların uygun olduğunu
görmek isterseniz tab-completion kullanın.  Örneğin şunu deneyin :

```python
>>> s = 'hello world'
>>> s.<tab key>
>>>
```

Eğer bu komut işinizde yaramadıysa `dir()` fonksiyonunu kullanabilirsiniz.
Örneğin:

```python
>>> s = 'hello'
>>> dir(s)
['__add__', '__class__', '__contains__', ..., 'find', 'format',
'index', 'isalnum', 'isalpha', 'isdigit', 'islower', 'isspace',
'istitle', 'isupper', 'join', 'ljust', 'lower', 'lstrip', 'partition',
'replace', 'rfind', 'rindex', 'rjust', 'rpartition', 'rsplit',
'rstrip', 'split', 'splitlines', 'startswith', 'strip', 'swapcase',
'title', 'translate', 'upper', 'zfill']
>>>
```

`dir()` fonksiyonu  '(.)'dan sonra kullanabileceğiniz tüm metotları gösterir .
`help()` komutu daha spesifik bilgiler için kullanılabilir:

```python
>>> help(s.upper)
Help on built-in function upper:

upper(...)
    S.upper() -> string

    Return a copy of the string S converted to uppercase.
>>>
```

[Contents](../Contents.md) \| [Previous (1.3 Numbers)](03_Numbers.md) \| [Next (1.5 Lists)](05_Lists.md)
