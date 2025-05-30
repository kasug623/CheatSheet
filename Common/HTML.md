# `&nbsp;`
non-breaking space
Basically, it's a space.
Its original purpose is to prevent line breaks—that is, to stop the browser from automatically breaking the line at that space.
In English text, words are typically separated by half-width spaces, but in cases like company names composed of multiple words, this is used to keep them together on the same line without breaking.
```
<?php echo "&nbsp;&nbsp;" . $message; ?>
```
ref. https://www.asoblock.net/contents/nbsp

# "input" tag
## "inputmode" option
You can specify the type of on-screen keyboard that appears automatically when a user taps on an input form, such as on a smartphone.
```
<input type="text" inputmode="text">

// To prevent the system keyboard from appearing
<input type="text" inputmode="none">
```
ref. https://developer.mozilla.org/ja/docs/Web/HTML/Global_attributes/inputmode

## "required" option
Specify that input is required.
```
<input required type="text" name="yourname"> 
```
ref. http://www.htmq.com/html5/input_required.shtml

# "fieldset" tag
A tag used to group "input" fields defined within a "form" tag.
It can also be used simply for organizational purposes.
By grouping "input" fields, it becomes easier to navigate between groups using the Tab key.
```
<form method="post" action="sample.cgi">
  <fieldset>
    <legend>Data 1</legend>
    <p>name<input type="text" name="name1" size="10"></p>
    <p>address<input type="text" name="address1" size="30"></p>
  </fieldset>
  <input type="submit" value="Push Here!">
</form>
```
ref. https://html-coding.co.jp/annex/dictionary/html/fieldset/