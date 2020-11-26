[Contents](../Contents.md) \| [Previous (3.4 Modules)](04_Modules.md) \| [Next (3.6 Design Discussion)](06_Design_discussion.md)

# 3.5 Main Modülü

Bu bölüm main programı (main modülü) kavramını tanıtmaktadır.

### Main Fonksiyonları

Pek çok programlama dilinde, *main* fonksiyonu/methodu kavramı vardır.

```c
// c / c++
int main(int argc, char *argv[]) {
    ...
}
```

```java
// java
class myprog {
    public static void main(String args[]) {
        ...
    }
}
```

Bu, bir uygulama başlatıldığında çalıştırılan ilk fonksiyondur.

### Python Main Modülü

Python'un *main* fonksiyonu veya methodu yoktur. Bunun yerine bir *main* modülü vardır. Bu *main modül* ilk çalıştırılan kaynak dosyadır.

```bash
bash % python3 prog.py
...
```

Başlangıçta yorumlayıcıya verdiğiniz dosya *main* olur. Adı önemli değildir.

### `__main__` check

Bu kuralı kullanmak için ana komut dosyası olarak çalışan modüllerin standart uygulaması:

```python
# prog.py
...
if __name__ == '__main__':
    # Ana program olarak çalışıyor ...
    ifadeler
    ...
```
`if` ifadesinin içindeki ifadeler ana program haline gelir.

### Ana Programlar vs Kütüphane Import'u

Herhangi bir python dosyası main olarak veya kütüphanenin importu olarak çalışabilir:

```bash
bash % python3 prog.py # Ana program olarak çalışıyor
```

```python
import prog   # Kütüphane importu olarak çalışıyor
```

Her iki durumda da, `__name__` modülün adıdır. Ancak, main olarak çalışıyorsa yalnızca `__main__` olarak ayarlanacaktır.

Genellikle ana programın parçası olan ifadelerin bir kütüphane 
importunda yürütülmesini istemezsiniz. Bu nedenle, her iki şekilde de
kullanılabilecek bir `if-` kontrol koduna sahip olmak yaygındır.

```python
if __name__ == '__main__':
    # Import ile yüklenirse çalışmaz ...
```

### Program Şablonu

İşte bir Python programı yazmak için yaygın bir program şablonu:

```python
# prog.py
# Import ifadeleri (kütüphaneler)
import modules

# Fonksiyonlar
def spam():
    ...

def blah():
    ...

# Main fonksiyonu
def main():
    ...

if __name__ == '__main__':
    main()
```

### Komut Satırı Araçları

Python genellikle komut satırı araçları için kullanılır.

```bash
bash % python3 report.py portfolio.csv prices.csv
```

Bu, komut dosyalarının terminalden /shell'den yürütüldüğü anlamına gelir.
Yaygın kullanım durumları: otomasyon, arka plan görevleri vb. içindir.

### Komut Satırı Argümanları

Komut satırı, metin dizelerinin bir listesidir.

```bash
bash % python3 report.py portfolio.csv prices.csv
```

Bu metin dizeleri listesi `sys.argv`'de bulunur.

```python
# Önceki bash komutunda
sys.argv # ['report.py, 'portfolio.csv', 'prices.csv']
```

İşte argümanları işlemenin basit bir örneği:

```python
import sys

if len(sys.argv) != 3:
    raise SystemExit(f'Usage: {sys.argv[0]} ' 'portfile pricefile')
portfile = sys.argv[1]
pricefile = sys.argv[2]
...
```

### Standard I/O

Standard Input / Output (veya stdio), normal dosyalarla aynı şekilde çalışan dosyalardır.

```python
sys.stdout
sys.stderr
sys.stdin
```

Varsayılan olarak, print (yazdırma) `sys.stdout`'a yönlendirilir. Input (girdi)
`sys.stdin`'den okunur. İzlemeler ve hatalar `sys.stderr`'e yönlendirilir.

Stdio'nun terminallere, dosyalara, pipelara vb. bağlanabileceğini unutmayın.

```bash
bash % python3 prog.py > results.txt
# veya
bash % cmd1 | python3 prog.py | cmd2
```

### Ortam Değişkenleri

Ortam değişkenleri kabukta (Shell'de) ayarlanır.

```bash
bash % setenv NAME dave
bash % setenv RSH ssh
bash % python3 prog.py
```

`os.environ`, bu değerleri içeren bir kütüphanedir.

```python
import os

name = os.environ['NAME'] # 'dave'
```

Değişiklikler, daha sonra program tarafından başlatılan tüm alt işlemlere yansıtılır.

### Program Çıkışı

Program çıkışı, istisnalar aracılığıyla gerçekleştirilir.

```python
raise SystemExit
raise SystemExit(exitcode)
raise SystemExit('Informative message')
```

Alternatif olarak:

```python
import sys
sys.exit(exitcode)
```

Sıfır olmayan bir çıkış kodu bir hatayı gösterir.

### `#!` Satırı

Unix'de `#!` satırı, Python olarak bir komut dosyası başlatabilir.
Aşağıdakileri komut dosyanızın ilk satırına ekleyin.

```python
#!/usr/bin/env python3
# prog.py
...
```

Yürütülebilir izin gerektirir.

```bash
bash % chmod +x prog.py
# Sonra yürütebilirsin
bash % prog.py
... output ...
```

*Not: Windows'taki Python Başlatıcı, dil sürümünü gösteren #! satırı da arar.*

### Komut Dosyası Şablonu

Son olarak burada, komut satırı betikleri olarak çalışan Python programları için ortak bir
kod şablonu verilmiştir:

```python
#!/usr/bin/env python3
# prog.py

# Import ifadeleri (kütüphaneler)
import modules

# Fonksiyonlar
def spam():
    ...

def blah():
    ...

# Ana fonksiyon
def main(argv):
    # Komut satırı argümanlarını, ortamı vb. ayrıştırın
    ...

if __name__ == '__main__':
    import sys
    main(sys.argv)
```

## Egzersizler

### Egzersiz 3.15: `main()` fonksiyonları

`report.py` dosyasında, komut satırı seçeneklerinin bir listesini kabul eden ve
öncekiyle aynı çıktıyı üreten bir `main()` fonksiyonu ekleyin. Bunu şu şekilde 
çalıştırabilmelisiniz:

```python
>>> import report
>>> report.main(['report.py', 'Data/portfolio.csv', 'Data/prices.csv'])
      Name     Shares      Price     Change
---------- ---------- ---------- ----------
        AA        100       9.22     -22.98
       IBM         50     106.28      15.18
       CAT        150      35.46     -47.98
      MSFT        200      20.89     -30.34
        GE         95      13.48     -26.89
      MSFT         50      20.89     -44.21
       IBM        100     106.28      35.84
>>>
```

`pcost.py` dosyasını, benzer bir `main()` fonksiyonuna sahip olacak şekilde değiştirin:

```python
>>> import pcost
>>> pcost.main(['pcost.py', 'Data/portfolio.csv'])
Total cost: 44671.15
>>>
```

### Egzersiz 3.16: Komut Dosyaları Oluşturma

`report.py` ve `pcost.py` programlarını, komut satırında bir komut dosyası olarak
çalışabilecek şekilde değiştirin:

```bash
bash $ python3 report.py Data/portfolio.csv Data/prices.csv
      Name     Shares      Price     Change
---------- ---------- ---------- ----------
        AA        100       9.22     -22.98
       IBM         50     106.28      15.18
       CAT        150      35.46     -47.98
      MSFT        200      20.89     -30.34
        GE         95      13.48     -26.89
      MSFT         50      20.89     -44.21
       IBM        100     106.28      35.84

bash $ python3 pcost.py Data/portfolio.csv
Total cost: 44671.15
```

[Contents](../Contents.md) \| [Previous (3.4 Modules)](04_Modules.md) \| [Next (3.6 Design Discussion)](06_Design_discussion.md)
