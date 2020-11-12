[Contents](../Contents.md) \| [Previous (7.1 Variable Arguments)](01_Variable_arguments.md) \| [Next (7.3 Returning Functions)](03_Returning_functions.md)

# 7.2 Anonim Fonksiyonlar ve Lambda

### Yeniden Liste Sıralama Ziyareti

Listeler `sort` metodu kullanılarak sıralanabilirler.

```python
s = [10,1,7,3]
s.sort() # s = [1,3,7,10]
```

Aynı şekilde tersten bir şekilde de sıralanabilirler.

```python
s = [10,1,7,3]
s.sort(reverse=True) # s = [10,7,3,1]
```

Listeleri sıralamak gerçekten basit gözüküyor, fakat sözlük veri tiplerini nasıl sıralayabiliriz?

```python
[{'name': 'AA', 'price': 32.2, 'shares': 100},
{'name': 'IBM', 'price': 91.1, 'shares': 50},
{'name': 'CAT', 'price': 83.44, 'shares': 150},
{'name': 'MSFT', 'price': 51.23, 'shares': 200},
{'name': 'GE', 'price': 40.37, 'shares': 95},
{'name': 'MSFT', 'price': 65.1, 'shares': 50},
{'name': 'IBM', 'price': 70.44, 'shares': 100}]
```

Hangi kriterlere göre sıralayabiliriz?

*Anahtar fonksiyon* kullanarak sıralamaya rehberlik edebilirsiniz. *Anahtar fonksiyon* sözlük değişkenini alan ve içindeki değere göre sıralamayı döndüren fonksiyondur.

```python
def stock_name(s):
    return s['name']

portfolio.sort(key=stock_name)
```

Yukarıdaki kodun sonucunu burada görebilirsiniz.

```python
# Check how the dictionaries are sorted by the `name` key
[
  {'name': 'AA', 'price': 32.2, 'shares': 100},
  {'name': 'CAT', 'price': 83.44, 'shares': 150},
  {'name': 'GE', 'price': 40.37, 'shares': 95},
  {'name': 'IBM', 'price': 91.1, 'shares': 50},
  {'name': 'IBM', 'price': 70.44, 'shares': 100},
  {'name': 'MSFT', 'price': 51.23, 'shares': 200},
  {'name': 'MSFT', 'price': 65.1, 'shares': 50}
]
```

### Geri Çağırılan Fonksiyonlar

Aşağıdaki örnekte anahtar fonksiyon geri çağırılan fonksiyonlara bir örnektir. `sort()` metodu gönderdiğiniz fonksiyonu geri çağırır. Geri çağırılan fonksiyonlar genelde sadece bir operasyon için kullanılan ve bir satırdan oluşan fonksiyonlardır. Programcılar genellikle bu ekstra işlemi belirtmek için kısa yollar ararlar.

### Lambda: Anonim Fonksiyon

Yeni bir fonksiyon yaratmak yerine, lambda kullanabilirsiniz. Bir önceki örneğimizi görecek olursak;

```python
portfolio.sort(key=lambda s: s['name'])
```

Bu kod tek bir ifadeyi hesaplayan *isimsiz* bir fonksiyon yaratır. Yukarıda yazdığımız kod ilk kodumuza göre daha kısadır.

```python
def stock_name(s):
    return s['name']

portfolio.sort(key=stock_name)

# vs lambda
portfolio.sort(key=lambda s: s['name'])
```

### lambda kullanmak

* lambda oldukça kısıtlıdır.
* Sadece tek bir ifadeye izin vardır.
* `if`, `while` vb. gibi ifadeler yoktur.
* Yaygın olarak `sort()` gibi fonksiyonlarla beraber kullanılır.

## Alıştırmalar

Verileri oku ve onları listeye çevir.

```python
>>> import report
>>> portfolio = list(report.read_portfolio('Data/portfolio.csv'))
>>> for s in portfolio:
        print(s)

Stock('AA', 100, 32.2)
Stock('IBM', 50, 91.1)
Stock('CAT', 150, 83.44)
Stock('MSFT', 200, 51.23)
Stock('GE', 95, 40.37)
Stock('MSFT', 50, 65.1)
Stock('IBM', 100, 70.44)
>>>
```

### Exercise 7.5: Alana göre sıralamak

Stok isimlerine göre alfabetik olarak sıralama yapan aşağıdaki ifadeleri dene.

```python
>>> def stock_name(s):
       return s.name

>>> portfolio.sort(key=stock_name)
>>> for s in portfolio:
           print(s)

... inspect the result ...
>>>
```

Bu bölümde , `stock_name()` fonksiyonu portfolyo listesinin içinden gelen tek bir giriş ile stok ismini döndürüyor. Karşılaştırma yapabilmek için ise `sort()` fonksiyonu bu fonksiyondan dönen değeri kullanıyor.

### Exercise 7.6: Lambda kullanarak sıralamak

`lambda` ifadesini kullanarak 'shares' numaralarına göre portfolyoları sıralamayı dene.:

```python
>>> portfolio.sort(key=lambda s: s.shares)
>>> for s in portfolio:
        print(s)

... inspect the result ...
>>>
```

Her bir stoğun fiyatına göre sıralamayı dene:

```python
>>> portfolio.sort(key=lambda s: s.price)
>>> for s in portfolio:
        print(s)

... inspect the result ...
>>>
```

Not: `lambda`, ayrı ayrı fonksiyonlar yaratmanın aksine ; `sort()` fonksiyonunun içinde doğrudan işlem yapma kolaylığını size sağladığı için çok kullanışlı bir kısayoldur.

[Contents](../Contents.md) \| [Previous (7.1 Variable Arguments)](01_Variable_arguments.md) \| [Next (7.3 Returning Functions)](03_Returning_functions.md)