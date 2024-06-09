# b''について (Python3の場合)
> Python 2 では str 型をテキストとバイナリデータのどちらにも使うことが出来ていました。...()... 
str と bytes の両方のコンストラクタは同じ引数を与えても Python 2 と 3 で異なる意味を持ちます。Python 2 で bytes に数値を与えると、整数の文字列表現を生成します: bytes(3) == '3' 。ですが Python 3 では、 bytes に整数を与えると、整数値で与えたぶんの長さの、null バイトで埋められたバイト列を生成します: bytes(3) == 略b'\x00\x00\x00' 。似たような話はバイト列オブジェクトを str に与える場合にも起こります。Python 2 ではバイト列が渡したものがそのまま戻ってきます: str(b'3') == b'3' 。対して Python 3 では、バイト列オブジェクトの文字列表現になって返ってきます: str(b'3') == "b'3'" 。

[https://docs.python.org/ja/3.6/howto/pyporting.html](https://docs.python.org/ja/3.6/howto/pyporting.html)

## b'hoge' -> hoge
decodeを使う
```
>>> type(b'hoge')
<class 'bytes'>
>>> b'hoge'.decode()
'hoge'
```
## hoge -> b'hoge'
encodeを使う
```
>>> type('hoge')
<class 'str'>
>>> 'hoge'.encode()
b'hoge'
```

# split
## str split
```
>>> a = 'aaa,bbb,ccc'
>>> a.split(',')[0]
'aaa'
```
## bytes split
```
>>> a = b'¥68¥31.::98::.¥90¥78'
>>> a.split(b'.::98::.')[0]
b'¥68¥31'
```

# base64
[https://docs.python.org/ja/3/library/base64.html](https://docs.python.org/ja/3/library/base64.html)
## encode "plain" to "base64"
bytes -> bytes
```
>>> import base64
>>> print(base64.b64encode('こんにちは'.encode()))
b'44GT44KT44Gr44Gh44Gv'
```

## decode "base64" to "plain"
str or bytes -> bytes
```
>>> import base64
>>> 
>>> base64.b64decode(b'44GT44KT44Gr44Gh44Gv')
b'\xe3\x81\x93\xe3\x82\x93\xe3\x81\xab\xe3\x81\xa1\xe3\x81\xaf'
>>>
>>> b'44GT44KT44Gr44Gh44Gv'.decode()
'44GT44KT44Gr44Gh44Gv'
>>> base64.b64decode(b'44GT44KT44Gr44Gh44Gv')
b'\xe3\x81\x93\xe3\x82\x93\xe3\x81\xab\xe3\x81\xa1\xe3\x81\xaf'
>>>
>>> print(base64.b64decode(b'44GT44KT44Gr44Gh44Gv').decode())
こんにちは
```
