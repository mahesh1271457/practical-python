[Contents](../Contents.md) \| [Previous (7.5 Decorated Methods)](../07_Advanced_Topics/05_Decorated_methods.md) \| [Next (8.2 Logging)](02_Logging.md)

# 8.1 Test etme

## Test etmek harika, hata ayıklama berbat

Python'un dinamik yapısı, testi çoğu uygulama için kritik
derecede önemli hale getirir. Hatalarınızı bulacak bir derleyici yoktur.
Hataları bulmanın tek yolu, kodu çalıştırmak ve tüm özelliklerini 
denediğinizden emin olmaktır.

## Assert deyimi

`Assert` ifadesi, program için dahili bir kontroldür.
Bir ifade true değilse, `AssertionError` istisnası oluşturur.

`Assert` ifadesi sentaksı

```python
assert <expression> [, 'Diagnostic message']
```
Örneğin:

```python
assert isinstance(10, int), 'Expected int'
```

Kullanıcı girişini kontrol etmek için kullanılmamalıdır 
(yani, bir web formuna veya başka bir şeye girilen veriler).
Amacı daha çok iç kontroller ve değişmezler içindir 
(her zaman doğru olması gereken koşullar).

### Contract Programlama

Design By Contract olarak da bilinen, asset deyiminin liberal 
kullanımı bir yazılım tasarımı yaklaşımıdır.Yazılım tasarımcılarının,
yazılımın bileşenleri için kesin arayüz spesifikasyonları tanımlamaları gerektiğini belirtir.

Örneğin, bir fonksiyonun tüm girdilerine assert deyimini koyabilirsiniz.

```python
def add(x, y):
    assert isinstance(x, int), 'Expected int'
    assert isinstance(y, int), 'Expected int'
    return x + y
```

Girdileri kontrol etmek,uygun argümanlar kullanmayan
çağıranları hemen yakalar

```python
>>> add(2, 3)
5
>>> add('2', '3')
Traceback (most recent call last):
...
AssertionError: Expected int
>>>
```

### Satıriçi Testler

Assertion'lar basit testler için de kullanılabilir.

```python
def add(x, y):
    return x + y

assert add(2,2) == 4
```

Bu şekilde testi kodunuzla aynı modüle dahil etmiş olursunuz.

 *Avantaj: Kod açıkça kırılmışsa, modülü içe aktarma 
 girişimleri çökecektir. *

Bu, kapsamlı testler için önerilmez.  Daha çok temel bir "smoke testi".
Fonksiyon herhangi bir örnek üzerinde çalışıyor mu?
Çalışmıyorsa, o zaman kesinlikle bir şey kırılmıştır.

### `unittest` Modülü

Bir kodunuz olduğunu varsayalım.

```python
# simple.py

def add(x, y):
    return x + y
```

Şimdi, test etmek istediğinizi varsayalım. Bunun gibi ayrı bir test dosyası oluşturun.

```python
# test_simple.py

import simple
import unittest
```

Ardından bir test sınıfı tanımlayın.

```python
# test_simple.py

import simple
import unittest

# unittest.TestCase den kalıtım aldığına dikkat edin.

class TestAdd(unittest.TestCase):
    ...
```

Test sınıfı `unittest.TestCase` den kalıtım almalıdır.

Test sınıfında, test yöntemlerini tanımlarsınız.

```python
# test_simple.py

import simple
import unittest

# unittest.TestCase den kalıtım aldığına dikkat edin.
class TestAdd(unittest.TestCase):
    def test_simple(self):
        # Test with simple integer arguments
        r = simple.add(2, 2)
        self.assertEqual(r, 5)
    def test_str(self):
        # Test with strings
        r = simple.add('hello', 'world')
        self.assertEqual(r, 'helloworld')
```

*Önemli: Her metot `test` ile başlamalıdır.

### `unittest` kullanımı

"Unittest" ile birlikte gelen birkaç yerleşik assert vardır. Her biri farklı bir şey assert eder.

```python
# Doğru ise assert et
self.assertTrue(expr)

# Assert x == y
self.assertEqual(x,y)

# Assert x != y
self.assertNotEqual(x,y)

# Assert x is near y
self.assertAlmostEqual(x,y,places)

# Assert callable(arg1,arg2,...) raises vs.
self.assertRaises(exc, callable, arg1, arg2, ...)
```

Bu, kapsamlı bir liste değildir.  Modülde başka assert'ler de var.

### "Unittest" çalıştırmak

Testleri çalıştırmak için kodu komut dosyasına dönüştürün.

```python
# test_simple.py

...

if __name__ == '__main__':
    unittest.main()
```

ardından test dosyasında Python'u çalıştırın.

```bash
bash % python3 test_simple.py
F.
========================================================
FAIL: test_simple (__main__.TestAdd)
--------------------------------------------------------
Traceback (most recent call last):
  File "testsimple.py", line 8, in test_simple
    self.assertEqual(r, 5)
AssertionError: 4 != 5
--------------------------------------------------------
Ran 2 tests in 0.000s
FAILED (failures=1)
```

### Yorum

Etkili birim testi bir sanattır ve büyük uygulamalar için 
oldukça karmaşık hale gelebilir.

`unittest` modülü, test runner'ları, sonuçların toplanması ve testin
diğer yönleriyle ilgili çok sayıda seçeneğe sahiptir.Ayrıntılar için 
belgelere bakın.

### Üçüncü Taraf Test Araçları

Yerleşik `unittest` modülü, her yerde kullanılabilir olma
avantajına sahiptir--Python'un bir parçasıdır.Bununla birlikte, 
birçok programcı bunu oldukça ayrıntılı bulmaktadır.
Popüler alternatif ise budur [pytest](https://docs.pytest.org/en/latest/).  
Pytest ile, test dosyanız aşağıdaki gibi basitleştirilir:

```python
# test_simple.py
import simple

def test_simple():
    assert simple.add(2,2) == 4

def test_str():
    assert simple.add('merhaba','dünya') == 'merhabadünya'
```

Testleri çalıştırmak için, `python -m pytest` gibi bir komut yazmanız yeterlidir.
Tüm testleri keşfedecek ve çalıştıracaktır.

Bu örnekten çok daha fazla `pytest` var, ancak denemeye karar verirseniz 
başlamak genellikle oldukça kolaydır.

## Alıştırmalar

Bu alıştırmada, Python'un `unittest` modülünü kullanmanın temel mekaniğini 
keşfedeceksiniz.  


Önceki alıştırmalarda, `Stock` sınıfı içeren bir `stock.py dosyası yazmıştınız.

Bu alıştırma için, yazılan özellikleri içeren [Exercise
7.9](../07_Advanced_Topics/03_Returning_functions) için yazılmış kodu 
kullandığınız varsayılıyordu. Herhangi bir nedenle bu işe yaramazsa,
`Çözümler/7_9` dan çalışma dizininize kopyalamak isteyebilirsiniz.
directory.

### Alıştırma 8.1: Birim Testleri Yazma

Ayrı bir `test_stock.py dosyasında, `Stock` sınıfı için bir birim testi kümesi yazın. 
Başlamanız için, örnek oluşturmayı test eden küçük bir kod parçası aşağıda verilmiştir:   


```python
# test_stock.py

import unittest
import stock

class TestStock(unittest.TestCase):
    def test_create(self):
        s = stock.Stock('GOOG', 100, 490.1)
        self.assertEqual(s.name, 'GOOG')
        self.assertEqual(s.shares, 100)
        self.assertEqual(s.price, 490.1)

if __name__ == '__main__':
    unittest.main()
```

Birim testlerinizi çalıştırın. Şuna benzeyen bazı çıktılar almalısınız:

```
.
----------------------------------------------------------------------
Ran 1 tests in 0.000s

OK
```

Çalıştığından emin olduğunuzda, aşağıdakileri kontrol eden ek birim testleri yazın:

- `s.cost` özelliğinin doğru değeri döndürdüğünden emin olun (49010.0)
- `s.sell()` yönteminin doğru çalıştığından emin olun. 
   Buna göre `s.shares` değerini azaltmalıdır.
- `s.shares` özelliğinin tam sayı olmayan bir değere ayarlanamayacağından emin olun.

Son kısım için, bir istisna olup olmadığını kontrol etmeniz gerekecek.
Bunu yapmanın kolay bir yolu şuna benzer bir kod kullanmaktır:

```python
class TestStock(unittest.TestCase):
    ...
    def test_bad_shares(self):
         s = stock.Stock('GOOG', 100, 490.1)
         with self.assertRaises(TypeError):
             s.shares = '100'
```

[İçindekiler](../Contents.md) \| [Önceki (7.5 Süslü Metotlar)](../07_Advanced_Topics/05_Decorated_methods.md) \| [Sonraki (8.2 Raporlama)](02_Logging.md)