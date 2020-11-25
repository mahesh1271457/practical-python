[Contents](../Contents.md) \| [Previous (1.1 Python)](01_Python.md) \| [Next (1.3 Numbers)](03_Numbers.md)

# 1.2 İlk Program

Bu bölüm ilk programınızı oluşturmayı, yorumlayıcıyı çalıştırmayı ve bazı basit hata ayıklama işlemlerini kapsar.

### Python’ı Çalıştırmak

Python programları her zaman bir yorumlayıcı içerisinde çalışır.
 
Yorumlayıcı normalde komut kabuğundan çalışan bir “konsol-bazlı” uygulamadır.

```bash
python3
Python 3.6.1 (v3.6.1:69c0db5050, Mar 21 2017, 01:21:04)
[GCC 4.2.1 (Apple Inc. build 5666) (dot 3)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>>
```

Uzman programcılar yorumlayıcıyı bu şekilde kullanmak konusunda problem yaşamazlar ancak bu yeni başlayanlar için pek kullanıcı dostu değildir. Python’a farklı arayüz sağlayan bir ortam kullanıyor olabilirsiniz. Bunda sorun yok, ancak Python terminali nasıl çalıştıracağınız öğrenmek kullanışlı olabilir.

### İnteraktif Mod

Python’ı başlattığınızda, içerisinde denemeler yapabileceğiniz *interaktif* mod açılır.
 
Eğer ifadeleri yazmaya başlarsanız, hemen çalıştırılacaklardır. düzenle/derle/çalıştır/hata ayıkla döngüsü yoktur.

```python
>>> print('hello world')
hello world
>>> 37*42
1554
>>> for i in range(5):
...     print(i)
...
0
1
2
3
4
>>>
```

Bu *read-eval-print-loop* (or REPL) olarak adlandırılır ve hata ayıklama ve keşif için çok kullanışlıdır.
 
**DURUN**: Python ile nasıl etkileşime geçeceğinizi anlayamadıysanız, ne yapıyorsanız durun ve etkileşime nasıl geçileceğini çözmeye çalışın.  Eğer bir IDE kullanıyorsanız, menü seçeneğinin veya diğer pencere seçeneğinin altında gizlenmiş olabilir. Bu kursun birçok kısmı yorumlayıcıyla etkileşime geçtiğinizi varsayar.

REPL’in elementlerine yakından göz atalım:

- `>>>` yeni ifade başlatan yorumlayıcı komutudur.
- `...` bir ifadeyi devam ettiren yorumlayıcı komutudur. Yazmayı bitirmek ve ne girdiyseniz onu çalıştırmak için boş bir satır girin.
 
`...` komutu çalıştığınız ortama bağlı olarak gösterilebilir de gösterilmeyebilir de. Bu kurs için kes/yapıştır kod örneklerini kolaylaştırmak amacıyla boşluk olarak gösterilecektir.

Alt tire `_` son sonucu tutar.

```python
>>> 37 * 42
1554
>>> _ * 2
3108
>>> _ + 50
3158
>>>
```

*Bu sadece interaktif modda doğrudur.* Programda asla `_` kullanılmaz.

### Programları oluşturmak

Programlar `.py` dosyalarının içine konur.

```python
# hello.py
print('hello world')
```

Bu dosyaları favori metin editörünüzde oluşturabilirsiniz.
 
### Programları Çalıştırmak

Bir programı çalıştırmak için, terminalde `python` komutu ile çalıştırın.
Örneğin, Unix’te komut satırında:

```bash
bash % python hello.py
hello world
bash %
```

Veya Windows Shell’den:

```
C:\SomeFolder>hello.py
hello world

C:\SomeFolder>c:\python36\python hello.py
hello world
```

Not: Windows’ta, Python yorumlayıcısına `c:\python36\python` gibi tam yolu belirtmeniz gerekebilir.
Ancak, eğer Python olağan bir şekilde yüklüyse, `hello.py` gibi sadece programın adını yazmanız da yeterli olacaktır.

### Basit Bir Program

Aşağıdaki problemi çözelim:

> Bir sabah, dışarı çıkıyorsunuz ve Chicago’daki Sears kulesin yanındaki kaldırıma bir dolar banknotu koyuyorsunuz.
> Günden güne, dışarı çıkıyor ve banknotları iki katıyla çoğaltıyorsunuz.
> Banknotların yığınının kulenin yüksekliğini aşması ne kadar sürer?

İşte çözüm:

```python
# sears.py
bill_thickness = 0.11 * 0.001 # Metre (0.11 mm)
sears_height = 442 # Yükseklik (metre)
num_bills = 1
day = 1

while num_bills * bill_thickness < sears_height:
    print(day, num_bills, num_bills * bill_thickness)
    day = day + 1
    num_bills = num_bills * 2

print('Number of days', day)
print('Number of bills', num_bills)
print('Final height', num_bills * bill_thickness)
```

Çalıştırdığınızda aşağıdaki çıktıyı alırsınız:

```bash
bash % python3 sears.py
1 1 0.00011
2 2 0.00022
3 4 0.00044
4 8 0.00088
5 16 0.00176
6 32 0.00352
...
21 1048576 115.34336
22 2097152 230.68672
Number of days 23 
Number of bills 4194304 
Final height 461.37344
```

Bu programı rehber olarak kullandığınızda, Python hakkında birçok önemli temel konseptleri öğrenebilirsiniz.
 
### İfadeler

Bir Python programı bir dizi ifadelerden oluşur:

```python
a = 3 + 4
b = a * 2
print(b)
```

Her ifade yeni bir satır tarafından çalıştırılır. İfadeler dosyanın sonuna gelinene kadar teker teker sırayla çalıştırılır.
 
### Yorumlar

Metin halindeki yorumlar çalıştırılmayacaktır. 

```python
a = 3 + 4
# Bu bir yorumdur
b = a * 2
print(b)
```

Yorumlar `#` ile gösterilir ve satırın sonuna kadar genişler.

### Değişkenler

Değişken, bir değer için verilen isimdir. Alt tire `_` karakterinin yanı sıra a-z (büyük ve küçük olarak) tüm harfleri kullanabilirsiniz. 
Sayılar da değişken isimlerinin bir parçası olabilir ancak ilk karakteri olamaz.

```python
height = 442 # uygun
_height = 442 # uygun
height2 = 442 # uygun
2height = 442 # uygun değil
```

### Tipler

Değişkenler tiplerle beraber oluşturulmak zorunda değildirler. Tip, değişken ismiyle değil eşitliğin sağ tarafındaki değerin tipiyle ilişkilendirilir. 

```python
height = 442           # Tamsayı (Integer)
height = 442.0         # Ondalıklı Sayı (Floating point)
height = 'Really tall' # Kelime (String)
```

Python dinamik tiplidir. Bir değişkenin tipi program çalışırken kendisine atanan değere bağlı olarak değişebilir.

### Karakter Duyarlılığı

Python karakter duyarlıdır. Büyük ve küçük harfler farklı harflermiş gibi algılanır. Bunlar farklı değişkenlerdir:

```python
name = 'Jake'
Name = 'Elwood'
NAME = 'Guido'
```

Dil (Python dili) koşulları her zaman küçük harfle yazılır.

```python
while x < 0:   # DOĞRU
WHILE x < 0:   # HATA
```

### Döngü

`while` ifadesi döngü çalıştırır.

```python
while num_bills * bill_thickness < sears_height:
    print(day, num_bills, num_bills * bill_thickness)
    day = day + 1
    num_bills = num_bills * 2

print('Number of days', day)
```

`while` ifadesi altındaki girintili komutlar `while` `true` olduğu sürece çalışır.

### Girintiler

Girintiler birlikte çalışacak ifade gruplarını belirtmek için kullanılır. Önceki örneği göz önüne alırsak:

```python
while num_bills * bill_thickness < sears_height:
    print(day, num_bills, num_bills * bill_thickness)
    day = day + 1
    num_bills = num_bills * 2

print('Number of days', day)
```

Girinti, aşağıdaki ifadeleri tekrar eden işlemler olarak gruplar:
 
```python
    print(day, num_bills, num_bills * bill_thickness)
    day = day + 1
    num_bills = num_bills * 2
```

Sondaki `print()` komutu girintilenmediği için, döngüye ait değildir. Boş satır okunabilirlik içindir. Çalışmayı etkilemez.

### Girinti hakkında temel bilgiler

* tab yerine boşluk kullanın.
* Her seviye için 4 boşluk kullanın.
* Python’a duyarlı bir editör kullanın.

Python'ın tek koşulu aynı blok içerisindeki girintilerin tutarlı olması.
Örneğin aşağıdaki kod hatalıdır:

```python
while num_bills * bill_thickness < sears_height:
    print(day, num_bills, num_bills * bill_thickness)
        day = day + 1 # ERROR
    num_bills = num_bills * 2
```

### Koşullar

`if` ifadesi bir koşul belirtmek için kullanılır:
 
```python
if a > b:
    print('Computer says no')
else:
    print('Computer says yes')
```

`elif` kullanarak birden fazla koşulun kontrolünü sağlayabilirsiniz.

```python
if a > b:
    print('Computer says no')
elif a == b:
    print('Computer says yes')
else:
    print('Computer says maybe')
```

### Yazdırma

`print` fonksiyonu aldığı değerleri tek satır halinde ekrana yazdırır.
 
```python
print('Hello world!') # 'Hello world!' yazdırır
```

Değişkenler kullanabilirsiniz. Yazılacak metin değişkenin ismi değil, değeri olur.
 
```python
x = 100
print(x) # '100' yazdırır.
```

Eğer `print` ifadesine birden fazla değer gönderilecekse bu değerler virgülle ayrılır.

```python
name = 'Jake'
print('My name is', name) # 'My name is Jake' yazdırır
```

`print()` her zaman sona yeni satır ekler.

```python
print('Hello')
print('My name is', 'Jake')
```

Ekran çıktıları:

```code
Hello
My name is Jake
```

Ekstra yeni satır eklemesi önlenebilir:

```python
print('Hello', end=' ')
print('My name is', 'Jake')
```

Bu kod aşağıdaki çıktıyı verir:

```code
Hello My name is Jake
```

### Kullanıcı Girdisi

Kullanıcı tarafından girilmiş bir satırı okumak için, `input()` fonksiyonunu kullanın:

```python
name = input('Enter your name:')
print('Your name is', name)
```

`input` kullanıcıya bir istem yazdırır ve kullanıcıların girdiği değeri döndürür. Bu küçük programlar, egzersizler ve basit hata ayıklama için kullanışlıdır. Gerçek programlarda çokça kullanılmaz.
 
### pass ifadesi

Bazen boş bir kod bloğu oluşturmanız gerekebilir. `pass` ifadesi bunun için kullanılır.

```python
if a > b:
    pass
else:
    print('Computer says false')
```

Bu ayrıca "no-op" ifadesi olarak adlandırılır. Hiçbir şey yapmaz. Sonradan eklenme durumu olan ifadeler için yer tutuculuk yapar.
 
## Egzersizler

Burası bir Python dosyası oluşturup onları çalıştırmanız gereken ilk egzersiz alanıdır. Bu aşamadan itibaren, `practical-python/Work/` klasörü içindeki dosyaları düzenlediğiniz varsayılacaktır. Uygun yeri bulmanızı sağlamak amacıyla, uygun dosya isimleriyle bir kaç başlangıç dosyası oluşturulmuştur. İlk egzersizde kullanılan `Work/bounce.py` dosyasını bulun.

### Egzersiz 1.5: Zıplayan Top

Kauçuk bir top 100 metre yüksekliğinden bırakılır ve yere her çarptığında düştüğü yüksekliğin 3/5’i oranında geri yukarı seker. İlk 10 sekişte topun yüksekliğini yazdıracak bir tablo gösteren `bounce.py` programını yazın.

Program, aşağıdakine benzer bir tablo oluşturmalı:

```code
1 60.0
2 36.0
3 21.599999999999998
4 12.959999999999999
5 7.775999999999999
6 4.6655999999999995
7 2.7993599999999996
8 1.6796159999999998
9 1.0077695999999998
10 0.6046617599999998
```

*Not: round() fonksiyonunu kullanarak çıktıyı biraz daha temiz hale getirebilirsiniz. Çıktıyı 4 haneye yuvarlamak için round() kullanmayı deneyin.*

```code
1 60.0
2 36.0
3 21.6
4 12.96
5 7.776
6 4.6656
7 2.7994
8 1.6796
9 1.0078
10 0.6047
```

### Egzersiz 1.6: Hata Ayıklama

Aşağıdaki kod parçası Sears kule probleminden bir parça içerir. Aynı zamanda içinde bir hata vardır.
 
```python
# sears.py

bill_thickness = 0.11 * 0.001    # Metre (0.11 mm)
sears_height   = 442             # Yükseklik (meters)
num_bills      = 1
day            = 1

while num_bills * bill_thickness < sears_height:
    print(day, num_bills, num_bills * bill_thickness)
    day = days + 1
    num_bills = num_bills * 2

print('Number of days', day)
print('Number of bills', num_bills)
print('Final height', num_bills * bill_thickness)
```

Yukarıda görünen kodu kopyalayıp `sears.py` isimli yeni bir program içine yapıştırın.

 Kodu çalıştırdığınızda programı çöktüren aşağıdaki hatayı alırsınız.

```code
Traceback (most recent call last):
  File "sears.py", line 10, in <module>
    day = days + 1
NameError: name 'days' is not defined
```

Hataları okumak Python kodunun önemli bir parçasıdır. Eğer programınız çökerse, traceback mesajınızın en son satırı programın çökmesinin asıl sebebidir. Onun üstünde kaynak kodunun bir parçasını ve ardından belirtici bir dosya ismi ile satır numarası görmelisiniz.

* Hangi satır hatalı?
* Hata ne?
* Hatayı düzelt
* Programı başarıyla çalıştır


[Contents](../Contents.md) \| [Previous (1.1 Python)](01_Python.md) \| [Next (1.3 Numbers)](03_Numbers.md)


