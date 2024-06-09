# `&nbsp;` の意味
## ノーブレークスペース
要は半角スペース。  
本来は「改行の禁止（＝当該空白部分での自動的な改行を防ぐ）」という機能を果たすものだそう。  
英文などの場合、単語と単語の間は半角スペースで区切られるが、例えば会社名など複数単語でひとつながりのものを改行させずに表示したいときなどに使うもの。  
```
<?php echo "&nbsp;&nbsp;" . $message; ?>
```
[cf. 参考ブログ](https://www.asoblock.net/contents/nbsp)

# inputタグのinputmode
スマホなどで、入力フォームにタッチした時に、強制的に出てくるキーボードの種類を指定できる。
```
<input type="text" inputmode="text">

// システムのキーボードを表示させない指定
<input type="text" inputmode="none">
```
[https://developer.mozilla.org/ja/docs/Web/HTML/Global_attributes/inputmode](https://developer.mozilla.org/ja/docs/Web/HTML/Global_attributes/inputmode)

# inputタグのrequired
入力必須を指定する
```
<input required type="text" name="yourname"> 
```
[http://www.htmq.com/html5/input_required.shtml](http://www.htmq.com/html5/input_required.shtml)

# fieldsetタグ
formタグで定義するフォームの入力項目をグループ化するためのタグ。  
ただまとめたい時にも使う。  
グループ化を行うことにより、グループの間をtabキーで簡単に移動することが可能になる。
```
<form method="post" action="sample.cgi">
  <fieldset>
    <legend>データ1</legend>
    <p>名前<input type="text" name="name1" size="10"></p>
    <p>住所<input type="text" name="address1" size="30"></p>
  </fieldset>
  <input type="submit" value="送信">
</form>
```
[https://html-coding.co.jp/annex/dictionary/html/fieldset/](https://html-coding.co.jp/annex/dictionary/html/fieldset/)