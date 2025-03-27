Не используется декоратор, что приводит к общедоступности ручек
```python
@login_required
```
Черный список использует разные ключи
```python
def is_token_blacklisted(token):
    return redis_client.exists(f"blacklist:{token}")
    

def blacklist_token(token: str):
    redis_client.set(f"black:{token}", "blocked")
```
SQLi
```python
with connection.cursor() as cursor:
    query = f"SELECT * FROM myapp_user WHERE login = '{username}' AND password = '{password}'"
    cursor.execute(query)
    user = cursor.fetchone() 
```
Декоратор
```python
@csrf_exempt
```
Пароли не хешируются
```python
cd = form.cleaned_data

new_user = User(login=cd['username'], 
                password=cd['password'], 
                surname=cd['surname'], 
                name=cd['name'], 
                lastname=cd['lastname'], 
                photo='', 
                role=cd['role'], 
                group=group, email=cd['email'])
```
Опечатка при проверки роли
```python
elif user.role == 'STU':
    marks = Mark.objects.filter(user=user, subject=sub)
....
if user == 'STU':
    return redirect("/")
```
CORS nginx config
```python
add_header Access-Control-Allow-Origin * always;
proxy_set_header Access-Control-Allow-Origin *;
```
Хардкод секретов
```python
# SECRET_KEY = os.popen("cat /run/secrets/secret_key").read()
SECRET_KEY = "Abqh4LSVdohqrlhtalvifAmEsymAvY9p"


SQLALCHEMY_DATABASE_URL = f'postgresql://postgres:postgres@db:5432/postgres'


DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': "postgres",
        'USER': "postgres",
        'PASSWORD': "postgres",
        'HOST': 'db',
        'PORT': '5432'
    }
}
```
Поиск доступен всем + sqli
```python
@csrf_exempt
def search(request):
    query = request.GET.get('q', '')
    with connection.cursor() as cursor:
        query1 = f"SELECT * FROM myapp_user WHERE surname LIKE '{query}' OR name LIKE '{query}'"
        cursor.execute(query1)
        results = cursor.fetchall()
    return render(request, 'mysite/search.html', {'results': results})
```
