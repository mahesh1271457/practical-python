[Contents](../Contents.md) \| [Previous (4.3 Special methods)](03_Special_methods.md) \| [Next (5 Object Model)](../05_Object_model/00_Overview.md)

# 4.4 İstisnaları Tanımlama

Kulllanıcı tanımlı istisnalar(exceptions) sınıflar tarafından tanımlanır.


```python
class NetworkError(Exception):
    pass
```

**İstisnalar her zaman `Exception`dan miras alır.**

Genellikle boş sınıflardır. Kodda `pass` kullanabilirsiniz.
İstisnalarınızın hiyerarşisini de oluşturabilirsiniz.

```python
class AuthenticationError(NetworkError):
     pass

class ProtocolError(NetworkError):
    pass
```

## Egzersizler
### Egzersiz 4.11: Özel istisna oluşturma

Kütüphanelerin kendi istisnalarını tanımlamaları genellikle iyi bir uygulamadır.

Bu, kütüphane tarafından spesifik kullanış problemlerini sinyal eden istisnalarla, Python'ın 
yaygın programlama hatalarından oluşturduğu istisnaları ayırt etmemizi kolaylaştırır.

Son egzersizdeki `create_formatter()` fonksiyonunu, kullanıcı yanlış format adı girdiğinde özel bir
`FormatError` istisna hatası vericek şekilde modifiye edin.

Örneğin:

```python
>>> from tableformat import create_formatter
>>> formatter = create_formatter('xls')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "tableformat.py", line 71, in create_formatter
    raise FormatError('Unknown table format %s' % name)
FormatError: Unknown table format xls
>>>
```

[Contents](../Contents.md) \| [Previous (4.3 Special methods)](03_Special_methods.md) \| [Next (5 Object Model)](../05_Object_model/00_Overview.md)
