[Contents](../Contents.md) \| [Previous (8.2 Logging)](02_Logging.md) \| [Next (9 Packages)](../09_Packages/00_Overview.md)

# 8.3 Hata Ayıklama

### Hata Ayıklama İpuçları

Programın çalışmadı...

```bash
bash % python3 blah.py
Traceback (most recent call last):
  File "blah.py", line 13, in ?
    foo()
  File "blah.py", line 10, in foo
    bar()
  File "blah.py", line 7, in bar
    spam()
  File "blah.py", 4, in spam
    line x.append(3)
AttributeError: 'int' object has no attribute 'append'
```

Şimdi ne olacak?!

### Dönütleri Okumak

Son satır bizim programımızın çalışmamasının nedenidir.

```bash
bash % python3 blah.py
Traceback (most recent call last):
  File "blah.py", line 13, in ?
    foo()
  File "blah.py", line 10, in foo
    bar()
  File "blah.py", line 7, in bar
    spam()
  File "blah.py", 4, in spam
    line x.append(3)
# Programın çalışmamasının nedeni
AttributeError: 'int' object has no attribute 'append'
```

Ama her zaman okumak veya anlamak o kadar da kolay değildir.

*ÖNEMLİ İPUCU: Dönütü Google'da aratabilirsiniz.*

### REPL'i Kullanmak

Bir script'i çalıştırırken Python'ı canlı tutmak için `-i` opsiyonunu kullanın.

```bash
bash % python3 -i blah.py
Traceback (most recent call last):
  File "blah.py", line 13, in ?
    foo()
  File "blah.py", line 10, in foo
    bar()
  File "blah.py", line 7, in bar
    spam()
  File "blah.py", 4, in spam
    line x.append(3)
AttributeError: 'int' object has no attribute 'append'
>>>
```

Bu opsiyon yorumlayıcının durumunu korur. Bu sizin programın çökmesinin ardından hatayı araştırabileceğiniz anlamına gelir. 
Değişken değerlerini ve diğer durumları kontrol ediyoruz. 

### Print ile Hata Ayıklama

`print()` ile hata ayıklama oldukça yaygındır.

*İpucu: `repr()` kullandığınızdan emin olun*.

```python
def spam(x):
    print('DEBUG:', repr(x))
    ...
```

`repr()` size bir değerin kesin temsilini gösterir. *Güzel* bir çıktıyı değil.

```python
>>> from decimal import Decimal
>>> x = Decimal('3.4')
# `repr` OLMADAN
>>> print(x)
3.4
# `repr` ILE
>>> print(repr(x))
Decimal('3.4')
>>>
```

### Python Hata Ayıklayıcısı

Manuel olarak hata ayıklayıcı programınızın içinde çalıştırabilirsiniz.

```python
def some_function():
    ...
    breakpoint()      # Hata ayıklayıcıyı gir (Python 3.7+)
    ...
```

Burada `breakpoint()` çağrısında hata ayıklayıcı çalışır.

Python'ın eski versiyonlarında, aşağıdaki gibi yapardınız.  Bazen bunun diğer rehberlerde
bahsedildiğini göreceksiniz.

```python
import pdb
...
pdb.set_trace()       # `breakpoint()` yerine
...
```

### Hata ayıklayıcı Altında Çalıştırmak

Aynı zamanda, bütün programınızı hata ayıklayıcısı altında çalıştırabilirsiniz.

```bash
bash % python3 -m pdb someprogram.py
```

Böylece otomatik olarak ilk ifadeden önce hata ayıklayıcısına girecektir. Bu sizin konfigürasyonları değiştirmenize veya kırılma noktalarını
ayarlamanıza imkan tanır.

Yaygın hata ayıklayıcı komutları:

```code
(Pdb) help            # Yardım al
(Pdb) w(here)         # Yığının nerde olduğunu yazdır
(Pdb) d(own)          # Bir yığın seviyesi aşağı in
(Pdb) u(p)            # Bir yığın seviyesi yukarı git
(Pdb) b(reak) loc     # Bir kırılma noktası ayarla
(Pdb) s(tep)          # Bir yönergeyi çalıştır
(Pdb) c(ontinue)      # Çalıştırmaya devam et
(Pdb) l(ist)          # Kaynak kodunu listele
(Pdb) a(rgs)          # Mevcut fonksiyonun args'ını yazdır
(Pdb) !statement      # İfadeyi çalıştır
```

Kırılma noktalarının konumu aşağıdakilerden biridir.

```code
(Pdb) b 45            # Mevcut dosyadaki Satır 45
(Pdb) b file.py:45    # file.py daki Satır 34
(Pdb) b foo           # Mevcut dosyadaki foo() fonksiyonu
(Pdb) b module.foo    # Mevcut modüldeki foo() fonksiyonu 
```

## Alıştırmalar

### Alıştırma 8.4:  Hatalar? Hangi Hatalar?

Çalışıyor. Yola devam et!

[Contents](../Contents.md) \| [Previous (8.2 Logging)](02_Logging.md) \| [Next (9 Packages)](../09_Packages/00_Overview.md)
