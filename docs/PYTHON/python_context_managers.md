
---
## <h2 style = 'text-align:center'><b>📌 Посилання на тему</b></h2>
---
* [Python documentation - Context Manager Types](https://docs.python.org/3.7/library/stdtypes.html#typecontextmanager)
* [Python documentation - 3.3.9. With Statement Context Managers](https://docs.python.org/3.7/reference/datamodel.html#context-managers)
* [Python documentation - The with statement](https://docs.python.org/3.7/reference/compound_stmts.html#with)
* [Python documentation - contextlib — Utilities for with-statement contexts](https://docs.python.org/3.7/library/contextlib.html)
---
## <h2 style = 'text-align:center'><b>📌 Визначення</b></h2>
---
Контекстні менеджери в Python — це дуже корисна концепція, яка дозволяє автоматично керувати ресурсами, такими як

* Файли: open()
* Блокування потоків: - ` with lock`
* Бази даних: відкриття/закриття транзакцій - `with connection`
* Тимчасова зміна налаштувань (наприклад, зміна робочої директорії)
* Мережеві з’єднання
* Автоматичне очищення ресурсів

**Контекстний менеджер** — це об’єкт, який реалізує методи `__enter__()` та `__exit__()`. Цей об’єкт визначає що треба зробити:

* при вході в контекст (наприклад, відкрити файл, створити з’єднання, взяти блокування);

* при виході з контексту (закрити файл, відпустити з’єднання, зняти блокування), навіть якщо сталася помилка.

Щоб бути контекстним менеджером, об’єкт повинен мати два методи:

* `__enter__(self)` — викликається при вході в контекст (після with);

* `__exit__(self, exc_type, exc_value, traceback)` — викликається при виході (навіть у разі помилки).
    * `exc_type, exc_value, traceback` - параметри, через які передаються дані про виключення, в разі його виникнення
    * якщо виключення не сталось, передаються None

**Порядок роботи з `with` виразом**
``` python
with A() as a, B() as b:
    suite
```
- `A()` - виклик контекстного менеджера
- `as a` - результат вмконання присвоюється в `a`
- `A() as a, B() as b` - в одному рядку можна викликати декілька контекстних менеджерів

Альтернативний варіант для роботи з декількома контекстними менеджерами:
```python
with A() as a:
    with B() as b:
        suite
```

**Послідовність виконання методу `with` для одного контекстного менеджера:**

1. Отримання об'єкта контекстного менеджера.
2. Завантаження магічного метода `__exit__` цього контекстного менеджера, для майбутнього автоматичного використання при виході з виразу `with`.
3. Виконується магічний метод `__enter__` (об'єкта контекстного менеджера).
4. Якщо він повертає якесь значення, то воно присвоюється у вираз `as`.
>якщо метод `__enter__` виконався без помилки, то метод `__exit__`, прив'язаний до контекстного менеджера виконається при будь яких обставинах. Навіть якщо в середині блоку коду, що вкладено після `with`, виникне помилка.
5. Виконання виконання вкладеного блоку коду `suite`.
6. Виконання методу `__exit__`
>Якщо блок коду `suite` видає помилку, то її тип, значення і повний трейсбек передаються у метод `__exit__` 

 Зазвичай менеджери контексту викликаються за допомогою інструкції with, , яка гарантує, що ресурси будуть закриті або звільнені, навіть якщо виникла помилка. Але їх також можна використовувати шляхом прямого виклику їхніх методів (`__enter__`, `__exit__`).

**Використання контекстного менеджера через `with`:**
```python
with open("data.txt", "r") as f:
    content = f.read()
# Тут файл автоматично закрито, навіть якщо всередині було виключення
```

**Приклад використання контекстного менеджера без `with`:**
```python
cm = open("data.txt", "w")  # створили контекстний менеджер
try:
    cm.__enter__()  # вхід у контекст
    cm.write("Привіт без with!\n")
except Exception as e:
    # Передаємо інформацію про помилку в __exit__
    import sys
    exc_info = sys.exc_info()  # (type, value, traceback)
    cm.__exit__(*exc_info)
else:
    cm.__exit__(None, None, None)  # вихід із контексту
```
---
## <h2 style = 'text-align:center'><b>📌 Створення власного контекстного менеджера</b></h2>
---
Реалізувати власний контекстний менеджер можна:

* Через класи
* Використанням декоратор `@contextmanager` із вбудованого модуля `contextlib`

**Класовий спосіб**
```python
class MyCM:
    def __enter__(self):
        print("Вхід у контекст")
        return "Дані"
    
    def __exit__(self, exc_type, exc_value, tb):
        print("Вихід із контексту")
        print("Тип помилки:  ", exc_type)
        print("Об'єкт помилки:", exc_value)
        print("Traceback:    ", tb)
        # Якщо повернути True — помилка буде приглушена
        return True

with MyCM() as data:
    print('Working...')
    print('Data = ', data)
```
🧾 Вивід
```mkdocs
Вхід у контекст
Working...
Data =  Дані
Вихід із контексту
Тип помилки:   None
Об'єкт помилки: None
Traceback:     None
```
**Декоратор `@contextmanager` із `contextlib`**
```python
from contextlib import contextmanager

@contextmanager
def my_context():
    print("Входимо")
    yield "Ресурс"
    print("Виходимо")

with my_context() as res:
    print("Всередині:", res)

```
- 🔧 yield розділяє вхід і вихід з контексту
- блок коду до yield є аналогічним реалізації методу `__enter__`
- блок коду після виразу yield аналогічний реалізацї методу `__exit__`


---
## <h2 style = 'text-align:center'><b>📌 Приклади</b></h2>
---
### **⏱ Контекстний менеджер для таймера**

Цей менеджер вимірює, скільки часу займає виконання блоку коду.
```python
import time

class Timer:
    def __enter__(self):
        self.start = time.time()
        return self  # можна повертати об'єкт, щоб використовувати його всередині

    def __exit__(self, exc_type, exc_value, traceback):
        self.end = time.time()
        self.interval = self.end - self.start
        print(f"Час виконання: {self.interval:.4f} секунд")

# Приклад використання
with Timer():
    total = sum(range(1000000))
```
- `__enter__` зберігає час початку
- `__exit__` обчислює час завершення і виводить результат

### **📋 Контекстний менеджер для логування**

Цей менеджер записує повідомлення до лог-файлу при вході та виході з блоку.
```python
from contextlib import contextmanager

@contextmanager
def log_action(action_name):
    with open("log.txt", "a") as log_file:
        log_file.write(f"[START] {action_name}\n")
        try:
            yield
        finally:
            log_file.write(f"[END] {action_name}\n")

# Приклад використання
with log_action("Обробка даних"):
    print("Тут відбувається обробка...")
```
📁 У файл `log.txt` буде записано:
```
[START] Обробка даних
[END] Обробка даних
```

### **📂 Контекстний менеджер: тимчасова зміна директорі**

```python
import os

class SafeChangeDirectory:
    def __init__(self, new_path):
        self.new_path = new_path
        self.original_path = os.getcwd()

    def __enter__(self):
        if not os.path.exists(self.new_path):
            print(f"Директорія '{self.new_path}' не існує. Створюю...")
            os.makedirs(self.new_path)
        else:
            print(f"Директорія '{self.new_path}' вже існує.")
        os.chdir(self.new_path)
        print(f"Перехід до: {self.new_path}")

    def __exit__(self, exc_type, exc_value, traceback):
        os.chdir(self.original_path)
        print(f"Повернення до: {self.original_path}")
```
✅ Приклад використання:
```
print("Поточна директорія:", os.getcwd())

with SafeChangeDirectory("test_folder"):
    print("Всередині блоку:", os.getcwd())

print("Після блоку:", os.getcwd())
```


