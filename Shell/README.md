# my setting on ~/.bashrc
```
export PATH=$PATH:/home/user/oledump/oledump_V0_0_69
export PATH=$PATH:/home/user/RegRipper3.0
export PATH=$PATH:/home/user/impacket/examples
export PATH=$PATH:/home/user/ubuntu-latest.Release/build
setxkbmap -layout jp
```

# 見やすいパス表示
```
$ echo $PATH | sed -e 's/:/\n/g' | sort
```

# ファイル検索
findコマンドの-printもしくは-name "*"で、  
指定のディレクトリ配下すべてのファイルをリストアップしてから、  
grepする
```
$ find . -name "*" | grep -i read
or
$ find ./ -print | grep -i "hoge"
```

# tlsの鍵生成
## 公開鍵ペアの作成
```
ssh-keygen -t rsa -b 4096 -c "#8"
```

## ssh公開鍵のコピー
```
ssh-copy-id root@XXX.XXX.XXX.XXX
```

## ssh configファイルの作成
```
cat .ssh/config 
host jump
hostname XXX.XXX.XXX.XXX
user root
port 22
identityfile ~/.ssh/id_rsa

ssh jump
```

# for
## lsのファイル全部に処理をかけたいとき
```
for f in `ls`; do file $f; done 
```
forの中で複数行の処理をかけたいときは;で足す。
```
for f in `ls`; do file $f; echo "AAA"; done 
```

## fileの中の各行でループ処理を回したいとき
```
cat test.txt | while read l ; do echo "www"; echo $l; done
```

## Reading lines from a file  
https://genzouw.com/entry/2020/04/15/140408/1972/    
- `|| [[ -n "$line" ]]` on shell loop  
    `|| [ -n "$line" ]` ensures that the loop continues if the last line of the file doesn't end with a newline character.  
    "while read line; do echo $line; done < hoge.txt" cannot read a content if the file has only one line and no "\n" at the end of the line.  
    "vim" seems to add "\n" automatically at the end of a line.  
- `IFS=`  
    With `IFS=`, leading and trailing spaces are not removed during the read operation.  
- `-r`  
    With `-r` option, unusual behavior can be avoided when there is a \ at the end of each line.  
- Test  
    test file.
    ```zsh
    $ vim test
    $ cat test
    This is 1.\nHoge
    This is 2.
        This is 3 with single space.
            This is 4 with single tab.
    This is 5.
    $
    $ echo -n "aaaaa" >> test
    $
    $ cat test
    This is 1.\nHoge
    This is 2.
    This is 3 with single space.
            This is 4 with single tab.
    This is 5.
    aaaaa%
    ```
    bad  
    ```zsh
    $ while read f || [[ -n "$f"]]; do echo $f; done < test
    zsh: parse error near ';'
    ```
    good
    ```zsh
    $ while read f || [[ -n "$f" ]]; do echo $f; done < test
    This is 1.nHoge
    This is 2.
    This is 3 with single space.
    This is 4 with single tab.
    This is 5.
    aaaaa
    ```
    bad  
    ```zsh
    $ while read f || [[ -n $f]]; do echo $f; done < test
    zsh: parse error near ';'
    ```
    good
    ```zsh
    $ while read f || [[ -n $f ]]; do echo $f; done < test
    This is 1.nHoge
    This is 2.
    This is 3 with single space.
    This is 4 with single tab.
    This is 5.
    aaaaa
    ```
    good
    ```zsh
    $ while IFS= read -r f || [[ -n $f ]]; do echo $f; done < test
    This is 1.
    Hoge
    This is 2.
        This is 3 with single space.
            This is 4 with single tab.
    This is 5.
    aaaaa
    ```

# シェルスクリプト
## 変数の使い方
```
URL=google
echo "https://$URL.com"
> https://google.com

echo https://$URL.com
> https://google.com

echo "https://${URL}.com"
> https://google.com
```
ダメな例）{}で＄を囲ってはいけない
```
echo "https://{$URL}.com"
> https://{google}.com
```

ダメな例）シングルクォートは変数が変換されない
```
echo 'https://$URL.com'
> https://$URL.com
```

# Example
## "awk" and "for" + "decimal <-> hex"
```
$ 
$ cat vadinfo_0x000000007d336950.txt | wc -l
1771
$ 
$ cat vadinfo_0x000000007d336950.txt | head -15
************************************************************************
Pid:   2448
VAD node @ 0xfffffa800464e4a0 Start 0x0000000075c20000 End 0x0000000075caffff Tag Vad 
Flags: CommitCharge: 2, Protection: 7, VadType: 2
Protection: PAGE_EXECUTE_WRITECOPY
Vad Type: VadImageMap
ControlArea @fffffa80033a01e0 Segment fffff8a00086aaf0
NumberOfSectionReferences:          1 NumberOfPfnReferences:          37
NumberOfMappedViews:                1 NumberOfUserReferences:          2
Control Flags: File: 1, Image: 1
FileObject @fffffa800339fc70, Name: \Device\HarddiskVolume1\Windows\SysWOW64\gdi32.dll
First prototype PTE: fffff8a00086ab38 Last contiguous PTE: fffffffffffffffc
Flags2: Inherit: 1

VAD node @ 0xfffffa8004661c80 Start 0x000000006de70000 End 0x000000006dec0fff Tag Vad 
$ 
$ cat vadinfo_0x000000007d336950.txt | grep VAD | grep -E "220000|2a10000|2a80000" | awk -F ' ' '{printf("%s %s\n", $8, $6)}' | while read l; do a=($l); end=${a[0]}; start=${a[1]}; size=$((end-start)); echo $end - $start = `printf '%X\n' $size`; done
0x000000000023ffff - 0x0000000000220000 = 1FFFF
0x0000000002a2cfff - 0x0000000002a10000 = 1CFFF
0x0000000002ab6fff - 0x0000000002a80000 = 36FFF
$ 
```

## stringの10進>stringの16進->エンディアン逆のstringの16進->ascii
```
printf '%X\n' 1416573010 | xargs echo | fold -w2 | /bin/tac | tr -d '\n' | xxd -r -p && echo ''
```
以下、↑の流れを説明
```
$ printf '%X\n' 1416573010
546F3052
$ 
$ printf '%X\n' 1416573010 | xargs echo
546F3052
$ 
$ printf '%X\n' 1416573010 | xargs echo | fold -w2
54
6F
30
52
$ 
$ printf '%X\n' 1416573010 | xargs echo | fold -w2 | tac
52
30
6F
54
$ 
$ printf '%X\n' 1416573010 | xargs echo | fold -w2 | tac | tr -d '\n' && echo ''
52306F54
$ 
$ printf '%X\n' 1416573010 | xargs echo | fold -w2 | tac | tr -d '\n' | xxd -r -p && echo ''
R0oT
```
tacコマンドが使えない時がある。  
/usr配下のtacがなぜか使えないので、/bin配下のtacを使うようにalias設定する。  
/usr/locl/bin/tac -> /bin/tac  
```
$ which tac
/usr/local/bin/tac
$
$ printf '%X\n' 1416573010 | xargs echo | fold -w2 | tac | tr -d '\n' | xxd -r -p && echo ''
Demo License expired, contact info@tzworks.net to purchase a commercial license.
$
$ alias tac='/bin/tac'
```
