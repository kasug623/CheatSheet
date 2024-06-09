# php全体のオプションを変更できる関数
## @mail  で飛ばす先のSMTPサーバの変更
```
// SMTP settings
$smtp_sender = "bwapp@mailinator.com";
$smtp_recipient = "bwapp@mailinator.com";
$smtp_server = "";

ini_set( "SMTP", $smtp_server);
```
[cf. 公式ドキュメント](https://www.php.net/manual/ja/function.ini-set.php)  
[cf. 参考ブログ](https://deep-blog.jp/engineer/13312/)

# @の意味
PHP の式の前に付けた場合、  
その式により生成されたエラーメッセージは無視される。
```
// Sends the e-mail
$status = @mail($recipient, $subject, $content, "From: $email");
```
[cf. 参考ブログ](https://tektektech.com/doll-at-mean/)

# . の意味
文字列の連結
```
<?php echo "&nbsp;&nbsp;" . $message; ?>
```
[cf. 公式ドキュメント](https://www.php.net/manual/ja/language.operators.string.php)

# $_SERVER['SCRIPT_FILENAME']
現在実行しているスクリプトの絶対パス。
例：/virtual/account/public_html/example.com/foo/filename.php  
```
<form action="<?php echo($_SERVER["SCRIPT_NAME"]);?>" method="POST">
```
[cf. 参考ブログ](https://webmaster.chielog.com/php/164.html)