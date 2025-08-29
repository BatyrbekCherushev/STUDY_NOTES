# <h1 style = 'text-align:center'><b>СТРУКТУРИ ДАНИХ </b></h1>
## <h2 style = 'text-align:center'><b>📖ВСТУП </b></h2>
## <h2 style = 'text-align:center'><b>📖Стек (Stack) </b></h2>
### 📌 Визначення
Стек — це абстрактна структура даних, яка працює за принципом "останній прийшов — перший пішов" (LIFO). Уяви собі стопку тарілок: ти можеш взяти лише верхню, і нову кладеш теж зверху. Саме так поводиться стек у програмуванні.

🧠 **Основні властивості стеку** 

- LIFO — останній доданий елемент буде першим, який видалиться.
- Операції тільки з верхівкою — не можна звертатися до середини стеку напряму.

🔧 **Основні операці**

* push (додавання елементу)
    * Додає елемент на верхівку стеку.
* pop (видалення елементу)
    * Видаляє елемент з верхівки стеку і повертає його.
* peek / top (перегляд верхнього елементу)
    * Повертає верхній елемент стеку, не видаляючи його.
* is_empty (перевірка на порожнечу)
    * Перевіряє, чи є елементи в стеку.
* size (розмір стеку)
    * Повертає кількість елементів у стеку.

🧩 **Застосування стеку**
- Рекурсія — кожен виклик функції зберігається в стеку викликів.
- Обробка виразів — калькулятори, парсери, компілятори.
- Повернення назад — браузери, редактори (історія дій).
- Алгоритми обходу графів — DFS (глибина першого пошуку).

🧠 **Реалізація стеку**

* Реалізація через список (list)
* Реалізація через зв’язаний список (linked list)
* Реалізація через collections.deque

### 📌 Приклади

**Реалізація через список (list)**
```python
class Stack:
    def __init__(self):
        self.items = []

    def push(self, item):
        self.items.append(item)

    def pop(self):
        if not self.is_empty():
            return self.items.pop()
        return None

    def peek(self):
        if not self.is_empty():
            return self.items[-1]
        return None

    def is_empty(self):
        return len(self.items) == 0

    def size(self):
        return len(self.items)

# Приклад використання
stack = Stack()
stack.push(10)
stack.push(20)
print(stack.pop())   # 20
print(stack.peek())  # 10
```


## <h2 style = 'text-align:center'><b>📖Черга (Queue)</b></h2>
### 📌 Визначення

Черга — це одна з базових структур даних, яка працює за принципом "перший прийшов — перший пішов" (FIFO). Уяви собі чергу в магазині: той, хто став першим, обслуговується першим. Саме так поводиться і програмна черга.

>«Перший прийшов — перший вийшов».

**Основні характеристики черги**

- Голова (head) — елемент, який буде видалено наступним.
- Хвіст (tail) — місце, куди додається новий елемент.
- FIFO — порядок обробки: перший доданий — перший оброблений.


**Основні операції черги**

| Операція             | Опис                                          |
| -------------------- | --------------------------------------------- |
| **enqueue(item)**    | Додати елемент у кінець черги                 |
| **dequeue()**        | Взяти елемент з початку черги і видалити його |
| **peek() / front()** | Подивитися перший елемент, не видаляючи       |
| **is\_empty()**      | Перевірити, чи черга порожня                  |

**Типи черг**

* одностороння
    * видалення елементів відбувається з кінця
    * додавання нових елементів з початку
* двостороння
    * видалення і додавання можуть відбуватися з обох боків


## <h2 style = 'text-align:center'><b>📖Зв'язаний список (linked list) </b></h2>
**ЛІНКИ**

* [stackabuse - Python Linked Lists](https://stackabuse.com/python-linked-lists/)
* [geeksforgeeks - Linked List Data Structure](https://www.geeksforgeeks.org/dsa/linked-list-data-structure/)
* [tutorialspoint - Python - Linked Lists](https://www.tutorialspoint.com/python_data_structure/python_linked_lists.htm)

**Зв’язаний список** — це структура даних, яка складається з елементів (вузлів). Кожен вузол має:

* дані (наприклад число, рядок, об’єкт);
* посилання (вказівник) на наступний елемент списку.

**Особливості:**

* Данні розташовані не обов’язково у суміжних сегментах пам’яті.
* Кінець списку позначається значенням None (аналог NIL)

**Типи зв’язаних списків:**

* Однозв’язаний список (Singly Linked List):
    * кожен вузол знає тільки наступний.
    * вузол має лише next-посилання
* Двозв’язаний список (Doubly Linked List):
    * вузол має посилання на наступний і попередній.
    * кожен вузол містить і prev, і next;
    * дозволяє двобічну навігацію, але потребує більше пам’яті
* Кільцевий список (Circular Linked List):
    * останній вузол посилається на перший, що створює цикл;
    * корисний для циклічних структур (наприклад, у грі чи повторюваних чергах)


**Порівняння із масивами**

**Переваги:**

* Динамічне виділення пам’яті — не потрібно резервувати місце попередньо
* Легкі вставка та видалення всередині структури — оновлюються лише посилання, без пересувань пам’яті

**Недоліки:**

* Потрібна додаткова пам’ять для зберігання посилань.
* Повільний прямий доступ — щоб дістатися до k-го елементу, доводиться проходити весь список (O(n))

### 📌 **Приклади**

**Однозв’язний список**
```python
class Node:
    def __init__(self, value):
        self.value = value
        self.next = None

class SinglyLinkedList:
    def __init__(self):
        self.head = None

    def push_front(self, value):
        new_node = Node(value)
        new_node.next = self.head
        self.head = new_node

    def push_back(self, value):
        new_node = Node(value)
        if not self.head:
            self.head = new_node
            return
        cur = self.head
        while cur.next:
            cur = cur.next
        cur.next = new_node

    def print_list(self):
        cur = self.head
        while cur:
            print(cur.value, end=" -> ")
            cur = cur.next
        print("None")

# Приклад
sll = SinglyLinkedList()
sll.push_front(3)
sll.push_back(5)
sll.push_front(1)
sll.print_list()   # 1 -> 3 -> 5 -> None

```

**Двозв’язний список**
```python
class DNode:
    def __init__(self, value):
        self.value = value
        self.prev = None
        self.next = None

class DoublyLinkedList:
    def __init__(self):
        self.head = None
        self.tail = None

    def push_front(self, value):
        new_node = DNode(value)
        if not self.head:
            self.head = self.tail = new_node
        else:
            new_node.next = self.head
            self.head.prev = new_node
            self.head = new_node

    def push_back(self, value):
        new_node = DNode(value)
        if not self.tail:
            self.head = self.tail = new_node
        else:
            self.tail.next = new_node
            new_node.prev = self.tail
            self.tail = new_node

    def print_forward(self):
        cur = self.head
        while cur:
            print(cur.value, end=" <-> ")
            cur = cur.next
        print("None")

# Приклад
dll = DoublyLinkedList()
dll.push_back(10)
dll.push_back(20)
dll.push_front(5)
dll.print_forward()  # 5 <-> 10 <-> 20 <-> None
```
**Кільцевий однозв’язний список**
```python
class CNode:
    def __init__(self, value):
        self.value = value
        self.next = None

class CircularLinkedList:
    def __init__(self):
        self.tail = None  # з tail.next отримаємо head

    def push_back(self, value):
        new_node = CNode(value)
        if not self.tail:
            self.tail = new_node
            self.tail.next = self.tail
        else:
            new_node.next = self.tail.next
            self.tail.next = new_node
            self.tail = new_node

    def print_list(self):
        if not self.tail:
            print("Empty")
            return
        cur = self.tail.next  # head
        while True:
            print(cur.value, end=" -> ")
            cur = cur.next
            if cur == self.tail.next:
                break
        print("(back to head)")

# Приклад
cll = CircularLinkedList()
cll.push_back("A")
cll.push_back("B")
cll.push_back("C")
cll.print_list()  # A -> B -> C -> (back to head)
```


## <h2 style = 'text-align:center'><b>📖 </b></h2>
### 📌
## <h2 style = 'text-align:center'><b>📖 </b></h2>
### 📌

## <h2 style = 'text-align:center'><b>📖 </b></h2>
### 📌
## <h2 style = 'text-align:center'><b>📖 </b></h2>
### 📌

## <h2 style = 'text-align:center'><b>📖 </b></h2>
### 📌
## <h2 style = 'text-align:center'><b>📖 </b></h2>
### 📌
## <h2 style = 'text-align:center'><b>📖 </b></h2>
### 📌