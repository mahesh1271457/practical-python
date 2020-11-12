[Contents](../Contents.md) \| [Previous (4.1 Classes)](01_Class.md) \| [Next (4.3 Special methods)](03_Special_methods.md)

# 4.2 Kalıtım

Kalıtım, genişletilebilir programlar yazmak için yaygın olarak kullanılan bir araçtır.
Bu bölüm bu fikri araştırır.

### Giriş

Kalıtım, mevcut nesneleri özelleştirmek için kullanılır:

```python
class Parent:
    ...

class Child(Parent):
    ...
```
Yeni `Child` sınıfına, türetilmiş sınıf veya alt sınıf adı verilir.
`Parent` sınıfı, temel sınıf veya süper sınıf olarak bilinir.
`class Child(Parent):`sınıf adından sonra `Parent``()`içinde belirtilir.

### Genişletmek

Kalıtım ile, mevcut bir sınıfı alırsınız ve:

* Yeni metotlar ekler
* Mevcut metotlardan bazılarının yeniden tanımlar
* Örneklere yeni öznitelikler ekler

Sonunda **mevcut kodu genişletirsiniz**.

### Örnek

Bunun başlangıç sınıfınız olduğunu varsayalım:

```python
class Stock:
    def __init__(self, name, shares, price):
        self.name = name
        self.shares = shares
        self.price = price

    def cost(self):
        return self.shares * self.price

    def sell(self, nshares):
        self.shares -= nshares
```

Bunun herhangi bir bölümünü kalıtım yoluyla değiştirebilirsiniz.

### Yeni metot ekleme

```python
class MyStock(Stock):
    def panic(self):
        self.sell(self.shares)
```

Kullanım Örneği.

```python
>>> s = MyStock('GOOG', 100, 490.1)
>>> s.sell(25)
>>> s.shares
75
>>> s.panic()
>>> s.shares
0
>>>
```

### Mevcut bir metodu yeniden tanımlama

```python
class MyStock(Stock):
    def cost(self):
        return 1.25 * self.shares * self.price
```

Kullanım Örneği.

```python
>>> s = MyStock('GOOG', 100, 490.1)
>>> s.cost()
61262.5
>>>
```
Yeni metot eskisinin yerini alır.  Diğer metotlar etkilenmez.  Bu muazzamdır.
 
## Geçersiz kılma

Bazen bir sınıf mevcut bir metodu genişletir, ancak yeniden tanımlamanın
içinde orijinal uygulamayı kullanmak ister. Bunun için `super()` kullanın:

```python
class Stock:
    ...
    def cost(self):
        return self.shares * self.price
    ...

class MyStock(Stock):
    def cost(self):
        # Check the call to `super`
        actual_cost = super().cost()
        return 1.25 * actual_cost
```

Bir önceki versiyonu çağırmak için `super()` kullanın.

* Dikkat: Python 2'de sözdizimi daha ayrıntılıydı. *

```python
actual_cost = super(MyStock, self).cost()
```

### `__init__` ve kalıtım

"__İnit__" yeniden tanımlanırsa, parent'ı başlatmak önemlidir.

```python
class Stock:
    def __init__(self, name, shares, price):
        self.name = name
        self.shares = shares
        self.price = price

class MyStock(Stock):
    def __init__(self, name, shares, price, factor):
        # Check the call to `super` and `__init__`
        super().__init__(name, shares, price)
        self.factor = factor

    def cost(self):
        return self.factor * super().cost()
```

Daha önce gösterildiği gibi önceki sürümü çağırmak için,
`super`ile `__init__()` metodunu çağırmalısınız

### Kalıtım kullanımı

Kalıtım bazen ilgili nesneleri düzenlemek için kullanılır.

```python
class Shape:
    ...

class Circle(Shape):
    ...

class Rectangle(Shape):
    ...
```
Mantıksal bir hiyerarşi veya sınıflandırma düşünün.Bununla birlikte, 
daha yaygın (ve pratik) bir kullanım, yeniden kullanılabilir veya 
genişletilebilir kod yapmakla ilgilidir.Örneğin, bir çerçeve bir 
temel sınıfı tanımlayabilir ve size onu özelleştirmeniz için talimat verebilir.

```python
class CustomHandler(TCPHandler):
    def handle_request(self):
        ...
        # Özel işleme
```

Temel sınıf bazı genel amaçlı kodlar içerir.
Sınıfınız belirli parçaları devralır ve özelleştirir.

### "is a" İlişkisi

Kalıtım, bir tür ilişki kurar.

```python
class Shape:
    ...

class Circle(Shape):
    ...
```

Nesne örneğini kontrol edin.

```python
>>> c = Circle(4.0)
>>> isinstance(c, Shape)
True
>>>
```

* Önemli: İdeal olarak, üst sınıfın(parent) örnekleriyle çalışan herhangi bir kod,
alt sınıfın(child)örnekleriyle de çalışacaktır.

### `Nesne` temel sınıfı

Bir sınıfın ebeveyni(parent) yoksa, bazen temel olarak `Nesne` nin kullanıldığını görürsünüz.

```python
class Shape(object):
    ...
```

`Nesne`, Python'daki tüm nesnelerin ebeveynidir.

* Not: teknik olarak gerekli değildir, ancak genellikle Python 2'de gerekli kullanımdan 
kaynaklanan bir engel olarak belirtildiğini görürsünüz. Atlanırsa, sınıf yine de dolaylı
olarak `Nesne`den kalıtım alır.

### Çoklu Kalıtım

Sınıfın tanımında belirterek birden çok sınıftan kalıtım alabilirsiniz.

```python
class Mother:
    ...

class Father:
    ...

class Child(Mother, Father):
    ...
```

`Çocuk` sınıfı, özellikleri her iki ebeveynden devralır.
Oldukça hileli detaylar var.  Eğer ne yaptığınızı bilmiyorsanız, yapmayın.
Bir sonraki bölümde daha fazla bilgi verilecektir, ancak bu kursta
çoklu kalıtımdan daha fazla yararlanmayacağız.


## Alıştırmalar

Kalıtımın önemli bir kullanımı, özellikle kitaplıklarda veya çerçevelerde çeşitli 
şekillerde genişletilmesi veya özelleştirilmesi amaçlanan kod yazmaktır.
Örnek olarak, `report.py` programınızda `print_report ()`işlevini düşünün
Bunun gibi bir şeye benzemeli:

```python
def print_report(reportdata):
    '''
   (name, shares, price, change) demet listesinden güzel biçimlendirilmiş bir tablo yazdırın.
    '''
    headers = ('Name','Shares','Price','Change')
    print('%10s %10s %10s %10s' % headers)
    print(('-'*10 + ' ')*len(headers))
    for row in reportdata:
        print('%10s %10d %10.2f %10.2f' % row)
```

Rapor programınızı çalıştırdığınızda, aşağıdaki gibi çıktılar almalısınız:

```
>>> import report
>>> report.portfolio_report('Data/portfolio.csv', 'Data/prices.csv')
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

### Egzersiz 4.5: Bir Genişletilebilirlik Problemi

Düz metin, HTML, CSV, veya XML gibi çeşitli farklı çıktı biçimlerini 
desteklemek için `print_report()` fonksiyonunu değiştirmek istediğinizi
varsayalım.  Bunu yapmak için, her şeyi yapan devasa bir fonksiyonu
yazmaya çalışabilirsiniz.Bununla birlikte, bunu yapmak muhtemelen 
sürdürülemez bir karmaşaya yol açacaktır.Bunun yerine bu, kalıtımı 
kullanmak için mükemmel bir fırsattır.

Başlamak için, bir tablo oluşturmanın içerdiği adımlara odaklanın.
Tablonun üst kısmında bir dizi tablo başlığı bulunur.Bundan sonra, 
tablo verisi satırları görünür.Bu adımları atalım ve kendi sınıflarına 
yerleştirelim.`tableformat.py` adlı bir dosya oluşturun ve aşağıdaki 
sınıfı tanımlayın:

```python
# tableformat.py

class TableFormatter:
    def headings(self, headers):
        '''
        Emit the table headings.
        '''
	raise NotImplementedError()

    def row(self, rowdata):
        '''
        Emit a single row of table data.
        '''
	raise NotImplementedError()
```

Bu sınıf hiçbir şey yapmaz, ancak kısaca tanımlanacak ek sınıflar için
bir tür tasarım özelliği olarak hizmet eder.Bunun gibi bir sınıfa bazen 
"soyut temel sınıf" denir.

`print_report()` işlevini, bir `TableFormatter` nesnesini girdi olarak 
kabul edecek ve çıktıyı üretmek için üzerinde metotlar çağıracak şekilde 
değiştirin.  Örneğin, bunun gibi:

```python
# report.py
...

def print_report(reportdata, formatter):
    '''
    (name, shares, price, change) demet listesinden güzel biçimlendirilmiş bir tablo yazdırın.
    '''
    formatter.headings(['Name','Shares','Price','Change'])
    for name, shares, price, change in reportdata:
        rowdata = [ name, str(shares), f'{price:0.2f}', f'{change:0.2f}' ]
        formatter.row(rowdata)
```
print_report() fonksiyonuna bir bağımsız değişken eklediğiniz için,
`portfolio_report()` fonksiyonunu da değiştirmeniz gerekecektir.
Bunu, aşağıdaki gibi bir `TableFormatter` oluşturacak şekilde değiştirin:

```python
# report.py

import tableformat

...
def portfolio_report(portfoliofile, pricefile):
    '''
    Make a stock report given portfolio and price data files.
    '''
    # Veri dosyalarını oku
    portfolio = read_portfolio(portfoliofile)
    prices = read_prices(pricefile)

    # Rapor verilerini oluşturun
    report = make_report_data(portfolio, prices)

    # Yazdırın
    formatter = tableformat.TableFormatter()
    print_report(report, formatter)
```

Bu yeni kodu çalıştırın:

```python
>>> ================================ RESTART ================================
>>> import report
>>> report.portfolio_report('Data/portfolio.csv', 'Data/prices.csv')
... crashes ...
```
Hemen bir `NotImplementedError` istisnasıyla çökmelidir.  Bu çok heyecan verici değil,
ama tam da beklediğimiz gibi.  Bir sonraki bölüme devam edin.

### Alıştırma 4.6: Farklı Çıktı Üretmek İçin Kalıtımı Kullanma

Bölüm (a) 'da tanımladığınız `TableFormatter` sınıfı,
miras yoluyla genişletildi.  Aslında tüm fikir bu.  Örneklemek için şöyle bir 
`TextTableFormatter` sınıfı tanımlayın:

```python
# tableformat.py
...
class TextTableFormatter(TableFormatter):
    '''
    Emit a table in plain-text format
    '''
    def headings(self, headers):
        for h in headers:
            print(f'{h:>10s}', end=' ')
        print()
        print(('-'*10 + ' ')*len(headers))

    def row(self, rowdata):
        for d in rowdata:
            print(f'{d:>10s}', end=' ')
        print()
```

`portfolio_report()` 'fonksiyonunu şu şekilde değiştirin ve deneyin:

```python
# report.py
...
def portfolio_report(portfoliofile, pricefile):
    '''
    Portföy ve fiyat veri dosyaları verilen bir hisse senedi raporu hazırlayın.
    '''
    # Veri dosyalarını oku
    portfolio = read_portfolio(portfoliofile)
    prices = read_prices(pricefile)

    # Rapor verilerini oluşturun
    report = make_report_data(portfolio, prices)

    # Yazdırın
    formatter = tableformat.TextTableFormatter()
    print_report(report, formatter)
```

Bu, öncekiyle aynı çıktıyı üretmelidir:

```python
>>> ================================ RESTART ================================
>>> import report
>>> report.portfolio_report('Data/portfolio.csv', 'Data/prices.csv')
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
Bununla birlikte, çıktıyı başka bir şeye değiştirelim.  CSV biçiminde çıktı üreten
yeni bir `CSVTableFormatter` sınıfı tanımlayın:

```python
# tableformat.py
...
class CSVTableFormatter(TableFormatter):
    '''
    Output portfolio data in CSV format.
    '''
    def headings(self, headers):
        print(','.join(headers))

    def row(self, rowdata):
        print(','.join(rowdata))
```

Ana programınızı aşağıdaki şekilde değiştirin:

```python
def portfolio_report(portfoliofile, pricefile):
    '''
    Make a stock report given portfolio and price data files.
    '''
    # Veri dosyalarını oku
    portfolio = read_portfolio(portfoliofile)
    prices = read_prices(pricefile)

    # Rapor verilerini oluşturun
    report = make_report_data(portfolio, prices)

    # Yazdırın
    formatter = tableformat.CSVTableFormatter()
    print_report(report, formatter)
```

Şimdi CSV çıktısını şu şekilde görmelisiniz:

```python
>>> ================================ RESTART ================================
>>> import report
>>> report.portfolio_report('Data/portfolio.csv', 'Data/prices.csv')
Name,Shares,Price,Change
AA,100,9.22,-22.98
IBM,50,106.28,15.18
CAT,150,35.46,-47.98
MSFT,200,20.89,-30.34
GE,95,13.48,-26.89
MSFT,50,20.89,-44.21
IBM,100,106.28,35.84
```

Benzer bir fikir kullanarak bir `HTMLTableFormatter` sınıfı tanımlayın, bu
aşağıdaki çıktıya sahip bir tablo oluşturur:

```
<tr><th>Name</th><th>Shares</th><th>Price</th><th>Change</th></tr>
<tr><td>AA</td><td>100</td><td>9.22</td><td>-22.98</td></tr>
<tr><td>IBM</td><td>50</td><td>106.28</td><td>15.18</td></tr>
<tr><td>CAT</td><td>150</td><td>35.46</td><td>-47.98</td></tr>
<tr><td>MSFT</td><td>200</td><td>20.89</td><td>-30.34</td></tr>
<tr><td>GE</td><td>95</td><td>13.48</td><td>-26.89</td></tr>
<tr><td>MSFT</td><td>50</td><td>20.89</td><td>-44.21</td></tr>
<tr><td>IBM</td><td>100</td><td>106.28</td><td>35.84</td></tr>
```

Bir `CSVTableFormatter` nesnesi yerine bir `HTMLTableFormatter` nesnesi 
oluşturmak için ana programı değiştirerek kodunuzu test edin.

### Alıştırma 4.7: Çok Biçimlilik İş Başında

Nesne yönelimli programlamanın önemli bir özelliği, bir nesneyi
bir programa ekleyebilirsiniz ve mevcut kodlardan herhangi birini
değiştirmeye gerek kalmadan çalışacaktır.  Örneğin, bir `TableFormatter`
nesnesini kullanması beklenen bir program yazdıysanız, ona gerçekte 
ne tür bir `TableFormatter` verirseniz verin çalışacaktır.
Bu davranışa bazen "Çok Biçimlilik" adı verilir.

Olası sorunlardan biri, kullanıcının istediği biçimlendiriciyi seçmesine 
nasıl izin verileceğini bulmaktır.  `TextTableFormatter` gibi sınıf 
adlarının doğrudan kullanımı genellikle can sıkıcıdır.  Bu nedenle,
basitleştirilmiş bir yaklaşım düşünebilirsiniz.  Belki de koda şu şekilde
bir `if-`ifadesi yerleştirmişsinizdir:

```python
def portfolio_report(portfoliofile, pricefile, fmt='txt'):
    '''
    Portföy ve fiyat veri dosyaları verilen bir hisse senedi raporu hazırlayın.
    '''
    # veri dosyalarını oku
    portfolio = read_portfolio(portfoliofile)
    prices = read_prices(pricefile)

    # Rapor verilerini oluşturun
    report = make_report_data(portfolio, prices)

    # Yazdırın
    if fmt == 'txt':
        formatter = tableformat.TextTableFormatter()
    elif fmt == 'csv':
        formatter = tableformat.CSVTableFormatter()
    elif fmt == 'html':
        formatter = tableformat.HTMLTableFormatter()
    else:
        raise RuntimeError(f'Unknown format {fmt}')
    print_report(report, formatter)
```

Bu kodda, kullanıcı bir format seçmek için "'txt' 'veya"' csv '' gibi
basitleştirilmiş bir ad belirtir.  Ancak, `portfolio_report()` 
fonksiyonuna büyük bir `if`-ifadesi koymak en iyi fikir midir?
Bu kodu başka bir yerde genel amaçlı bir fonksiyona taşımak daha iyi 
olabilir.

`tableformat.py` dosyasında, `create_formatter(name)` işlevini ekleyin
bu, kullanıcının `'txt'`, `'csv'`, veya `'html'`gibi bir çıktı adı verilen
bir biçimlendirici oluşturmasına olanak tanır.`Portfolio_report () 'u
şöyle görünecek şekilde değiştirin:

```python
def portfolio_report(portfoliofile, pricefile, fmt='txt'):
    '''
    Portföy ve fiyat veri dosyaları verilen bir hisse senedi raporu hazırlayın.
    '''
    # veri dosyalarını oku
    portfolio = read_portfolio(portfoliofile)
    prices = read_prices(pricefile)

    # Rapor verilerini oluşturun
    report = make_report_data(portfolio, prices)

    # Yazdırın
    formatter = tableformat.create_formatter(fmt)
    print_report(report, formatter)
```

Çalıştığından emin olmak için fonksiyonu farklı biçimlerle çağırmayı deneyin.

### Egzersiz 4.8: Hepsini bir araya getirmek

`report.py` programını, `portfolio_report()` işlevinin çıktı biçimini belirten
isteğe bağlı bir bağımsız değişken alması için değiştirin.  Örneğin:

```python
>>> report.portfolio_report('Data/portfolio.csv', 'Data/prices.csv', 'txt')
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

Ana programı, komut satırında bir format verilebilecek şekilde değiştirin: 

```bash
bash $ python3 report.py Data/portfolio.csv Data/prices.csv csv
Name,Shares,Price,Change
AA,100,9.22,-22.98
IBM,50,106.28,15.18
CAT,150,35.46,-47.98
MSFT,200,20.89,-30.34
GE,95,13.48,-26.89
MSFT,50,20.89,-44.21
IBM,100,106.28,35.84
bash $
```

### Tartışma

Genişletilebilir kod yazmak, kütüphanelerde ve çatılarda kalıtımın 
en yaygın kullanımlarından biridir. Örneğin, bir çatı, sağlanan temel
sınıftan miras alan kendi nesnenizi tanımlamanızı isteyebilir.
Ardından, çeşitli işlevsellik bitlerini uygulayan çeşitli metotları 
doldurmanız istenir.

Biraz daha derin bir başka kavram da "soyutlamalarınıza sahip olmak."
Alıştırmalarda, bir tabloyu biçimlendirmek için * kendi sınıfımızı * 
tanımladık.  Kodunuza bakıp kendinize "Sadece bir format kütüphanesi 
veya onun yerine başka birinin yaptığı bir şey kullanmalıyım!" 
diyebilirsiniz.  Hayır, hem sınıfınızı hem de bir kütüphaneyi kullanmalısınız.
Kendi sınıfınızı kullanmak, gevşek bağlamayı teşvik eder ve daha esnektir.
Uygulamanız sınıfınızın programlama arayüzünü kullandığı sürece, 
dahili uygulamayı istediğiniz şekilde çalışacak şekilde değiştirebilirsiniz.
Tamamen özel kod yazabilirsiniz.  Birinin üçüncü taraf paketini kullanabilirsiniz.
Daha iyi bir paket bulduğunuzda, bir üçüncü taraf paketini farklı bir paketle 
değiştirirsiniz.  Farketmez, arayüzü koruduğunuz sürece uygulama kodunuzdan
hiçbiri kırılmaz.  Bu güçlü bir fikir ve böyle bir şey için kalıtımı düşünmenizin 
nedenlerinden biridir.

Bununla birlikte, nesneye yönelik programlar tasarlamak son derece zor olabilir.
Daha fazla bilgi için, muhtemelen tasarım kalıpları konusuyla ilgili kitaplara 
bakmalısınız(bu alıştırmada ne olduğunu anlamak, nesneleri pratik olarak yararlı
bir şekilde kullanma konusunda sizi oldukça ileriye götürür).

[Contents](../Contents.md) \| [Previous (4.1 Classes)](01_Class.md) \| [Next (4.3 Special methods)](03_Special_methods.md)