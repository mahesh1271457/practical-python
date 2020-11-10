[İçindekiler](../Contents.md) \| [Geri (6.4 Oluşturma İfadeleri)](../06_Generators/04_More_generators.md) \| [İleri (7.2 Anonim Fonksiyonlar)](02_Anonymous_function.md)

# 7.1 Değişken Argümanları

Bu bölüm `*args` ve `**kwargs` olarak da ifade edilen, değişken sayıda argüman alan fonksiyonlar konusunu ele alacaktır.

### Konumsal Değişken Argümanları (*args)

Değişken sayılarda argüman kabul eden bir fonksiyon değişken argümanlarını kullanır.

Örneğin:

```python
def f(x, *args):
    ...
```

Fonksiyon çağırılır.

```python
f(1,2,3,4,5)
```

Ekstra argümanlar tuple olarak gönderilir.

```python
def f(x, *args):
    # x -> 1
    # args -> (2,3,4,5)
```

### Klavye Değişken Argümanları (**kwargs)

Bir fonksiyon aynı zamanda değişken sayılardaki klavye argümanlarını da kabul edebilir.

Örnek:

```python
def f(x, y, **kwargs):
    ...
```

Fonksiyon çağırılır.

```python
f(2, 3, flag=True, mode='fast', header='debug')
```

Ekstra argümanlar fonksiyona sözlük veri tipinde gönderilir.

```python
def f(x, y, **kwargs):
    # x -> 2
    # y -> 3
    # kwargs -> { 'flag': True, 'mode': 'fast', 'header': 'debug' }
```

### İkisini beraber kullanmak

Bir fonksiyon aynı zamanda hem klavyesiz argümanları hem de klavyeden girilen değişken sayılardaki argümanları kabul edebilir.

```python
def f(*args, **kwargs):
    ...
```

Fonksiyon çağırılır.

```python
f(2, 3, flag=True, mode='fast', header='debug')
```

Argümanlar konumsal ve anahtar kelime parçalarına ayrılır.

```python
def f(*args, **kwargs):
    # args = (2, 3)
    # kwargs -> { 'flag': True, 'mode': 'fast', 'header': 'debug' }
    ...
```

Bu fonksiyon, konumsal ya da anahtar kelime argümanlarının herhangi bir kombinasyonunu alır. Bazen diğer fonksiyonlara argüman gönderirken kullanılır.

### Tuple ve Sözlük veri tiplerini göndermek

Tuple'lar değişken argümanlarına dönüşebilir.

```python
numbers = (2,3,4)
f(1, *numbers)      # Same as f(1,2,3,4)
```

Sözlükler de aynı zamanda anahtar kelime argümanlarına dönüşebilirler.

```python
options = {
    'color' : 'red',
    'delimiter' : ',',
    'width' : 400
}
f(data, **options)
# Same as f(data, color='red', delimiter=',', width=400)
```

## Alıştırmalar

### Alıştırma 7.1: Değişken argümanları hakkında basit bir alıştırma

Aşağıdaki fonksiyonu tanımlamaya çalışın.

```python
>>> def avg(x,*more):
        return float(x+sum(more))/(1+len(more))

>>> avg(10,11)
10.5
>>> avg(3,4,5)
4.0
>>> avg(1,2,3,4,5,6)
3.5
>>>
```

`*more` parametresinin tüm ekstra argümanları nasıl topladığına dikkat edin.

### Alıştırma 7.2: Tuple ve Sözlük veri tipini argüman olarak göndermek

Bir dosyadan veri okuduğunuzu ve aşağıdaki tuple'ı elde ettiğinizi varsayalım.

```
>>> data = ('GOOG', 100, 490.1)
>>>
```

Şimdi de bu veriden `Stock` adında bir obje yaratmak istediğinizi varsayalım. Eğer  `data` değişkenini doğrudan göndermeye çalışırsanız bu işe yaramayacaktır.

```
>>> from stock import Stock
>>> s = Stock(data)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: __init__() takes exactly 4 arguments (2 given)
>>>
```

`*data` kullanarak bu hatayı kısaca çözebilirsiniz. Örnekteki gibi deneyin:

```python
>>> s = Stock(*data)
>>> s
Stock('GOOG', 100, 490.1)
>>>
```

Eğer sözlük veri tipiniz varsa `**` kullanmalısınız. Örnek olarak: 

```python
>>> data = { 'name': 'GOOG', 'shares': 100, 'price': 490.1 }
>>> s = Stock(**data)
Stock('GOOG', 100, 490.1)
>>>
```

### Alıştırma 7.3: Durumlar listesi yaratmak

`report.py` programında, aşağıdaki kodu kullanarak bir durumlar listesi yarattınız.

```python
def read_portfolio(filename):
    '''
    Read a stock portfolio file into a list of dictionaries with keys
    name, shares, and price.
    '''
    with open(filename) as lines:
        portdicts = fileparse.parse_csv(lines,
                               select=['name','shares','price'],
                               types=[str,int,float])

    portfolio = [ Stock(d['name'], d['shares'], d['price'])
                  for d in portdicts ]
    return Portfolio(portfolio)
```

`Stock(**d)` kullanarak bu kodu daha da sadeleştirebilirsiniz. Kendiniz deneyin.

### Alıştırma 7.4: Argüman göndermek

`fileparse.parse_csv()` fonksiyonu hata raporlaması ve dosya sınırlayıcısını değiştirmek için bazı seçenekler barındırıyor. Bu seçenekleri aşağıdaki `read_portfolio()` fonksiyonunda keşfetmek isteyebilirsiniz. Alttaki değişiklikleri uygulayın.

```
def read_portfolio(filename, **opts):
    '''
    Read a stock portfolio file into a list of dictionaries with keys
    name, shares, and price.
    '''
    with open(filename) as lines:
        portdicts = fileparse.parse_csv(lines,
                                        select=['name','shares','price'],
                                        types=[str,int,float],
                                        **opts)

    portfolio = [ Stock(**d) for d in portdicts ]
    return Portfolio(portfolio)
```

Değişikliği yaptıktan sonra, bazı hatalar içeren bir dosyayı okumayı deneyin:

```python
>>> import report
>>> port = report.read_portfolio('Data/missing.csv')
Row 4: Couldn't convert ['MSFT', '', '51.23']
Row 4: Reason invalid literal for int() with base 10: ''
Row 7: Couldn't convert ['IBM', '', '70.44']
Row 7: Reason invalid literal for int() with base 10: ''
>>>
```

Şimdi hataları görmezden gelmeyi deneyelim:

```python
>>> import report
>>> port = report.read_portfolio('Data/missing.csv', silence_errors=True)
>>>
```

[İçindekiler](../Contents.md) \| [Geri (6.4 Oluşturma İfadeleri )](../06_Generators/04_More_generators.md) \| [İleri (7.2 Anonim Fonksiyonlar)](02_Anonymous_function.md)