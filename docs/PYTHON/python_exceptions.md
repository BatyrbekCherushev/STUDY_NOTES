# ВИКЛЮЧЕННЯ і ПОМИЛКИ (ERRORS and EXCEPTIONS)

## Тематичні посилання:

* [aCode - Винятки в Python](https://acode.com.ua/exceptions-python/)
* [Exceptions](https://book.pythontips.com/en/latest/exceptions.html)
* [RealPython - Python Exceptions: An Introduction](https://realpython.com/python-exceptions/)
* [programiz - Python Exception Handling](https://www.programiz.com/python-programming/exception-handling)

---

## try... except... вираз

```
try:
    pass 
except:
    pass
else:
    pass
finally:
    pass
```

**Порядок виконання:**
* спочатку виконується блок try
* якщо помилки не виникає
    * виконується блок else
* якщо помилка виникає
    * виконується блок except
        * може бути загальним, без вказування конкретного типу помилки
        * можна створювати під конкретний тип помилки свій except блок
        * можна створити блок виключення для групи помилок перерахувавши їх в кортежі
        * можна в виключення передати клас Exception() для отримання інформації про помилку
* виконується блок finally  в незалежності від того чи була помилка чи ні

 **Особливості роботи виразу виключення**
* якщо блок try має в собі декілька помилкових виразів, то виключення спрацьовує на першу зустрінуту помилку
* assert - використання при дебагінгу

**Вираз виключення без вказання конкретної помилки**
```python
try:
    1 / 0
except:
    print('Error occured')
```

**Виключення для конкретного типу помилки**
```python
try:
    '1' + 1
except TypeError:
    print('---> TypeError occured')

try:
    1 / 0
except ZeroDivisionError:
    print('---> ZeroDivisionError occured')

try:
    [1, 2, 3][5]
    '1' + 1
except IndexError:
    print('---> IndexError occured')
```

**Поєднання різних виключень в одному виразі**
```python
try:
    # '1' + 1
    # [1, 2, 3][5]
    1 / 0
except (TypeError, ZeroDivisionError, IndexError):
    print('---> IndexError or TypeError or ZeroDivisionError occured')
```

**Вираз виключення з використанням класу Exception для отримання інформації про помилку і використанням аліаса**

інстанс цього класа включає в себе різні атрибути, що містять інформацію про помилку
```python
try:
    1 / 0
except Exception as error_instance:
    print(error_instance)
    print(type(error_instance))
    print(error_instance.args)
    print(dir(error_instance))
```

```python
try:
    '1' + 1
except Exception as error_instance:
    print(error_instance)
    print(type(error_instance))
    print(error_instance.args)
```

**Виключення спрацьовує на першу зустрінуту помилку**
```python
try:
    [1, 2, 3][5]
    '1' + 1
except Exception as error_instance:
    print(error_instance)
    print(type(error_instance))
    print(error_instance.args)
```

**Блок else виконується в разі якщо не виникло помилки в блоку try**

```python
# коли НЕ має помилки, спрацьовує блок else
try:
    new_var = 1
except Exception as error_instance:
    print(error_instance)
    print(type(error_instance))
    print(error_instance.args)
else:
    print('---> error DID NOT occur')

print('\n---------------------\n')

# коли є помилка, блок else не спрацьовує
try:
    1 / 0
except Exception as error_instance:
    print(error_instance)
    print(type(error_instance))
    print(error_instance.args)
else:
    print('---> error DID NOT occur')
```

**Блок finally спрацьовує в незалежності від того чи виникла помилка, чи ні**
```python
# ERROR doesn`t occur
try:
    print('TRY block')
    new_var = 1
except:
    print('EXCEPTION block')
else:
    print('ELSE block')
finally:
    print('FINALLY block')

print('\n---------------------\n')

# ERROR occurs
try:
    print('TRY block')
    1 / 0
except:
    print('EXCEPTION block')
else:
    print('ELSE block')
finally:
    print('FINALLY block')
```
---
## RAISE

* Операція raise викликає помилку за потреби. Можна передати об'єкт класу Exception з певним аргументами, що присвояться до атрибуту args інстансу цього об'єкта
    * при такому виклику виключення програма зупиняється
* Використання її у виключенні у блоку except робить виклик тієї ж помилки по якій було виключення, але вже без спрацюювання виключення, а спрацювання реакції інтерпретатора на помилку

```python
number = 10
if number > 5:
    raise Exception(f"The number should not exceed 5. ({number=})")
print(number)
```

```python
# ERROR occurs
try:
    print('TRY block')
    1 / 0
except:
    print('EXCEPTION block')
    raise
else:
    print('ELSE block')
finally:
    print('FINALLY block')
```
---

## Custom exception

**Виклик кастомного виключення, що наслідує клас Exception**

```python
class CustomException(Exception):
    """My custom exception"""
    pass

try:
    raise  CustomException(1,2,3)
except CustomException:
    print('--->>> CustomException occured')
```
 

