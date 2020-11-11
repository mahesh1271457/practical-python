[Contents](../Contents.md) \| [Previous (2.2 Containers)](02_Containers.md) \| [Next (2.4 Sequences)](04_Sequences.md)

# 2.3 Formatting(Biçimlendirme)

Burası küçük bir konudur . Verilerle çalıştığınızda genellikle 
yapılandırılmış çıktı üretmek istersiniz (tablo, vb.). Örneğin:

```code
      Name      Shares        Price
----------  ----------  -----------
        AA         100        32.20
       IBM          50        91.10
       CAT         150        83.44
      MSFT         200        51.23
        GE          95        40.37
      MSFT          50        65.10
       IBM         100        70.44
```

### String Biçimlendirme

String biçimlendirme için bir yol ise  ,Python 3.6+ için `f-strings` ifadesidir.

```python
>>> name = 'IBM'
>>> shares = 100
>>> price = 91.1
>>> f'{name:>10s} {shares:>10d} {price:>10.2f}'
'       IBM        100      91.10'
>>>
```

`{expression:format}` şeklinde değiştirilir.

Genellikle `print` ifadesiyle beraber kullanılır.

```python
print(f'{name:>10s} {shares:>10d} {price:>10.2f}')
```

### Biçimlendirme Kodları

Biçimlendirme kodu olarak ( `:`sonra  `{}`nın içine) C dilindeki  `printf()` ile benzerdir.  
Yaygın kodlar aşağıdadır:

```code
d       Decimal integer
b       Binary integer
x       Hexadecimal integer
f       Float as [-]m.dddddd
e       Float as [-]m.dddddde+-xx
g       Float, but selective use of E notation
s       String
c       Character (from integer)
```

Bunlar alan genişliği veya ondalık kısmın hassasiyeti için ayarlardır. 
Bu kısmi bir listedir:

```code
:>10d   Integer right aligned in 10-character field (10 karakterlik alanda sağa hizala)
:<10d   Integer left aligned in 10-character field(10 karakterlik alanda sola hizala)
:^10d   Integer centered in 10-character field (10 karakterlik alanda ortaya hizala)
:0.2f   Float with 2 digit precision (2 basamaklı hassasiyetle ondalık kısmı)
```

### Sözlüklerde Biçimlendirme

Sözlük değerlerini(value) biçimlendirirken `format_map()` metodunu kullanır :

```python
>>> s = {
    'name': 'IBM',
    'shares': 100,
    'price': 91.1
}
>>> '{name:>10s} {shares:10d} {price:10.2f}'.format_map(s)
'       IBM        100      91.10'
>>>
```

 `f-strings` ile aynı kullanımdır ama bu değerleri(value) sözlükten alır.

### format() metodu

There is a method `format()` that can apply formatting to arguments or
keyword arguments.

```python
>>> '{name:>10s} {shares:10d} {price:10.2f}'.format(name='IBM', shares=100, price=91.1)
'       IBM        100      91.10'
>>> '{:10s} {:10d} {:10.2f}'.format('IBM', 100, 91.1)
'       IBM        100      91.10'
>>>
```

Açıkçası, `format()` biraz ayrıntılı. Ben 'f-strings' i tercih ederim  .

### C-Style Formatting(C stili ile biçimlendirme)

 `%` yi  biçimlendirme operatörünü de kullanabilirsiniz..

```python
>>> 'The value is %d' % 3
'The value is 3'
>>> '%5d %-5d %10d' % (3,4,5)
'    3 4              5'
>>> '%0.2f' % (3.1415926,)
'3.14'
```

Bu sağında bir obje veya  tuple'a  ihtiyaç duyar.  Biçimlendirme kodları 
C 'printf()' gibi sonradan modellendi.

*Not: Bu biçimlendirme sadece byte string'lerinde geçerlidir.*

```python
>>> b'%s has %n messages' % (b'Dave', 37)
b'Dave has 37 messages'
>>>
```

## Alıştırmalar

### Alıştırma 2.8: Numaraları nasıl biçimlendiririz ?

Ondalık sayıların yapızlması yaygın bir problemdir .
Bunu çözmeninin bir yoluda 'f-strings' kullanabiliriz. 
Bunu örneklerden bakalım:

```python
>>> value = 42863.1
>>> print(value)
42863.1
>>> print(f'{value:0.4f}')
42863.1000
>>> print(f'{value:>16.2f}')
        42863.10
>>> print(f'{value:<16.2f}')
42863.10
>>> print(f'{value:*>16,.2f}')
*******42,863.10
>>>
```

f-strings hakkında daha fazlası için sitesini ziyaret edin
[here](https://docs.python.org/3/library/string.html#format-specification-mini-language).Biçimlendirme 
bazen `%` operatörü ile stringler üzerinde de gerçekleştirilebilir.

```python
>>> print('%0.4f' % value)
42863.1000
>>> print('%16.2f' % value)
        42863.10
>>>
```

`%` ile ilgili dökümantasyonlara bakmak için 
[here](https://docs.python.org/3/library/stdtypes.html#printf-style-string-formatting).

Yaygın olarak `print` kullanılsada, string biçimlendirmek 'printing'(yazdırma) ile ilgili değiildir.
Eğer bir biçimlendirilmiş stringi saklamak için onu sadece bir değişkene atayın.

```python
>>> f = '%0.4f' % value
>>> f
'42863.1000'
>>>
```

### Alıştırma 2.9: Collecting Data (Veri Toplama)

Alıştırma 2.7 içinde , `report.py` adlı hisse portföyünün kazanç/kayıp hesabı yapan programı yazdınız.  
Bu egzersizde tabloyu düzenlemeyi düzenleyeceğiz:

```
      Name     Shares      Price     Change
---------- ---------- ---------- ----------
        AA        100       9.22     -22.98
       IBM         50     106.28      15.18
       CAT        150      35.46     -47.98
      MSFT        200      20.89     -30.34
        GE         95      13.48     -26.89
      MSFT         50      20.89     -44.21
       IBM        100     106.28      35.84
```

Bu raporda, "Price" hissenin bedelini ve 
"Change" hisse senedinin değişimini gösterir.


Yukarıdaki tabloyu elde etmeden önce, ilk olarak kulllanacağınız tüm verileri toplamanız gerekir.
`make_report()` isimli fonksiyon yazalım bu girdi olarak hisse senetlerinin listesini ile fiyatlar sözlüğünü ve
dönüt olarak yukarıdaki tablonun satırlarını içeren tuple'ların bir listesini döndürür.

Sonrasında fonksiyonu `report.py` dosyasına. Nasıl çalıştığı gerektiği burada
etkileşimli olarak deneyerek görelim:

```python
>>> portfolio = read_portfolio('Data/portfolio.csv')
>>> prices = read_prices('Data/prices.csv')
>>> report = make_report(portfolio, prices)
>>> for r in report:
        print(r)

('AA', 100, 9.22, -22.980000000000004)
('IBM', 50, 106.28, 15.180000000000007)
('CAT', 150, 35.46, -47.98)
('MSFT', 200, 20.89, -30.339999999999996)
('GE', 95, 13.48, -26.889999999999997)
...
>>>
```

### Alıştırma 2.10: Biçimlendirilmiş Tabloyu Yazdıralım

Alıştırma 2.9 daki for-loop'u tekrar yapın, ama print ifadesini 
tuple biçimledirecek şekilde değiştirin.

```python
>>> for r in report:
        print('%10s %10d %10.2f %10.2f' % r)

          AA        100       9.22     -22.98
         IBM         50     106.28      15.18
         CAT        150      35.46     -47.98
        MSFT        200      20.89     -30.34
...
>>>
```

f-strings ile de verileri genişletebilirsiniz. Örneğin:

```python
>>> for name, shares, price, change in report:
        print(f'{name:>10s} {shares:>10d} {price:>10.2f} {change:>10.2f}')

          AA        100       9.22     -22.98
         IBM         50     106.28      15.18
         CAT        150      35.46     -47.98
        MSFT        200      20.89     -30.34
...
>>>
```

Yukarıdaki ifadeleri `report.py` adlı programınıza ekleyin.
Programınız `make_report()` fonksiyonu ve print ile size biçimlendirilmiş tabloyu verecektir.

### Alıştırma 2.11: Başlık Ekleme

Bunlar gibi başlıklarınız olduğunu varsayalım:

```python
headers = ('Name', 'Shares', 'Price', 'Change')
```

Programınıza yukarıdaki başlıkları içeren tuple ekleyin , her başlık stringi
10 karakterlik genişlikte sağa hizalanır ve her bir alan tek bir boşlukla ayrılmıştır.


```python
'      Name     Shares      Price      Change'
```

Kodunuzudaki başlık ve verilerin stringleri ayırmak için ,
Stringlerin altına "-" karakterini yerleştirin.Örneğin:

```python
'---------- ---------- ---------- -----------'
```

İşiniz bittiğinde ,tablonuz şu şekilde görünecektir.

```
      Name     Shares      Price     Change
---------- ---------- ---------- ----------
        AA        100       9.22     -22.98
       IBM         50     106.28      15.18
       CAT        150      35.46     -47.98
      MSFT        200      20.89     -30.34
        GE         95      13.48     -26.89
      MSFT         50      20.89     -44.21
       IBM        100     106.28      35.84
```

### Alıştırma 2.12: Biçimlendirme Yarışı

Kodunuzu ($) sembolü içerecek şekilde nasıl düzenlerdiniz ? Çıktı şu şekilde görünecek:

```
      Name     Shares      Price     Change
---------- ---------- ---------- ----------
        AA        100      $9.22     -22.98
       IBM         50    $106.28      15.18
       CAT        150     $35.46     -47.98
      MSFT        200     $20.89     -30.34
        GE         95     $13.48     -26.89
      MSFT         50     $20.89     -44.21
       IBM        100    $106.28      35.84
```

[Contents](../Contents.md) \| [Previous (2.2 Containers)](02_Containers.md) \| [Next (2.4 Sequences)](04_Sequences.md)
