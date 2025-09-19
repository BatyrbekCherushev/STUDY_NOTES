# <h1 style = 'text-align:center'><b>📌**Бібліотека requests**</b></h2>

```python
import requests
import json
```

## <h2 style = 'text-align:center'><b>📌**LINKS**</b></h2>


---

## <h2 style = 'text-align:center'><b>📌 **ОСНОВНІ GET ЗАПИТИ**</b></h2>
### 📋 Простий GET запит
```python
response = requests.get('https://httpbin.org/get')
print(f"Статус: {response.status_code}")
print(f"URL: {response.url}")
print(f"Заголовки відповіді: {dict(response.headers)}")
print(f"JSON відповідь: {response.json()}")
print()
```

### 📋 GET з параметрами
```python
params = {'name': 'John', 'age': 30, 'city': 'Kyiv'}
response = requests.get('https://httpbin.org/get', params=params)
print(f"URL з параметрами: {response.url}")
print(f"Параметри в відповіді: {response.json()['args']}")
print()
```

### 📋 GET з заголовками
```python
headers = {
    'User-Agent': 'MyApp/1.0',
    'Accept': 'application/json',
    'Custom-Header': 'test-value'
}
response = requests.get('https://httpbin.org/get', headers=headers)
print(f"Заголовки запиту: {response.json()['headers']}")
print()
```
## <h2 style = 'text-align:center'><b>📌 **POST ЗАПИТИ**</b></h2>


### 📋 POST з JSON даними
```python
json_data = {'username': 'john_doe', 'email': 'john@example.com', 'age': 25}
response = requests.post('https://httpbin.org/post', json=json_data)
print(f"POST JSON статус: {response.status_code}")
print(f"Надіслані JSON дані: {response.json()['json']}")
print()
```

### 📋 POST з form данними
```python
form_data = {'name': 'Ivan', 'profession': 'developer', 'country': 'Ukraine'}
response = requests.post('https://httpbin.org/post', data=form_data)
print(f"POST form статус: {response.status_code}")
print(f"Надіслані form дані: {response.json()['form']}")
print()
```

### 📋 POST з файлом (симуляція)
```python
files = {'file': ('test.txt', 'Це тестовий файл', 'text/plain')}
response = requests.post('https://httpbin.org/post', files=files)
print(f"POST файл статус: {response.status_code}")
print(f"Інформація про файл: {response.json()['files']}")
print()
```
## <h2 style = 'text-align:center'><b>📌 **ІНШІ HTTP МЕТОДИ**</b></h2>


### 📋 PUT запит
```python
put_data = {'id': 123, 'name': 'Updated Name', 'status': 'active'}
response = requests.put('https://httpbin.org/put', json=put_data)
print(f"PUT статус: {response.status_code}")
print(f"PUT дані: {response.json()['json']}")
print()
```

### 📋 PATCH запит
```python
patch_data = {'status': 'inactive'}
response = requests.patch('https://httpbin.org/patch', json=patch_data)
print(f"PATCH статус: {response.status_code}")
print(f"PATCH дані: {response.json()['json']}")
print()
```

### 📋 DELETE запит
```python
response = requests.delete('https://httpbin.org/delete')
print(f"DELETE статус: {response.status_code}")
print(f"DELETE URL: {response.json()['url']}")
print()
```

### 📋 HEAD запит (тільки заголовки)
```python
response = requests.head('https://httpbin.org/get')
print(f"HEAD статус: {response.status_code}")
print(f"HEAD заголовки: {dict(response.headers)}")
print(f"HEAD контент (порожній): '{response.text}'")
print()
```
---

## <h2 style = 'text-align:center'><b>📌 **РОБОТА З АВТЕНТИФІКАЦІЄЮ**</b></h2>


### 📋 Basic Auth
```python
response = requests.get('https://httpbin.org/basic-auth/user/pass',
                        auth=HTTPBasicAuth('user', 'pass'))
print(f"Basic Auth статус: {response.status_code}")
print(f"Basic Auth відповідь: {response.json()}")
print()
```

### 📋 Bearer token
```python
headers = {'Authorization': 'Bearer fake-jwt-token-123'}
response = requests.get('https://httpbin.org/bearer', headers=headers)
print(f"Bearer token статус: {response.status_code}")
if response.status_code == 200:
    print(f"Bearer відповідь: {response.json()}")
print()
```

---

## <h2 style = 'text-align:center'><b>📌 **СЕСІЇ**</b></h2>

```python
session = requests.Session()
session.headers.update({'User-Agent': 'MyApp Session/1.0'})
```

### 📋 Встановлення cookie через сесію
```python
response = session.get('https://httpbin.org/cookies/set/session_id/abc123')
print(f"Встановлення cookie статус: {response.status_code}")
```

### 📋 Перевірка cookie в сесії
```python
response = session.get('https://httpbin.org/cookies')
print(f"Cookie в сесії: {response.json()}")
print()
```

---

## <h2 style = 'text-align:center'><b>📌 **ОБРОБКА ПОМИЛОК**</b></h2>
```python
try:
    # Запит до неіснуючого ендпоінту
    response = requests.get('https://httpbin.org/status/404', timeout=5)
    print(f"404 статус: {response.status_code}")
    response.raise_for_status()  # Викине HTTPError
except requests.exceptions.HTTPError as e:
    print(f"HTTP помилка: {e}")
except requests.exceptions.RequestException as e:
    print(f"Загальна помилка запиту: {e}")
print()
```
```python
try:
    # Таймаут
    response = requests.get('https://httpbin.org/delay/10', timeout=2)
except requests.exceptions.Timeout:
    print("Перевищено час очікування запиту")
print()
```

---

## <h2 style = 'text-align:center'><b>📌 **ПАРАМЕТРИ ЗАПИТУ**</b></h2>


### 📋 Заборона редиректів
```python
response = requests.get('https://httpbin.org/redirect/1', allow_redirects=False)
print(f"Без редиректу статус: {response.status_code}")
print(f"Location заголовок: {response.headers.get('Location')}")
print()
```

### 📋 З редиректами (за замовчуванням)
```python
response = requests.get('https://httpbin.org/redirect/1')
print(f"З редиректом статус: {response.status_code}")
print(f"Історія редиректів: {[r.status_code for r in response.history]}")
print()
```

---

## <h2 style = 'text-align:center'><b>📌 **РОБОТА З JSON**</b></h2>

### 📋 Отримання JSON
```python
response = requests.get('https://jsonplaceholder.typicode.com/posts/1')
if response.status_code == 200:
    post = response.json()
    print(f"Пост ID: {post['id']}")
    print(f"Заголовок: {post['title']}")
    print(f"Автор: {post['userId']}")
print()
```

### 📋 Надсилання JSON
```python
new_post = {
    'title': 'Мій новий пост',
    'body': 'Це тіло мого нового поста',
    'userId': 1
}
response = requests.post('https://jsonplaceholder.typicode.com/posts', json=new_post)
if response.status_code == 201:
    created_post = response.json()
    print(f"Створено пост з ID: {created_post['id']}")
print()
```

---

## <h2 style = 'text-align:center'><b>📌 **ПЕРЕВІРКА СТАТУСУ**</b></h2>
# ===== 9. ПЕРЕВІРКА СТАТУСУ =====
```python

response = requests.get('https://httpbin.org/status/200')
print(f"Статус 200 - OK: {response.ok}")
```

```python
response = requests.get('https://httpbin.org/status/404')
print(f"Статус 404 - OK: {response.ok}")
print(f"Статус код: {response.status_code}")
print(f"Причина: {response.reason}")
print()
```

---

## <h2 style = 'text-align:center'><b>📌 **ПОТОКОВЕ ЧИТАННЯ**</b></h2>

### 📋 Потокове читання великого контенту
```python
response = requests.get('https://httpbin.org/stream/3', stream=True)
print("Потокове читання:")
for line in response.iter_lines():
    if line:
        data = json.loads(line.decode('utf-8'))
        print(f"  Рядок {data['id']}: {data['url']}")
print()
```

---

## <h2 style = 'text-align:center'><b>📌 **COOKIES**</b></h2>

### 📋 Надсилання cookies
```python
cookies = {'user_preference': 'dark_theme', 'language': 'uk'}
response = requests.get('https://httpbin.org/cookies', cookies=cookies)
print(f"Надіслані cookies: {response.json()['cookies']}")
print()
```

---

## <h2 style = 'text-align:center'><b>📌 **ЗАГОЛОВКИ ВІДПОВІДІ**</b></h2>
```python

response = requests.get('https://httpbin.org/response-headers?Content-Type=application/json&Server=MyServer')
print("Важливі заголовки відповіді:")
print(f"  Content-Type: {response.headers.get('Content-Type')}")
print(f"  Server: {response.headers.get('Server')}")
print(f"  Date: {response.headers.get('Date')}")
print(f"  Content-Length: {response.headers.get('Content-Length')}")
print()
```
---

## <h2 style = 'text-align:center'><b>📌 **РЕАЛЬНИЙ API ПРИКЛАД**</b></h2>
```python
print("=== 13. Реальний API приклад (GitHub) ===")

try:
    # Інформація про користувача GitHub
    response = requests.get('https://api.github.com/users/torvalds')
    if response.status_code == 200:
        user = response.json()
        print(f"GitHub користувач: {user['login']}")
        print(f"Ім'я: {user['name']}")
        print(f"Публічні репозиторії: {user['public_repos']}")
        print(f"Підписники: {user['followers']}")

    # Пошук репозиторіїв
    params = {'q': 'requests python', 'sort': 'stars', 'per_page': 3}
    response = requests.get('https://api.github.com/search/repositories', params=params)
    if response.status_code == 200:
        search_results = response.json()
        print(f"\nЗнайдено репозиторіїв: {search_results['total_count']}")
        print("Топ 3 репозиторії:")
        for repo in search_results['items']:
            print(f"  {repo['name']} - ⭐ {repo['stargazers_count']}")

except requests.exceptions.RequestException as e:
    print(f"Помилка при роботі з GitHub API: {e}")

print("\n=== Приклади завершено ===")
```
