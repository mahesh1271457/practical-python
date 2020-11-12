[Contents](../Contents.md) \| [Previous (3.2 More on Functions)](02_More_functions.md) \| [Next (3.4 Modules)](04_Modules.md)

# 3.3 Hata Denetimi
 Özel durumlar daha önce sunulmuş olsa da, bu bölüm 
hata denetimi ve istisnai durumlar ile ilgili bazı
ek ayrıntıları içerir.

## Bir Program Nasıl Başarısız Olur?
 Python,bağımsız değişken türlerini veya değerlerini
denetleme veya doğrulama gibi bir işlev yapmaz.
Bir işlev, işlevdeki ifadelerle uyumlu tüm veriler 
üzerinde çalışır.

```python
def add(x, y):
    return x + y

add(3, 4)               # 7
add('Hello', 'World')   # 'HelloWorld'
add('3', '4')           # '34'
```

Bir işlevde hatalar varsa, bunlar çalışma 
zamanında (özel durum olarak) görünür

```python
def add(x, y):
    return x + y

>>> add(3, '4')
Traceback (most recent call last):
...
TypeError: unsupported operand type(s) for +:
'int' and 'str'
>>>
```

Kodu doğrulamak için, deneme üzerinde güçlü bir
vurgu vardır.(daha sonra ele alınacak)

### 'Exceptions'
 Hataları bildirmek için özel durumlar kullanılır.
Kendiniz bir özel durum oluşturmak için, "raise"
ifadesini kullanın. 

```python
if name not in authorized:
    raise RuntimeError(f'{name} not authorized')
```

To catch an exception use `try-except`.

```python
try:
    authenticate(username)
except RuntimeError as e:
    print(e)
```

### Özel Durum Taşıma
 Özel durumlar,ilk eşleşen 'except' ile yayılır.

```python
def grok():
    ...
    raise RuntimeError('Whoa!')   # Exception raised here

def spam():
    grok()                        # Call that will raise exception

def bar():
    try:
       spam()
    except RuntimeError as e:     # Exception caught here
        ...

def foo():
    try:
         bar()
    except RuntimeError as e:     # Exception does NOT arrive here
        ...

foo()
```

 'Exception''ı işlemek için ifadeleri 'except' bloğuna koyun.İşlemek istediğiniz
hataya istediğiniz ifadeleri ekleyebilirsiniz.

```python
def grok(): ...
    raise RuntimeError('Whoa!')

def bar():
    try:
      grok()
    except RuntimeError as e:   # Exception caught here
        statements              # Use this statements
        statements
        ...

bar()
``````python
def grok(): ...
    raise RuntimeError('Whoa!')

def bar():
    try:
      grok()
    except RuntimeError as e:   # Exception caught here
        statements              # Use this statements
        statements
        ...

bar()
```

 İşlemden sonra,yürütme 'try except''ten sonra sonraki ilk
ifade ile devam eder.

```python
def grok(): ...
    raise RuntimeError('Whoa!')

def bar():
    try:
      grok()
    except RuntimeError as e:   # Exception caught here
        statements
        statements
        ...
    statements                  # Resumes execution here
    statements                  # And continues here
    ...

bar()
```

### Yerleşik Özel Durumlar
 Yaklaşık 2 düzine kadar yerleşik özel durumlar var.Genellikle
bu özel durumların adı neyin yanlış olduğunu gösterir.(örneğin, 
kötü bir değer sağladığınız için bir 'ValueError' yükseltilir.)
Aşağıdakiler kapsamlı bir liste değil. Daha fazlası için belgeleri kontrol
edin.

```python
ArithmeticError
AssertionError
EnvironmentError
EOFError
ImportError
IndexError
KeyboardInterrupt
KeyError
MemoryError
NameError
ReferenceError
RuntimeError
SyntaxError
SystemError
TypeError
ValueError
```

### Özel Durum Değerleri
 Özel durumların ilişkili bir değeri vardır. Neyin yanlış olduğu hakkında 
daha spesifik bilgiler içerir.


```python
raise RuntimeError('Invalid user name')

Bu değer, 'except' olarak verilen değişkene yerleştirilen özel durum 
örneğinin bir parçasıdır.

```python
try:
    ...
except RuntimeError as e:   # `e` holds the exception raised
    ...
``````python
try:
    ...
except RuntimeError as e:   # `e` holds the exception raised
    ...
```

'e' özel durum türünün bir örneğidir. Ancak, yazdırıldığında 
genellikle bir dize gibi görünür.

```python
except RuntimeError as e:
    print('Failed : Reason', e)
```

### Birden Çok Hatayı Yakalama
 Birden çok 'except' blokları kullanarak farklı türde özel durumlar 
yakalayabilirsiniz.

```python
try:
  ...
except LookupError as e:
  ...
except RuntimeError as e:
  ...
except IOError as e:
  ...
except KeyboardInterrupt as e:
  ...
```
Alternatif olarak, bunları işleyen ifadeler aynıysa, bunları gruplayabilirsiniz:

```python
try:
  ...
except (IOError,LookupError,RuntimeError) as e:
  ...
```

### Bütün Hataları Yakalama 
 Herhangi bir özel durumu yakalamak için 'Exception' ifadesini şu şekilde kullanın:

```python
try:
    ...
except Exception:       # DANGER. See below
    print('An error occurred')
```

 Genel olarak, böyle bir kod yazmak kötü bir fikirdir çünkü neden başarısız olduğu 
hakkında hiçbir fikriniz olmaz.

### Hataları Yakalamanın Yanlış Yolu
 İşte özel durumları kullanmanın yanlış yolları:


```python
try:
    go_do_something()
except Exception:
    print('Computer says no')
```

 Bu, olası tüm hataları yakalar ve kod hiç beklemediğiniz bir nedenle başarısız olduğunda
hata ayıklama imkansız hale getirebilir.
(e.g. uninstalled Python module, etc.).

### Biraz Daha İyi Bir Yaklaşım
 Eğer bütün hataları yakalayacaksanız,bu daha iyi bir yaklaşım:

```python
try:
    go_do_something()
except Exception as e:
    print('Computer says no. Reason :', e)
```

Kodun neden başarısız olduğu ile ilgili belirli bir neden bildirir. Olası tüm özel durumları 
yakalayan kod yazarken hataları görüntülemek/raporlamak için bazı mekanizmalara sahip olmak 
hemen hemen her zaman iyi bir fikirdir.

 Genel olarak,bir hatay nr kadar dar kapsamlı yakalanırsa o kadar makuldur.Sadece halledebileceğiniz
hataları yakalayın.Dier hataların geçmesine izin verin.(Belki başka kodlar onları halledebilir.)

### Bir Özel Durumu Yeniden Yükseltme
 Yakalanan hatayı yaymak için 'raise' ifadesini kullanın.

```python
try:
    go_do_something()
except Exception as e:
    print('Computer says no. Reason :', e)
    raise
```

Bu, işlem yapmanızı (örn. logging) ve hatayı arayana iletmenize olanak tanır.

### En İyi Özel Durum Uygulamaları
 Özel durumları yakalamayın.Hızlı ve yüksekte başarısız olun.Eğer önemliyse,
başka birisi probleme bakacaktır.Eğer *bu* birisi sizseniz, sadece özel durumu
yakalayın.Diğer bir şey, yalnızca kurtarabileceğiniz ve aklı başında devam 
edebileceğiniz hataları yakalayın.

### 'finally' İfadesi
 Bir özel durum oluşup oluşmadığına bakılmaksızın çalışması gereken kodu belirtir.

```python
lock = Lock()
...
lock.acquire()
try:
    ...
finally:
    lock.release()  # this will ALWAYS be executed. With and without exception.
```
 
 Kaynakları (özellikle kilitler, dosyalar, vb.) güvenli bir şekilde yönetmek için 
yaygın olarak kullanılır.

### 'with' İfadesi
 Modern kodda, 'try-finally' genellikle 'with' ifadesi ile yer değiştirilir.

```python
lock = Lock()
with lock:
    # lock acquired
    ...
# lock released
```

 Daha tanıdık örnekler:

```python
with open(filename) as f:
    # Use the file
    ...
# File closed
```

 `with`, bir kaynak için kullanım *bağlamını* tanımlar. Yürütme ne zaman
bu bağlamı bırakır, o zaman kaynaklar da serbest bırakılır.'ile' yalnızca onu 
desteklemek için özel olarak programlanmış belirli nesnelerle çalışır.

##Alıştırmalar

###Alıştırma 3.8:Özel Durumları Yükseltme
 Son bölümde yazdığınız 'parse_csv()' işlevi, kullanıcı tarafından belirtilen 
sütunların seçilmesine izin verir, ancak yalnızca giriş veri dosyasında sütun üstbilgileri varsa çalışır.


Hem 'select' hem de 'has_headers=False' bağımsız değişkenleri geçirilirse, kodu bir özel duruma yükseltecek
şekilde değiştirin.

```python
>>> parse_csv('Data/prices.csv', select=['name','price'], has_headers=False)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "fileparse.py", line 9, in parse_csv
    raise RuntimeError("select argument requires column headers")
RuntimeError: select argument requires column headers
>>>
```
 Bu tek denetimi ekledikte sonra,işlevde başka tür akıl sağlığı kontrolleri yapmanız gerekip gerekmediğini 
sorabilirsiniz.Örneğin, dosya adının bir dize olduğunu, türlerin bir liste olduğunu veya bu türden bir şey 
olduğunu kontrol etmeli misiniz?
 
 Genel bir kural olarak,genellikle bu tür testleri atlamak ve kötü girişlerde programın başarısız olduğunu 
iletmek en iyisidir.Traceback iletisi sorunun kaynağına işaret eder ve hata ayıklamaya yardımcı olabilir.

 Yukarıdaki denetimi eklemenin temel nedeni,kodu mantıklı olmayan bir modda çalıştırmaktan kaçınmaktır.
(örneğin,sütun başlıkları gerektiren ancak aynı anda hiçbir başlık olmadığını belirten bir özelliği kullanma)

 Bu,arama kodunun bir bölümünde bir proglamlama hatası gösterir."Olmaması gereken" vakaları kontrol etmek
genellikle iyi bir fikirdir.

  ### Alıştırma 3.9:Özel Durumları Yakalama
  Yazdığınız 'parse_csv()' işlevi, bir dosyanın tüm içeriğini işlemek için kullanılır.Ancak, gerçek dünyada, 
giriş dosyaları bozuk, eksik veya kirli veri olabilir.Bu denemeyi deneyin:

```python
>>> portfolio = parse_csv('Data/missing.csv', types=[str, int, float])
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "fileparse.py", line 36, in parse_csv
    row = [func(val) for func, val in zip(types, row)]
ValueError: invalid literal for int() with base 10: ''
>>>
```

 Kayıt oluşturma sırasında oluşturulan tüm "ValueError" istisnalarını yakalamak ve dönüştürülemeyen satırlar 
için bir uyarı mesajı yazdırmak için "parse_csv ()" işlevini değiştirin.

 Mesaj satır numarası ve neden başarısız olduğu hakkında bilgi içermelidir.İşlevinizi test etmek için 
yukarıdaki 'Data/missing.csv' dosyasını okumayı deneyin.Örneğin:


```python
>>> portfolio = parse_csv('Data/missing.csv', types=[str, int, float])
Row 4: Couldn't convert ['MSFT', '', '51.23']
Row 4: Reason invalid literal for int() with base 10: ''
Row 7: Couldn't convert ['IBM', '', '70.44']
Row 7: Reason invalid literal for int() with base 10: ''
>>>
>>> portfolio
[{'price': 32.2, 'name': 'AA', 'shares': 100}, {'price': 91.1, 'name': 'IBM', 'shares': 50}, {'price': 83.44, 'name': 'CAT', 'shares': 150}, {'price': 40.37, 'name': 'GE', 'shares': 95}, {'price': 65.1, 'name': 'MSFT', 'shares': 50}]
>>>
```

### Alıştırma 3.10:Hataları Susturma
 Kullanıcı tarafından açıkça istenirse ayrıştırma hata iletilerinin susturulabilmesi için 'parse_csv()' 
işlevini değiştirin.Örneğin:

```python
>>> portfolio = parse_csv('Data/missing.csv', types=[str,int,float], silence_errors=True)
>>> portfolio
[{'price': 32.2, 'name': 'AA', 'shares': 100}, {'price': 91.1, 'name': 'IBM', 'shares': 50}, {'price': 83.44, 'name': 'CAT', 'shares': 150}, {'price': 40.37, 'name': 'GE', 'shares': 95}, {'price': 65.1, 'name': 'MSFT', 'shares': 50}]
>>>
```

 Hata işleme, çoğu programda doğru yapılması en zor şeylerden biridir.Genel bir kural olarak,hataları sessizce görmezden gelmemelisiniz.
Bunun yerine, sorunları bildirmek ve kullanıcıya hata iletisini susturma seçeneği vermek daha iyidir.

[Contents](../Contents.md) \| [Previous (3.2 More on Functions)](02_More_functions.md) \| [Next (3.4 Modules)](04_Modules.md)