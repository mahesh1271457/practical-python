[Contents](../Contents.md) \| [Previous (1.2 A First Program)](02_Hello_world.md) \| [Next (1.4 Strings)](04_Strings.md)

# 1.3 Numaralar

Bu bölümde matematiksel hesaplamaları ele alacağız.

### Numara Tipleri

Python da 4 farklı numara tipi vardır:

* Booleans
* Integers
* Floating point
* Complex (karmaşık sayılar)

### Booleans (bool)

Boolean'ın 2 farklı değeri vardır: `True`, `False`.

```python
a = True
b = False
```

Sayısal olarak integers(tam sayı) olarak değerlendirilir. `1`, `0`.

```python
c = 4 + True # 5
d = False
if d == 0:
    print('d is False')
```

*Fakat siz kodu böyle yazmayın. Bu tuhaf olurdu.*

### Integers (int) (tam sayılar)

Rastgele boyut ve tabanın oluşturduğu imzalı değerler:

```python
a = 37
b = -299392993727716627377128481812241231
c = 0x7fa8      # Hexadecimal
d = 0o253       # Octal
e = 0b10001111  # Binary
```

Yaygın işlemler:

```
x + y      Ekleme
x - y      Çıkarma
x * y      Çarpma
x / y      Bölme (float üretir)
x // y     Tam değer bölme (integer üretir)
x % y      Modül (kalanı verir)
x ** y     Üs
x << n     Bit kadar sola kaydır
x >> n     Bit kadar sağa kaydır
x & y      Bit-wise AND (bitdüzeyinde AND)
x | y      Bit-wise OR(bit düzeyinde OR
x ^ y      Bit-wise XOR (bit düzeyinde XOR)
~x         Bit-wise NOT (bit düzeyinde NOT)
abs(x)     Mutlak değer
```

### Kayan Noktalı Sayılar (float)

Float değerini gösterimi için ondalıklı veya üstel gösterim kullanın:

```python
a = 37.45
b = 4e5 # 4 x 10**5 or 400,000
c = -1.345e-10
```

Floatlar yerel CPU gösterimi kullanılarak çift kesinlikli olarak temsil edilir. [IEEE 754](https://en.wikipedia.org/wiki/IEEE_754).
 Bu C dilindeki `double` tipiyle aynıdır .

> 17 basamaklı hassasiyet 
> -308  ile 308 arası üs 

Float tipini kullanırken ondalıklı sayılarda kesin doğruluklu sonuç vermediğini unutmayın.

```python
>>> a = 2.1 + 4.2
>>> a == 6.3
False
>>> a
6.300000000000001
>>>
```

** Bu Python'nın bir hatası değil ** , ama CPU daki 'kayan nokta' donanımıyla alakalı.

Yaygın İşlemler:

```
x + y      ekle
x - y      çıkar
x * y      çarpma
x / y      bölme
x // y     tam değer bölme
x % y      modül
x ** y     üs
abs(x)     mutlak değer
```

Bunlar, bit bazlı operatörler haricinde integerlar ile aynı operatörlerdir.
Daha fazla matematik fonksiyonu için 'math' modülünü kullanabilirsiniz.

```python
import math
a = math.sqrt(x)
b = math.sin(x)
c = math.cos(x)
d = math.tan(x)
e = math.log(x)
```


### 	Karşılaştırmalar

Aşağıdaki karşılaştırmalar/ilişkisel operatörler sayılarla çalışır:

```
x < y      daha az
x <= y     daha az veya eşit
x > y      daha fazla
x >= y     daha fazla veya eşit
x == y     eşittir
x != y     eşit değildir
```

Aşağıdakileri kullanarak daha karmaşık mantıksal ifadeler oluşturabilirsiniz :

`and`, `or`, `not`

İşte bir miktar örnek:

```python
if b >= a and b <= c:
    print('b ,c ile a arasında')

if not (b < a or b > c):
    print('b hala  a ve c arasında')
```

### Sayı Dönüşümleri

Değerlerin tiplerini değiştirmek için kullanılır:

```python
a = int(x)    # x'i integer a çevirir
b = float(x)  # x'i float a çevirir
```

Deneyelim.

```python
>>> a = 3.14159
>>> int(a)
3
>>> b = '3.14159' # bu string'leri sayılara çevirirken de kullanılabilir
>>> float(b)
3.14159
>>>
```

## Alıştırmalar

Hatırlatma: Bu alıştırmalar `practical-python/Work` dizinde çalıştığınız varsayılarak yazılmıştır. `mortgage.py`
dosyasına bakın.

### Alıştırma 1.7: Dave'in İpoteği

Dave, 30 yıllık sabit faizle $500,000 parayı
Guido’nun ipotek , Hisse yatırım ve Bitcoin şirketinden alır. 
Faiz oranı 5% ve aylık ödemesi $2684.11.

İşte Dave'in toplam ödeyeceği tutarı hesaplayan program:

```python
# mortgage.py

principal = 500000.0 #anapara
rate = 0.05 #faiz
payment = 2684.11 #ödeme
total_paid = 0.0 #toplam ödeme

while principal > 0:
    principal = principal * (1+rate/12) - payment
    total_paid = total_paid + payment

print('Total paid', total_paid)
```

Programı çalıştırdığınız zaman alacağınız cevap `966,279.6` olacaktır.

### Alıştırma 1.8: Ektra Ödemeler

Dave'in ipoteğin ilk 12 ayı için fazladan 1000 $ / ay ödediğini varsayalım?

Programı bu ekstra ödemeleri içerecek şekilde yeniden düzenleyelim ve ödenen toplam tutarı ,gerekli ay ile birlikte yazdırmasını sağlayalım

Bu yeni programı çalıştırdığınızda cevap olarak `929,965.62` ve 342 months (ay) olarak alacaksınız.

### Alıştırma 1.9: Ekstra Ödemeleri Hesaplayalım

Programı ektra ödeme bilgilerini daha genel bir şekilde işleyebilmesi için değiştirelim.
Kullanıcının bu değerleri daha sonra tekrar ayarlayabilmesi için değiştirelim:

```python
extra_payment_start_month = 61
extra_payment_end_month = 108
extra_payment = 1000
```

Programın bu değişkenleri kontrol etmesini sağlayın ve ödenen toplam tutarı uygun şekilde hesaplayın.

Dave eğer 5. yıldan başlayarak 4 yıl boyunca ektra $1000/aylık öderse ipoteğe toplam ne kadar ödeyecektir?  

### Alıştırma 1.10: Tablo Yapalım

Programı ; ayı, şimdiye kadar ödenen tutarı ve kalan ödemeyi gösterecek şekilde düzenleyelim.

Çıktımız şuna benzer bişey olacaktır:

```bash
1 2684.11 499399.22
2 5368.22 498795.94
3 8052.33 498190.15
4 10736.44 497581.83
5 13420.55 496970.98
...
308 874705.88 3478.83
309 877389.99 809.21
310 880074.1 -1871.53
Total paid 880074.1
Months 310
```

### Alıştırma 1.11: Bonus

Hazır siz burdayken, geçen ay meydana gelen fazla ödemeyi önlemek için programı düzeltin.

### Exercise 1.12: Bir Gizem

`int()` ve `float()` numaraları dönüştürürken kullanılabilir.  Örneğin,

```python
>>> int("123")
123
>>> float("1.23")
1.23
>>>
```

Bunu aklınıda tutarak ,aşağıdakini açıklayabilir misiniz?

```python
>>> bool("False")
True
>>>
```

[Contents](../Contents.md) \| [Previous (1.2 A First Program)](02_Hello_world.md) \| [Next (1.4 Strings)](04_Strings.md)
