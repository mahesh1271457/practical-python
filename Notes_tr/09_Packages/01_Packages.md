[Contents](../Contents.md) \| [Previous (8.3 Debugging)](../08_Testing_debugging/03_Debugging.md) \| [Next (9.2 Third Party Packages)](02_Third_party.md)

# 9.1 Paketler
Eğer büyük bir program yazıyorsanız, programınızı geniş ve birbirinden bağımsız dosyalarla organize etmek istemezsiniz. Bu bölüm paket kavramının ne olduğunu tanıtmakta.


### Modüller

Her python dosyası aslında bir modüldür.

```python
# foo.py
def grok(a):
    ...
def spam(b):
    ...
```


`İmport` komutu yükleme işini ve içeri alınan modüllerin *işlemesini* sağlar.

```python
# program.py
import foo

a = foo.grok(2)
b = foo.spam('Hello')
...
```

### Paketler vs. Modüller

Büyük parçalar halindeki kodlarda, modülleri paketlerin içinde olacak şekilde organize etmek yaygın bir yöntemdir.


```code
# From this
pcost.py
report.py
fileparse.py

# To this
porty/
    __init__.py
    pcost.py
    report.py
    fileparse.py
```

Bir isim seçip üst düzey bir dizin oluşturuyorsun. Yukardaki örnekteki `porty` komutu (bu adı seçmek en önemli ilk adımdır).

Bir `__init__.py`  dosyasını çalıştığınız dizine ekleyin. Boş da olabilir.


Kaynak dosyalarınızı ise aynı dizine koyun.

### Paketleri Kullanmak

Paketler import işlemleri için bir isim alanı işlevini yerine getirir.

Bunun anlamı artık çok düzeyli import işlemlerimizin olduğu.

```python
import porty.report
port = porty.report.read_portfolio('port.csv')
```

İmport komutunun birden fazla çeşidi vardır.


```python
from porty import report
port = report.read_portfolio('portfolio.csv')

from porty.report import read_portfolio
port = read_portfolio('portfolio.csv')
```

### 2 Problem

Bu yaklaşımda iki ana problem var.

* Aynı paket sonundaki dosyalar arasında import işlemi uygular.
* Paketin içine yerleştirilen ana komut dosyaları bozulur.

Temel olarak bunlar dışında bu yöntem çalışır.

### Problem: Imports (İçe Aktarma)

Aynı paketteki dosyalar arasındaki içe aktarmalar import edilen paket ismini içermek zorundadır. Yapıyı hatırlayın.

```code
porty/
    __init__.py
    pcost.py
    report.py
    fileparse.py
```

Değiştirilmiş İçe Aktarma Örneği


```python
# report.py
from porty import fileparse

def read_portfolio(filename):
    return fileparse.parse_csv(...)
```

Tüm içe aktarmalar *kesindir*, göreceli değildir.

```python
# report.py
import fileparse    # BREAKS. fileparse not found

...
```

### Göreceli İçe Aktarmalar

Direkt olarak paket ismini kullanmak yerine, bulunduğunuz paketi ifade etmek için `.`  kullanabilirsiniz. 

```python
# report.py
from . import fileparse

def read_portfolio(filename):
    return fileparse.parse_csv(...)
```

Syntax:

```python
from . import modname
```

Bu paketleri yeniden adlandırma işlemini daha kolay bir hale getirir.


### Problem: Main Scripts (Ana Kodlar)

Bir paket alt modülünü ana main script breaks olarak çalıştırmak.

```bash
bash $ python porty/pcost.py # BREAKS
...
```

*Sebep: Python'u tek bir dosya üzerinde çalıştırıyorsunuz ve Python paket yapısının geri kalanını doğru görmüyor (sys.path yanlış).
*


Bunu düzeltmek için -m komutunu kullanarak programınızı farklı bir şekilde çalıştırmanız gerekir.


```bash
bash $ python -m porty.pcost # WORKS
...
```

### `__init__.py` Dosyaları

Bu dosyaların asıl amacı modülleri birbirine bağlamaktır.

Örnek: Fonksiyonları Birleştirme


```python
# porty/__init__.py
from .pcost import portfolio_cost
from .report import portfolio_report
```

Bu kod import edilirken adların *üst düzeyde* görünmesini sağlar.

```python
from porty import portfolio_cost
portfolio_cost('portfolio.csv')
```

Çok düzeyli içe aktarmalar kullanmak yerine.


```python
from porty import pcost
pcost.portfolio_cost('portfolio.csv')
```

### Komut Dosyaları İçin Başka Bir Çözüm


Daha önce de belirtildiği gibi,kodunuzu kendi paketinizde çalıştırmak için  `-m package.module` ünü kullanmanız lazım.


```bash
bash % python3 -m porty.pcost portfolio.csv
```

Başka bir alternatif: Yeni bir top-level(üst düzey) komut dosyası yazın.

```python
#!/usr/bin/env python3
# pcost.py
import porty.pcost
import sys
porty.pcost.main(sys.argv)
```

Bu komut dosyası paketin *dışında* çalışabiliyor. Örneğin, dizinin yapısına bakalım:


```
pcost.py       # top-level-script
porty/         # package directory
    __init__.py
    pcost.py
    ...
```

### Uygulama Yapısı

Kod organizasyonu ve dosya yapısı, bir uygulamanın sürdürülebilirliğinin anahtarıdır. 

Python için "herkese uyan tek boyut" yaklaşımı yoktur. Bununla birlikte, birçok sorun için çalışan bir yapı, bunun gibi bir şeydir.
	

```code
porty-app/
  README.txt
  script.py         # SCRIPT
  porty/
    # LIBRARY CODE
    __init__.py
    pcost.py
    report.py
    fileparse.py
```

En üst düzey `porty-app`, diğer her şey için bir kapsayıcıdır (belgeler, üst düzey komut dosyaları, örnekler vb.).

Yine, üst düzey komut dosyalarının (varsa) kod paketinin dışında bulunması gerekir. Bir seviye yukarı.


```python
#!/usr/bin/env python3
# porty-app/script.py
import sys
import porty

porty.report.main(sys.argv)
```

## Alıştırmalar

Bu noktada, birkaç program içeren bir dizine sahipsiniz:


```
pcost.py          # computes portfolio cost
report.py         # Makes a report
ticker.py         # Produce a real-time stock ticker
```

Diğer işlevlere sahip çeşitli destek modülleri vardır:

```
stock.py          # Stock class
portfolio.py      # Portfolio class
fileparse.py      # CSV parsing
tableformat.py    # Formatted tables
follow.py         # Follow a log file
typedproperty.py  # Typed class properties
```

Bu alıştırmada, kodu temizleyip ortak bir pakete koyacağız.


### Alıştırma 9.1: Basit Bir Paket Yapmak

`porty/` adlı bir dizin oluşturun ve yukarıdaki tüm Python dosyalarını içine koyun. Ek olarak boş bir `__init__.py` dosyası oluşturun ve onu dizine koyun. Aşağıdaki gibi bir dosya dizininiz olmalıdır:


```
porty/
    __init__.py
    fileparse.py
    follow.py
    pcost.py
    portfolio.py
    report.py
    stock.py
    tableformat.py
    ticker.py
    typedproperty.py
```

Dizininizde bulunan `__pycache__` dosyasını kaldırın. Bu, önceden derlenmiş Python modüllerini içerir. Biz yeniden başlamak istiyoruz.

Bazı paket modüllerini içe aktarmayı deneyin:


```python
>>> import porty.report
>>> import porty.pcost
>>> import porty.ticker
```

Bu içe aktarmalar başarısız olursa, uygun dosyaya gidin ve modül içe aktarımlarını pakete göre içe aktarmayı içerecek şekilde düzeltin. Örneğin, `import fileparse` gibi bir ifade aşağıdaki şekilde değişebilir:

```
# report.py
from . import fileparse
...
```

Fileparse `from fileparse import parse_csv` gibi bir ifadeniz varsa, kodu şu şekilde değiştirin:


```
# report.py
from .fileparse import parse_csv
...
```

### Alıştırma 9.2: Uygulama Dizini Oluşturmak

Tüm kodunuzu bir "pakete" koymak bir uygulama için çoğu zaman yeterli değildir. Bazen destekleyici dosyalar, belgeler, komut dosyaları ve başka şeyler gerekebilir. Bu dosyaların yukarıda yaptığınız `porty/` dizinin DIŞINDA olması gerekir.

`porty-app` adlı yeni bir dizin oluşturun. Alıştırma 9.1'de oluşturduğunuz porty dizinini bu dizine taşıyın. `Data/portfolio.csv`ve `Data/prices.csv` test dosyalarını bu dizine kopyalayın. Ek olarak, kendinizle ilgili bazı bilgiler içeren bir `README.txt` dosyası oluşturun. Kodunuz şimdi aşağıdaki gibi düzenlenmelidir:

```
porty-app/
    portfolio.csv
    prices.csv
    README.txt
    porty/
        __init__.py
        fileparse.py
        follow.py
        pcost.py
        portfolio.py
        report.py
        stock.py
        tableformat.py
        ticker.py
        typedproperty.py
```

Kodunuzu çalıştırmak için üst düzey `porty-app/` dizininde çalıştığınızdan emin olmalısınız. Örneğin, terminalden:


```python
shell % cd porty-app
shell % python3
>>> import porty.report
>>>
```

Önceki komut dosyalarınızdan bazılarını ana program olarak çalıştırmayı deneyin:

```python
shell % cd porty-app
shell % python3 -m porty.report portfolio.csv prices.csv txt
      Name     Shares      Price     Change
---------- ---------- ---------- ----------
        AA        100       9.22     -22.98
       IBM         50     106.28      15.18
       CAT        150      35.46     -47.98
      MSFT        200      20.89     -30.34
        GE         95      13.48     -26.89
      MSFT         50      20.89     -44.21
       IBM        100     106.28      35.84

shell %
```

### Alıştırma 9.3: Üst Düzey Komut Dosyaları

`python -m` komutunu kullanmak genellikle biraz tuhaftır. Yalnızca paketlerin tuhaflıkları ile ilgilenen üst düzey bir komut dosyası yazmak isteyebilirsiniz. Yukarıdaki raporu oluşturan bir `print-report.py` komut dosyası oluşturun:


```python
#!/usr/bin/env python3
# print-report.py
import sys
from porty.report import main
main(sys.argv)
```

Bu komut dosyasını üst düzeydeki `porty-app/ dizinine yerleştirin. O konumda çalıştırabileceğinden emin olun

```
shell % cd porty-app
shell % python3 print-report.py portfolio.csv prices.csv txt
      Name     Shares      Price     Change
---------- ---------- ---------- ----------
        AA        100       9.22     -22.98
       IBM         50     106.28      15.18
       CAT        150      35.46     -47.98
      MSFT        200      20.89     -30.34
        GE         95      13.48     -26.89
      MSFT         50      20.89     -44.21
       IBM        100     106.28      35.84

shell %
```

Son olarak kodunuz şu şekilde yapılanmış olmalıdır:

```
porty-app/
    portfolio.csv
    prices.csv
    print-report.py
    README.txt
    porty/
        __init__.py
        fileparse.py
        follow.py
        pcost.py
        portfolio.py
        report.py
        stock.py
        tableformat.py
        ticker.py
        typedproperty.py
```

[Contents](../Contents.md) \| [Previous (8.3 Debugging)](../08_Testing_debugging/03_Debugging.md) \| [Next (9.2 Third Party Packages)](02_Third_party.md)
