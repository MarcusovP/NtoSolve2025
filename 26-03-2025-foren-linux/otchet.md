# Условие
14. Враг врага 2 - 1

Разведка донесла, что APT «Таймырский муксун» недавно провернул изящную операцию против «Таежного кролика». Поговаривают, что в инфраструктуру «Кролика» было внедрено вредоносное ПО, которое успело наделать шума. Теперь наша задача — докопаться до истины. Вперёд, охотники за «кроличьими» секретами!

Необходимые файлы должны быть на вашем оборудовании. К заданию относится образ linux системы.

1. Как был получен первичный доступ к системе? (2 балла)

# Server
- OS: Ubuntu 24.04.1 LTS (Noble Numbat)
- HOSTNAME: `fileshare`
- IP:
  - enp0s2: `10.0.2.15/24` (DNS `10.0.2.3`)
  - br-4d2c7d1f6f87: `172.18.0.1/16`
  - docker0: `172.17.0.1/16`
- USERS:
  - root
  - ioal
- ACTIVITY:
  - Jan 16
  - Jan 22
- INSTALLED:
  - VirtualBox (/opt): /opt/VBoxGuestAdditions-7.1.4/sbin/VBoxService
  
# IOAL
- ...

# ROOT
- .bash_history
  - cd nto-forensics/
  - ls
  - cp -r project /home/ioal/app
  - cd /home/ioal
  - ls
  - cd app
  - ls
  - cd ../
  - chown -h
  - chown --help
  - chown -hR ioal ./app
  - ls -al
  - exit
  - cd nto-forensics/
  - ls
  - ls -al
  - cd ../

### Сетап от оргов?
  

---

```
Jan 16 15:26:10 fileshare systemd-journald[238]: Journal stopped
-- Boot 7b8f3446c6fa49b2a65c656e50301c73 --
Jan 16 15:27:52 fileshare kernel: Linux version 6.8.0-51-generic (buildd@lcy02-amd64-091) (x86_64-linux-gnu-gcc-13
```

(обычное? выключение)

---

```
Jan 16 18:29:36 fileshare pkexec[6765]: pam_unix(polkit-1:session): session opened for user root(uid=0) by ioal(uid=1000)
Jan 16 18:29:36 fileshare pkexec[6765]: ioal: Executing command [USER=root] [TTY=unknown] [CWD=/home/ioal] [COMMAND=/usr/libexec/gvfsd-admin --spawner :1.19 /org/gtk/gvfs/exec_spaw/11 --address unix:path=/run/user/1000/bus --dir /run/user/1000]
Jan 16 18:29:36 fileshare kernel: capability: warning: `gvfsd-admin' uses 32-bit capabilities (legacy support in use)
Jan 16 18:29:36 fileshare polkitd[776]: Identity `unix-group:admin' is not valid, ignoring: No UNIX group with name admin: No such file or directory
Jan 16 18:29:38 fileshare polkitd[776]: Operator of unix-session:2 successfully authenticated as unix-user:ioal to gain TEMPORARY authorization for action org.gtk.vfs.file-operations for unix-process:6647:1089159 [/usr/bin/nautilus --gapplication-service] (owned by unix-user:ioal)
Jan 16 18:29:43 fileshare systemd[1880]: Started vte-spawn-b481b95e-cb83-4a68-b903-33701cedaec3.scope - VTE child process 6799 launched by gnome-terminal-server process 3526.
Jan 16 18:29:46 fileshare systemd[1]: fprintd.service: Deactivated successfully.
Jan 16 18:29:49 fileshare sudo[6812]:     ioal : TTY=pts/1 ; PWD=/mnt ; USER=root ; COMMAND=/usr/bin/su
Jan 16 18:29:49 fileshare sudo[6812]: pam_unix(sudo:session): session opened for user root(uid=0) by ioal(uid=1000)
Jan 16 18:29:49 fileshare su[6816]: (to root) root on pts/2
Jan 16 18:29:49 fileshare su[6816]: pam_unix(su:session): session opened for user root(uid=0) by ioal(uid=0)
```

(???)

---

```
Jan 16 19:23:59 fileshare systemd[1]: mnt-nto\x2dforensics.mount: Deactivated successfully.
Jan 16 19:23:59 fileshare systemd[1]: Unmounted mnt-nto\x2dforensics.mount - /mnt/nto-forensics.
```

Видимо как раз тут кончился тот самый сетап от оргов (это последние такие строки)

(строка 13946)


---

```
Jan 16 19:23:58 fileshare systemd-logind[799]: Power key pressed short.
```

Jan 16 -> Jan 22

```
Jan 16 19:24:02 fileshare systemd-journald[239]: Journal stopped
-- Boot cbc4e470bd1040e4bdd7ee714f5fe13b --
Jan 22 20:48:13 fileshare kernel: Linux version 6.8.0-51-generic (buildd@lcy02-amd64-091) (x86_64-linux-gnu-gcc-13
```
(обычное? выключение)

---

```
Jan 22 22:22:46 fileshare systemd-logind[814]: Power key pressed short.
...
Jan 22 22:22:50 fileshare systemd-journald[238]: Journal stopped
-- Boot b834a2fc57204e2194f02a3325857c18 --
Jan 22 22:25:57 fileshare kernel: Linux version 6.8.0-51-generic (buildd@lcy02-amd64-091) (x86_64-linux-gnu-gcc-13
```

(обычное? выключение)

---

```
Jan 22 22:26:32 fileshare systemd[1]: systemd-timedated.service: Deactivated successfully.
Jan 22 22:26:34 fileshare systemd[1]: Stopping user@120.service - User Manager for UID 120...
Jan 22 22:26:34 fileshare systemd[1351]: Activating special unit exit.target...
Jan 22 22:26:34 fileshare systemd[1351]: Stopped target default.target - Main User Target.
Jan 22 22:26:34 fileshare systemd[1351]: Stopping dbus.service - D-Bus User Message Bus...
Jan 22 22:26:34 fileshare systemd[1351]: Stopping filter-chain.service - PipeWire filter chain daemon...
Jan 22 22:26:34 fileshare systemd[1351]: Stopping pipewire-pulse.service - PipeWire PulseAudio...
Jan 22 22:26:34 fileshare systemd[1351]: Stopping xdg-document-portal.service - flatpak document portal service...
Jan 22 22:26:34 fileshare systemd[1351]: Stopping xdg-permission-store.service - sandboxed app permission store...
Jan 22 22:26:34 fileshare systemd[1351]: Stopped filter-chain.service - PipeWire filter chain daemon.
Jan 22 22:26:34 fileshare systemd[1351]: Stopped xdg-permission-store.service - sandboxed app permission store.
Jan 22 22:26:34 fileshare systemd[1351]: Stopped pipewire-pulse.service - PipeWire PulseAudio.
Jan 22 22:26:34 fileshare systemd[1351]: Stopping wireplumber.service - Multimedia Service Session Manager...
Jan 22 22:26:34 fileshare wireplumber[1380]: stopped by signal: Terminated
Jan 22 22:26:34 fileshare systemd[1351]: Stopped dbus.service - D-Bus User Message Bus.
Jan 22 22:26:34 fileshare wireplumber[1380]: disconnected from pipewire
Jan 22 22:26:34 fileshare systemd[1]: run-user-120-doc.mount: Deactivated successfully.
Jan 22 22:26:34 fileshare systemd[1351]: Stopped wireplumber.service - Multimedia Service Session Manager.
Jan 22 22:26:34 fileshare systemd[1351]: Stopping pipewire.service - PipeWire Multimedia Service...
Jan 22 22:26:34 fileshare systemd[1351]: Stopped pipewire.service - PipeWire Multimedia Service.
Jan 22 22:26:34 fileshare systemd[1351]: xdg-document-portal.service: Main process exited, code=exited, status=20/n/a
Jan 22 22:26:34 fileshare systemd[1351]: xdg-document-portal.service: Failed with result 'exit-code'.
Jan 22 22:26:34 fileshare systemd[1351]: Stopped xdg-document-portal.service - flatpak document portal service.
Jan 22 22:26:34 fileshare systemd[1351]: Removed slice session.slice - User Core Session Slice.
Jan 22 22:26:34 fileshare systemd[1351]: Stopped target basic.target - Basic System.
Jan 22 22:26:34 fileshare systemd[1351]: Stopped target paths.target - Paths.
Jan 22 22:26:34 fileshare systemd[1351]: Stopped ubuntu-report.path - Pending report trigger for Ubuntu Report.
Jan 22 22:26:34 fileshare systemd[1351]: Stopped target sockets.target - Sockets.
Jan 22 22:26:34 fileshare systemd[1351]: Stopped target timers.target - Timers.
Jan 22 22:26:34 fileshare systemd[1351]: Stopped launchpadlib-cache-clean.timer - Clean up old files in the Launchpadlib cache.
Jan 22 22:26:34 fileshare systemd[1351]: Stopped snap.firmware-updater.firmware-notifier.timer - Timer firmware-notifier for snap application firmware-updater.firmware-notifier.
Jan 22 22:26:34 fileshare systemd[1351]: Closed dbus.socket - D-Bus User Message Bus Socket.
Jan 22 22:26:34 fileshare systemd[1351]: Closed dirmngr.socket - GnuPG network certificate management daemon.
Jan 22 22:26:34 fileshare systemd[1351]: Closed gcr-ssh-agent.socket - GCR ssh-agent wrapper.
Jan 22 22:26:34 fileshare systemd[1351]: Closed gnome-keyring-daemon.socket - GNOME Keyring daemon.
Jan 22 22:26:34 fileshare systemd[1351]: Closed gpg-agent-browser.socket - GnuPG cryptographic agent and passphrase cache (access for web browsers).
Jan 22 22:26:34 fileshare systemd[1351]: Closed gpg-agent-extra.socket - GnuPG cryptographic agent and passphrase cache (restricted).
Jan 22 22:26:34 fileshare systemd[1351]: Stopping gpg-agent-ssh.socket - GnuPG cryptographic agent (ssh-agent emulation)...
Jan 22 22:26:34 fileshare systemd[1351]: Closed gpg-agent.socket - GnuPG cryptographic agent and passphrase cache.
Jan 22 22:26:34 fileshare systemd[1351]: Closed keyboxd.socket - GnuPG public key management service.
Jan 22 22:26:34 fileshare systemd[1351]: Closed pipewire-pulse.socket - PipeWire PulseAudio.
Jan 22 22:26:34 fileshare systemd[1351]: Closed pipewire.socket - PipeWire Multimedia System Sockets.
Jan 22 22:26:34 fileshare systemd[1351]: Closed pk-debconf-helper.socket - debconf communication socket.
Jan 22 22:26:34 fileshare systemd[1351]: Closed snapd.session-agent.socket - REST API socket for snapd user session agent.
Jan 22 22:26:34 fileshare systemd[1351]: Closed speech-dispatcher.socket - Speech Dispatcher Socket.
Jan 22 22:26:34 fileshare systemd[1351]: Closed gpg-agent-ssh.socket - GnuPG cryptographic agent (ssh-agent emulation).
Jan 22 22:26:34 fileshare systemd[1351]: Removed slice app.slice - User Application Slice.
Jan 22 22:26:34 fileshare systemd[1351]: Reached target shutdown.target - Shutdown.
Jan 22 22:26:34 fileshare systemd[1351]: Finished systemd-exit.service - Exit the Session.
Jan 22 22:26:34 fileshare systemd[1351]: Reached target exit.target - Exit the Session.
Jan 22 22:26:34 fileshare (sd-pam)[1352]: pam_unix(systemd-user:session): session closed for user gdm
Jan 22 22:26:34 fileshare systemd[1]: user@120.service: Deactivated successfully.
Jan 22 22:26:34 fileshare systemd[1]: Stopped user@120.service - User Manager for UID 120.
Jan 22 22:26:34 fileshare systemd[1]: user@120.service: Consumed 1.076s CPU time.
Jan 22 22:26:34 fileshare systemd[1]: Stopping user-runtime-dir@120.service - User Runtime Directory /run/user/120...
Jan 22 22:26:34 fileshare systemd[1]: run-user-120.mount: Deactivated successfully.
Jan 22 22:26:34 fileshare systemd[1]: user-runtime-dir@120.service: Deactivated successfully.
Jan 22 22:26:34 fileshare systemd[1]: Stopped user-runtime-dir@120.service - User Runtime Directory /run/user/120.
Jan 22 22:26:34 fileshare systemd[1]: Removed slice user-120.slice - User Slice of UID 120.
Jan 22 22:26:34 fileshare systemd[1]: user-120.slice: Consumed 7.491s CPU time.
Jan 22 22:26:37 fileshare systemd[1]: fprintd.service: Deactivated successfully.
Jan 22 22:26:39 fileshare systemd-timesyncd[817]: Timed out waiting for reply from 91.189.91.157:123 (ntp.ubuntu.com).
Jan 22 22:26:41 fileshare systemd-timesyncd[817]: Contacted time server 185.125.190.56:123 (ntp.ubuntu.com).
Jan 22 22:26:41 fileshare systemd-resolved[435]: Clock change detected. Flushing caches.
Jan 22 22:26:41 fileshare systemd-timesyncd[817]: Initial clock synchronization to Wed 2025-01-22 22:26:41.112346 MSK.
Jan 22 22:26:54 fileshare systemd[1]: systemd-localed.service: Deactivated successfully.
Jan 22 22:26:54 fileshare systemd[1]: systemd-hostnamed.service: Deactivated successfully.
```

(???)

(по идее пошло выключение но нет, далее логи ниже (с фаерфоксом))

---

```
Jan 22 22:22:46 fileshare systemd-logind[814]: Power key pressed short.
...
Jan 22 22:22:50 fileshare systemd-journald[238]: Journal stopped
-- Boot b834a2fc57204e2194f02a3325857c18 --
Jan 22 22:25:57 fileshare kernel: Linux version 6.8.0-51-generic (buildd@lcy02-amd64-091) (x86_64-linux-gnu-gcc-13
```

(обычное? выключение)

---

```
Jan 22 22:26:56 fileshare firefox_firefox.desktop[3195]: update.go:85: cannot change mount namespace according to change mount (/var/lib/snapd/hostfs/usr/local/share/doc /usr/local/share/doc none bind,ro 0 0): cannot open directory "/usr/local/share": permission denied
Jan 22 22:26:56 fileshare kernel: audit: type=1400 audit(1737574016.628:156): apparmor="DENIED" operation="open" class="file" profile="snap-update-ns.firefox" name="/usr/local/share/" pid=3195 comm="5" requested_mask="r" denied_mask="r" fsuid=0 ouid=0
Jan 22 22:26:56 fileshare firefox_firefox.desktop[3195]: update.go:85: cannot change mount namespace according to change mount (/var/lib/snapd/hostfs/usr/share/gimp/2.0/help /usr/share/gimp/2.0/help none bind,ro 0 0): cannot write to "/var/lib/snapd/hostfs/usr/share/gimp/2.0/help" because it would affect the host in "/var/lib/snapd"
Jan 22 22:26:56 fileshare firefox_firefox.desktop[3195]: update.go:85: cannot change mount namespace according to change mount (/var/lib/snapd/hostfs/usr/share/gtk-doc /usr/share/gtk-doc none bind,ro 0 0): cannot write to "/var/lib/snapd/hostfs/usr/share/gtk-doc" because it would affect the host in "/var/lib/snapd"
Jan 22 22:26:56 fileshare firefox_firefox.desktop[3195]: update.go:85: cannot change mount namespace according to change mount (/var/lib/snapd/hostfs/usr/share/javascript /usr/share/javascript none bind,ro 0 0): cannot write to "/var/lib/snapd/hostfs/usr/share/javascript" because it would affect the host in "/var/lib/snapd"
Jan 22 22:26:56 fileshare firefox_firefox.desktop[3195]: update.go:85: cannot change mount namespace according to change mount (/var/lib/snapd/hostfs/usr/share/libreoffice/help /usr/share/libreoffice/help none bind,ro 0 0): cannot write to "/var/lib/snapd/hostfs/usr/share/libreoffice/help" because it would affect the host in "/var/lib/snapd"
Jan 22 22:26:56 fileshare firefox_firefox.desktop[3195]: update.go:85: cannot change mount namespace according to change mount (/var/lib/snapd/hostfs/usr/share/sphinx_rtd_theme /usr/share/sphinx_rtd_theme none bind,ro 0 0): cannot write to "/var/lib/snapd/hostfs/usr/share/sphinx_rtd_theme" because it would affect the host in "/var/lib/snapd"
Jan 22 22:26:56 fileshare firefox_firefox.desktop[3195]: update.go:85: cannot change mount namespace according to change mount (/var/lib/snapd/hostfs/usr/share/xubuntu-docs /usr/share/xubuntu-docs none bind,ro 0 0): cannot write to "/var/lib/snapd/hostfs/usr/share/xubuntu-docs" because it would affect the host in "/var/lib/snapd"
Jan 22 22:26:56 fileshare tracker-miner-fs-3[3239]: (tracker-extract-3:3239): GLib-GIO-WARNING **: 22:26:56.994: Error creating IO channel for /proc/self/mountinfo: Invalid argument (g-io-error-quark, 13)
Jan 22 22:26:57 fileshare firefox[3169]: Not loading module "atk-bridge": The functionality is provided by GTK natively. Please try to not load it.
Jan 22 22:26:58 fileshare dbus-daemon[829]: [system] Activating via systemd: service name='org.freedesktop.timedate1' unit='dbus-org.freedesktop.timedate1.service' requested by ':1.105' (uid=1000 pid=3169 comm="/snap/firefox/4793/usr/lib/firefox/firefox" label="snap.firefox.firefox (enforce)")
Jan 22 22:26:58 fileshare systemd[1]: Starting systemd-timedated.service - Time & Date Service...
Jan 22 22:26:58 fileshare dbus-daemon[829]: [system] Successfully activated service 'org.freedesktop.timedate1'
Jan 22 22:26:58 fileshare systemd[1]: Started systemd-timedated.service - Time & Date Service.
Jan 22 22:26:59 fileshare kernel: audit: type=1107 audit(1737574019.619:157): pid=829 uid=101 auid=4294967295 ses=4294967295 subj=unconfined msg='apparmor="DENIED" operation="dbus_method_call"  bus="system" path="/org/freedesktop/timedate1" interface="org.freedesktop.DBus.Properties" member="GetAll" mask="send" name=":1.106" pid=3169 label="snap.firefox.firefox" peer_pid=3292 peer_label="unconfined"
                                   exe="/usr/bin/dbus-daemon" sauid=101 hostname=? addr=? terminal=?'
Jan 22 22:26:59 fileshare kernel: audit: type=1107 audit(1737574019.622:158): pid=829 uid=101 auid=4294967295 ses=4294967295 subj=unconfined msg='apparmor="DENIED" operation="dbus_method_call"  bus="system" path="/org/freedesktop/timedate1" interface="org.freedesktop.DBus.Properties" member="GetAll" mask="send" name=":1.106" pid=3169 label="snap.firefox.firefox" peer_pid=3292 peer_label="unconfined"
                                   exe="/usr/bin/dbus-daemon" sauid=101 hostname=? addr=? terminal=?'
```

(чето с фаерфоксом)

---

```
Jan 22 22:27:22 fileshare systemd[2104]: Started app-gnome-ubuntu\x2dadvantage\x2dnotification-3945.scope - Application launched by gnome-session-binary.
Jan 22 22:27:22 fileshare systemd[2104]: Started app-gnome-update\x2dnotifier-3947.scope - Application launched by gnome-session-binary.
Jan 22 22:27:25 fileshare pkexec[3983]: pam_unix(polkit-1:session): session opened for user root(uid=0) by ioal(uid=1000)
Jan 22 22:27:28 fileshare gnome-shell[2354]: Window manager warning: Ping serial 94800 was reused for window W1, previous use was for window W0.
-- Boot 872e5d1c2fb24ee9b92bd26376b26090 --
```

(unexpected shutdown?)

---

```
Jan 22 22:27:28 fileshare gnome-shell[2354]: Window manager warning: Ping serial 94800 was reused for window W1, previous use was for window W0.
-- Boot 872e5d1c2fb24ee9b92bd26376b26090 --
Jan 22 22:28:29 fileshare kernel: Linux version 6.8.0-51-generic (buildd@lcy02-amd64-091) (x86_64-linux-gnu-gcc-13 (Ubuntu 13.3.0-6ubuntu2~24.04)
```

(unexpected shutdown?)

---

```
Jan 22 22:28:30 fileshare systemd[1]: Finished snapd.apparmor.service - Load AppArmor profiles managed internally by snapd.
-- Boot f3626b38e31447448cc8c2ca2b09ab11 --
Jan 22 22:30:07 fileshare kernel: Linux version 6.8.0-51-generic (buildd@lcy02-amd64-091) (x86_64-linux-gnu-gcc-13
```

(unexpected shutdown?)

---

```
Jan 22 22:32:27 fileshare sudo[4454]:     ioal : TTY=pts/0 ; PWD=/home/ioal/app ; USER=root ; COMMAND=/usr/bin/docker compose up -d
Jan 22 22:32:27 fileshare sudo[4454]: pam_unix(sudo:session): session opened for user root(uid=0) by ioal(uid=1000)
```

В /home/ioal/app:

```
├── docker-compose.yml.enc
├── init.sql
├── init.sql.enc
└── server
    ├── assets
    │   ├── css
    │   │   ├── custom.css.enc
    │   │   ├── jquery.filer.css.enc
    │   │   └── listr.pack.css.enc
    │   ├── fonts
    │   │   ├── fontawesome-webfont.svg.enc
    │   │   ├── fontawesome-webfont.ttf.enc
    │   │   ├── fontawesome-webfont.woff.enc
    │   │   └── jquery.filer-icons
    │   │       ├── jquery-filer.css.enc
    │   │       ├── jquery-filer.eot.enc
    │   │       ├── jquery-filer-preview.html.enc
    │   │       ├── jquery-filer.svg.enc
    │   │       ├── jquery-filer.ttf.enc
    │   │       └── jquery-filer.woff.enc
    │   └── js
    │       ├── bootstrap.min.js.enc
    │       ├── custom.js.enc
    │       ├── jquery.base64.min.js.enc
    │       ├── jquery.filer.min.js.enc
    │       ├── jquery.min.js.enc
    │       ├── listr.pack.js.enc
    │       └── tether.min.js.enc
    ├── Dockerfile.enc
    ├── __init__.py.enc
    ├── requirements.txt.enc
    ├── static
    │   └── img
    ├── templates
    │   └── index.html.enc
    └── wait-for-postgres.sh.enc
```

```
-rw-r--r--  1 root root  472 янв 22 22:35 docker-compose.yml.enc
drwxr-xr-x  2 root root 4,0K мар 26 14:02 init.sql
-rw-r--r--  1 root root  528 янв 22 22:35 init.sql.enc
drwxr-x---  5 kali root 4,0K мар 26 16:12 server
```

---

```
Jan 22 22:32:41 fileshare sudo[4923]:     ioal : TTY=pts/0 ; PWD=/home/ioal/app ; USER=root ; COMMAND=/usr/bin/wireshark
Jan 22 22:32:41 fileshare sudo[4923]: pam_unix(sudo:session): session opened for user root(uid=0) by ioal(uid=1000)
```

---

```
Jan 22 22:37:15 fileshare kernel: vboxsf: Successfully loaded version 7.1.4 r165100 on 6.8.0-51-generic (LINUX_VERSION_CODE=0x6080c)
Jan 22 22:37:15 fileshare kernel: 19:37:15.482260 automount vbsvcAutomounterMountIt: Successfully mounted 'C3nt' on '/mnt'
Jan 22 22:37:17 fileshare sudo[4923]: pam_unix(sudo:session): session closed for user root
Jan 22 22:37:27 fileshare sudo[5131]:     ioal : TTY=pts/0 ; PWD=/home/ioal ; USER=root ; COMMAND=/usr/bin/su
Jan 22 22:37:27 fileshare sudo[5131]: pam_unix(sudo:session): session opened for user root(uid=0) by ioal(uid=1000)
Jan 22 22:37:27 fileshare su[5133]: (to root) root on pts/1
Jan 22 22:37:27 fileshare su[5133]: pam_unix(su:session): session opened for user root(uid=0) by ioal(uid=0)
Jan 22 22:37:46 fileshare tracker-miner-fs-3[5143]: (tracker-extract-3:5143): GLib-GIO-WARNING **: 22:37:46.531: Error creating IO channel for /proc/self/mountinfo: Invalid argument (g-io-error-quark, 13)
-- Boot dde92f2cb3de4b6c88fb6d9497008726 --
Mar 26 14:00:31 fileshare kernel: Linux version 6.8.0-51-generic (buildd@lcy02-amd64-091) (x86_64-linux-gnu-gcc-13 (Ubuntu 13.3.0-6ubuntu2~24.04) 13.3.0, GNU ld (GNU Binutils for Ubuntu) 2.42) #52-Ubuntu SMP PREEMPT_DYNAMIC Thu Dec  5 13:09:44 UTC 2024 (Ubuntu 6.8.0-51.52-generic 6.8.12)
```

(shutdown after sudo su)
