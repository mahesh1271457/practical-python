[Contents](../Contents.md) \| [Previous (8.1 Testing)](01_Testing.md) \| [Next (8.3 Debugging)](03_Debugging.md)

# 8.2 Logging

Bu bölümde kısaca logging modülü anlatılıyor.

### logging Modülü

logging modülü teşhis bilgilerini(except bloğunun çıktısı) kayıt eden standard Python kütüphanesidir. 
Aynı zamanda, birçok gelişmiş işlevi bulunan büyük bir modüldür. 
Şimdi ne kadar kullanışlı olduğunu göstermek için basit bir örnek yapacağız.

### İstisnaları(exceptions) Yeniden Gözden Geçirmek

Bu örnekte, aşağıda olduğu gibi bir `parse()` adlı bir fonksiyon tanımladık.

```python
# fileparse.py
def parse(f, types=None, names=None, delimiter=None):
    records = []
    for line in f:
        line = line.strip()
        if not line: continue
        try:
            records.append(split(line,types,names,delimiter))
        except ValueError as e:
            print("Couldn't parse :", line)
            print("Reason :", e)
    return records
```

`try-except` ifadesine odaklan. `except` bloğunda ne yapmalısın?

Uyarı mesajı yazdırmalı mısın?

```python
try:
    records.append(split(line,types,names,delimiter))
except ValueError as e:
    print("Couldn't parse :", line)
    print("Reason :", e)
```

Yoksa sadece görmezden mi gelmelisin?

```python
try:
    records.append(split(line,types,names,delimiter))
except ValueError as e:
    pass
```

Her iki çözüm de tatmin edici değil çünkü *ikisini de* isteyebilirsiniz(kullanıcı isteğine bağlı).

### logging'i Kullanma

`logging` modülü bu durumu çözebilir.

```python
# fileparse.py
import logging
log = logging.getLogger(__name__)

def parse(f,types=None,names=None,delimiter=None):
    ...
    try:
        records.append(split(line,types,names,delimiter))
    except ValueError as e:
        log.warning("Couldn't parse : %s", line)
        log.debug("Reason : %s", e)
```

Yazdığımız kodu uyarı mesajlarını bildirmesi veya `logging.getLogger(__name__)`
ile oluşturduğumuz özel bir `Logger` nesnesini saptaması için değiştirdik.

### Logging Temel Ögeleri

Bir logger nesnesi oluştur.

```python
log = logging.getLogger(name)   # name bir string ifade
```

Log mesajlarını saptıyoruz.

```python
log.critical(message [, args])
log.error(message [, args])
log.warning(message [, args])
log.info(message [, args])
log.debug(message [, args])
```

*Her bir metod farklı seviye önem derecesini temsil eder.*

Her biri biçimlendirilmiş log mesajları oluşturur. Mesajı oluşturmak için`args` `%` operatörüyle birlikte kullanılır.

```python
logmsg = message % args # Log'a yazıldı
```

### Logging Yapılandırılması

logging durumları farklı şekillerde yapılandırılır.

```python
# main.py

...

if __name__ == '__main__':
    import logging
    logging.basicConfig(
        filename  = 'app.log',      # Log çıktı dosyası
        level     = logging.INFO,   # Çıktı seviyesi
    )
```

Genelde, bu yapılandırma bir kereliğine program başlatıldığında yapılır. Yapılandırma, logging çağrılarını yapması için yazdığımız koddan ayrıdır.

### Yorumlar

Logging çokça yapılandırılabilir.  Her kısmını ayarlayabilirsiniz:
çıktı dosyalarını, seviyelerini, mesaj formatarını, vs. Logging'i kullanan kodun bunun için endişelenmesine gerek yok.

## Alıştırmalar

### Alıştırma 8.2: logging'i modülümüze ekleme

`fileparse.py` da, aşağıdakiler gibi kötü girdiden dolayı istisnalardan kaynaklanan bir hata idaresi(error handling) vardı.

```python
# fileparse.py
import csv

def parse_csv(lines, select=None, types=None, has_headers=True, delimiter=',', silence_errors=False):
    '''
    CSV dosyasını tip değiştirme kullanarak kayıtların listesine ayrıştır.
    '''
    if select and not has_headers:
        raise RuntimeError('select requires column headers')

    rows = csv.reader(lines, delimiter=delimiter)

    # Dosya başlıklarını oku (eğer varsa)
    headers = next(rows) if has_headers else []

    # Eğer spesifik sütünlar seçilmişse, filtrelemek için göstergeler ve çıktı sütünlarını oluştur
    if select:
        indices = [ headers.index(colname) for colname in select ]
        headers = select

    records = []
    for rowno, row in enumerate(rows, 1):
        if not row:     # Verisi olmayan sütünları geç
            continue

        # Eğer spesifik sütün endeksleri seçilmişse, onları al
        if select:
            row = [ row[index] for index in indices]

        # Satıra tip dönüştürmesini uygula
        if types:
            try:
                row = [func(val) for func, val in zip(types, row)]
            except ValueError as e:
                if not silence_errors:
                    print(f"Row {rowno}: Couldn't convert {row}")
                    print(f"Row {rowno}: Reason {e}")
                continue

        # Bir demet veya sözlük oluştur
        if headers:
            record = dict(zip(headers, row))
        else:
            record = tuple(row)
        records.append(record)

    return records
```

Teşhis bilgilerini saptayan print ifadelerine dikkat edin. O print ifadelerini logging işlemlerine dönüştürmek nispeten çok kolay. Kodu şöyle değiştirelim:

```python
# fileparse.py
import csv
import logging
log = logging.getLogger(__name__)

def parse_csv(lines, select=None, types=None, has_headers=True, delimiter=',', silence_errors=False):
    '''
    CSV dosyasını tip değiştirme kullanarak kayıtların listesine ayrıştır.
    '''
    if select and not has_headers:
        raise RuntimeError('select requires column headers')

    rows = csv.reader(lines, delimiter=delimiter)

    # Dosya başlıklarını oku (eğer varsa)
    headers = next(rows) if has_headers else []

    # Eğer spesifik sütünlar seçilmişse, filtrelemek için göstergeler ve çıktı sütünlarını oluştur
    if select:
        indices = [ headers.index(colname) for colname in select ]
        headers = select

    records = []
    for rowno, row in enumerate(rows, 1):
        if not row:     # Skip rows with no data
            continue

        # Eğer spesifik sütün endeksleri seçilmişse, onları al
        if select:
            row = [ row[index] for index in indices]

        # Satıra tip dönüştürmesini uygula
        if types:
            try:
                row = [func(val) for func, val in zip(types, row)]
            except ValueError as e:
                if not silence_errors:
                    log.warning("Row %d: Couldn't convert %s", rowno, row)
                    log.debug("Row %d: Reason %s", rowno, e)
                continue

        # Bir sözlük veya demet oluştur
        if headers:
            record = dict(zip(headers, row))
        else:
            record = tuple(row)
        records.append(record)

    return records
```

Şimdi bu değişiklikleri yaptın, şimdi bir de kötü veriler üzerinde kodu dene.

```python
>>> import report
>>> a = report.read_portfolio('Data/missing.csv')
Row 4: Bad row: ['MSFT', '', '51.23']
Row 7: Bad row: ['IBM', '', '70.44']
>>>
```

Eğer hiçbir şey yapmazsan, alacağın mesajlar sadece `WARNING`
seviyesi ve yukarısı için olacak.  Çıktı basit bir print ifadesine benzeyecek.
Ama, eğer logging modülünü yapılandırırsan, logging seviyeleri, modülü ve daha fazlası hakkında ek bilgiler alacaksın.  Bunu görmek için aşağıdaki adımları yap:

```python
>>> import logging
>>> logging.basicConfig()
>>> a = report.read_portfolio('Data/missing.csv')
WARNING:fileparse:Row 4: Bad row: ['MSFT', '', '51.23']
WARNING:fileparse:Row 7: Bad row: ['IBM', '', '70.44']
>>>
```

`log.debug()`işleminden bir çıktı olmadığını göreceksin. Seviyeyi görmek için bunu yaz:

```
>>> logging.getLogger('fileparse').level = logging.DEBUG
>>> a = report.read_portfolio('Data/missing.csv')
WARNING:fileparse:Row 4: Bad row: ['MSFT', '', '51.23']
DEBUG:fileparse:Row 4: Reason: invalid literal for int() with base 10: ''
WARNING:fileparse:Row 7: Bad row: ['IBM', '', '70.44']
DEBUG:fileparse:Row 7: Reason: invalid literal for int() with base 10: ''
>>>
```

En kritik logging mesajları dışındakilerini bırak:

```
>>> logging.getLogger('fileparse').level=logging.CRITICAL
>>> a = report.read_portfolio('Data/missing.csv')
>>>
```

### Exercise 8.3: Logging'i Bir Programa Eklemek 

logging'i bir uygulamaya eklemek için, öncelikle logging modülünü 
main modülünde başlatmak için gereken bir düzene ihtiyacın var.  Bunu yapmanın bir yolu, uygulamaya aşağıdakine benzer bir setup kodu eklemektir.

```
# Bu dosya logging modülünün temel konfigürasyonunu kurar.
# logging çıktısını ayarlamak için ayarları değiştirebilirsiniz.
import logging
logging.basicConfig(
    filename = 'app.log',            # Name of the log file (omit to use stderr)
    filemode = 'w',                  # File mode (use 'a' to append)
    level    = logging.WARNING,      # Logging level (DEBUG, INFO, WARNING, ERROR, or CRITICAL)
)
```

Tekrardan, aşağıdaki kodu programının başlangıç aşamalarında bir yere koyman gerekecek.  
Mesela, bunu `report.py` adlı programının neresine koyardın?

[Contents](../Contents.md) \| [Previous (8.1 Testing)](01_Testing.md) \| [Next (8.3 Debugging)](03_Debugging.md)