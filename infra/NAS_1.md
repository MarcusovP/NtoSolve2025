Перечитал таску раз 40 так точно,  и написал такой код который стучится на эти три порта, скрипт нужно запустить три раза, далее просто подключится к 10.10.1.129:80. Нашел на сайте 
`https://www.exploit-db.com/exploits/29323` креды, и вошел под ними, далее нашел флаг в шеред фолдерс.

```
import socket

target_ip = "10.10.1.129"
ports = [1337, 1345, 1362]

for port in ports:
    sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    sock.settimeout(1)
    try:
        sock.connect((target_ip, port))
        print(f"Порт {port} открыт")
    except:
        print(f"Порт {port} закрыт или недоступен")
    finally:
        sock.close()

```
ТАСКА УЦУЦУГА!!!!
