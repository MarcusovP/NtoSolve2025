https://gitlab.polygon/team9/diary/-/commit/9db91b8dac79ce5f03cd26c951c4e375bcc3c4d6
https://gitlab.polygon/team9/diary/-/commit/ee3902010f382575f8b1c7dd6ac964c6e766211b

# 0 Убрали выключение проверок csrf
```python
@csrf_exempt
```
# 1 используем env
```python
SECRET_KEY = "Abqh4LSVdohqrlhtalvifAmEsymAvY9p"

SECRET_KEY = os.getenv('DJANGO_SECRET_KEY', 'SECRET_KEY')
ИЛИ
SECRET_KEY = os.getenv('DJANGO_SECRET_KEY', 'SECRET_KEY')

SECRET_KEY = "Abqh4LSVdohqrlhtalvifAmEsymAvY9p"  # Замените на ваш секретный ключ
ALGORITHM = "HS256"
ACCESS_TOKEN_EXPIRE_MINUTES = 10000000000000000000
MAX_LOGIN_ATTEMPTS = 10000000000000000000
BLOCK_TIME_MINUTES = 0

SECRET_KEY = os.getenv('DJANGO_SECRET_KEY', 'SECRET_KEY')  # Замените на ваш секретный ключ
ALGORITHM = "HS256"
ACCESS_TOKEN_EXPIRE_MINUTES = 60 * 10
MAX_LOGIN_ATTEMPTS = 3
BLOCK_TIME_MINUTES = 5
``` 
# 2 используем env
```python
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
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': os.getenv('POSTGRES_DB', 'postgres'),
        'USER': os.getenv('POSTGRES_USER', 'postgres'),
        'PASSWORD': os.getenv('POSTGRES_PASSWORD', 'postgres'),
        'HOST': os.getenv('POSTGRES_HOST', 'db'),
        'PORT': os.getenv('POSTGRES_PORT', '5432'),
    }
}
```
# 3 фикс sqli
```python
@csrf_exempt
def user_login(request):
    if request.method == 'POST':
        username = request.POST.get('username')
        password = request.POST.get('password')
        with connection.cursor() as cursor:
            query = f"SELECT * FROM myapp_user WHERE login = '{username}' AND password = '{password}'"
            cursor.execute(query)
            user = cursor.fetchone()
        
        if user:
            # Небезопасная авторизация
            user = django_user.objects.get(username=username)
            login(request, user)
            return redirect("/")
        else:
            return render(request, 'mysite/login.html', {"message": "Ошибка входа"})
    return render(request, 'mysite/login.html')

@csrf_exempt
def user_login(request):
    if request.method == 'POST':
        username = request.POST.get('username')
        password = request.POST.get('password')
        
        hashed_password = sha256(password.encode()).hexdigest()
        
        with connection.cursor() as cursor:
            query = "SELECT * FROM myapp_user WHERE login = %s AND password = %s"
            cursor.execute(query, [username, hashed_password])
            user = cursor.fetchone()
        
        if user:
            django_user_obj = django_user.objects.get(username=username)
            login(request, django_user_obj)
            return redirect("/")
        else:
            return render(request, 'mysite/login.html', {"message": "Ошибка входа"})
    return render(request, 'mysite/login.html')
```
# 4 хешируем пароли
```python
password=cd['password'], 
password=sha256(cd['password'].encode()).hexdigest(),
```
# 5 расставили декораторы для ручек, которые требуют входа
```python
@login_required
```
# 6 фикс sqli
```python
query = f"""
                INSERT INTO myapp_mark 
                (mark, date, user_id, subject_id)
                VALUES ({mark_value}, '{date}', 
                (SELECT id FROM myapp_user WHERE login = '{student_login}'), 
                (SELECT id FROM myapp_subject WHERE name = '{sub_name}'))
            """
            cursor.execute(query)

query = """
                INSERT INTO myapp_mark 
                (mark, date, user_id, subject_id)
                VALUES (%s, %s, 
                (SELECT id FROM myapp_user WHERE login = %s), 
                (SELECT id FROM myapp_subject WHERE name = %s))
            """
            cursor.execute(query, (mark_value, date, student_login, sub_name))

query1 = f"SELECT * FROM myapp_user WHERE surname LIKE '{query}' OR name LIKE '{query}'"
        cursor.execute(query1)

query1 = """
            SELECT * FROM myapp_user 
            WHERE surname LIKE %s OR name LIKE %s
        """
        cursor.execute(query1, (query, query))
```
