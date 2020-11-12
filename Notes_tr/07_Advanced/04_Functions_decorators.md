[Contents](../Contents.md) \| [Previous (7.3 Returning Functions)](03_Returning_functions.md) \| [Next (7.5 Decorated Methods)](05_Decorated_methods.md)

# 7.4 Fonksiyon Dekoratörleri

Bu bölüm dekoratör kavramına giriş yapmaktadır. Aslında ileri seviye bir konu olan dekoratör kavramı, burada yüzeysel bir şekilde anlatılmıştır.

### Loglama (Kayıt Tutma) Örneği

Aşağıdaki fonksiyonu inceleyiniz

```python
def topla(x, y):
    return x + y
```

Şimdi aynı fonksiyonun loglanmış halini inceleyin.

```python
def topla(x, y):
    print('Toplama yapılıyor')
    return x + y
```

Yine loglama mantığı ile ikinci bir fonksiyon.

```python
def cikar(x, y):
    print('Çıkartma yapılıyor')
    return x - y
```

### Gözlem

Kodların birçok kez kopyalanıp tekrarlandığı bir program yazmak biraz sinir bozucu olabilir. Yazması sıkıcı ve sürdürülebilirliği zor olurlar. Özellikle de kodun işleyişini değiştirmek istediğiniz zaman... (örneğin farklı bir loglama türü)

### Loglama Yapan Kod

Loglama eklenmiş fonksiyonlar üreten bir fonksiyon yaratabilirsiniz; bir wrapper(sarıcı, kapsayıcı)

```python
def logged(func):
    def wrapper(*args, **kwargs):
        print('Cagiriliyor ', func.__name__)
        return func(*args, **kwargs)
    return wrapper
```

Şimdi bunu deneyin.

```python
def topla(x, y):
    return x + y

logged_topla = logged(topla)
```

`logged` tarafından return edilen fonksiyonu çağırdığınız zaman ne olur?

```python
logged_topla(3, 4)      # Loglama mesajı görünür.
```

Bu örnek, bir wrapper fonksiyonun oluşturulma sürecini gösteriyor.

Wrapper yani sarıcı, başka fonksiyonları da sarabilen bir fonksiyondur, bunun dışında normal bir fonksiyonla aynı şekilde çalışır.

```python
>>> logged_topla(3, 4)
Cagiriliyor topla   # Wrapper tarafından eklenen ek çıktı
7
>>>
```

*Not: `logged()` fonksiyonu bir wrapper oluşturup bunu sonuç olarak döndürür.

## Dekoratörler

Fonksiyonlara wrapper eklemek Python’da oldukça yaygındır. O kadar yaygındır ki, kendine ait bir syntax’ı(yazım kuralı) bile vardır.

```python
def topla(x, y):
    return x + y
topla = logged(topla)

# Özel syntax
@logged
def topla(x, y):
    return x + y
```

Alt kısımdaki özel syntax bir üstündeki işlerin aynılarını gerçekleştirir. Bir dekoratör, yeni bir syntax... Dekoratörüm fonksiyonu *dekore* ettiği söylenebilir.

### Açıklama

Dekoretörlerin, burada açıklandığından çok daha fazla ve daha ince detayları vardır.
Class içinde kullanımları, veya bir fonksiyonda birden çok dekoratör kullanması bunlara örnek olarak verilebilir.
Yine de bir önceki örnekte kullanımları genel ve güzel bir şekilde özetleniyor.
Genellikle fonksiyon tanımlarında tekrar eden kodlardan kaçınmak için kullanılırlar. Bir dekoratör bu kodları genel bir tanımlama haline getirebilir.

## Alıştırmalar

### Alıştırma 7.10: Zamanlama için bir Dekoratör

Bir fonksiyon tanımlarsanız, bu fonksiyonun ismi ve modulü `__name__` ve `__module__` olarak kaydedilir. Örneğin:

```python
>>> def topla(x,y):
     return x+y

>>> topla.__name__
topla
>>> topla.__module__
'__main__'
>>>
```

`timethis.py` adlı bir dosyada, bir fonksiyonu sarıp o fonksiyonun ne kadar sürede çalıştığını gösteren bir mantık katmanı sağlayan `timethis(func)` isimli bir dekoratör fonksiyonu oluşturun. Bunu yapabilmek için, zamanlama çağrılarıyla fonksiyonu şu şekilde sarabilirsiniz:

```python
start = time.time()
r = func(*args,**kwargs)
end = time.time()
print('%s.%s: %f' % (func.__module__, func.__name__, end-start))
```

Dekoratörün nasıl çalışması gerektiğini gösteren bir örnek:

```python
>>> from timethis import timethis
>>> @timethis
def countdown(n):
        while n > 0:
             n -= 1

>>> countdown(10000000)
__main__.countdown : 0.076562
>>>
```

`@timethis` dekoratörü herhangi bir fonksiyon tanımlamasının önünde kullanılabilir. Bunun yanı sıra, performans iyileştirmesi yaparken tanı aracı olarak da kullanılabilir.

[Contents](../Contents.md) \| [Previous (7.3 Returning Functions)](03_Returning_functions.md) \| [Next (7.5 Decorated Methods)](05_Decorated_methods.md)

