# re
#Python #re

## Example
```python
import re

raw = b'hogehogehogehoge'

match = re.search(
    rb'(?P<header>Content-Range:\s+bytes\s+(\d+)-(\d+)/(\d+))\r\n\r\n(?P<body>.*)',
    raw,
    flags=re.MULTILINE | re.DOTALL
)

if match:
    body = match.group('header')
    body = match.group('body')
    start = int(match.group(2))
    end = int(match.group(3))
```
### `rb''` vs. `b''`
- `rb''` :  a raw bytes literal — backslashes are not escaped.  
- `b''` : a regular bytes literal — backslashes must be escaped (\\).  

`rb''` is better:  
Easier to write and read regex patterns like rb'\d+\s\w+'.  
Avoids mistakes with double backslashes (e.g. b'\\d+\\s\\w+').

✅ Use `rb''` when writing regex for bytes.  
✅ Use `r''` when writing regex for strings.  
🚫 Avoid `b''` for regex patterns — it’s error-prone and hard to read.  

### `re.MULTILINE`
```python
import re

text = "first line\nsecond line"

# Without MULTILINE
print(re.findall(r"^second", text))  # []

# With MULTILINE
print(re.findall(r"^second", text, flags=re.MULTILINE))  # ['second']
```
- Makes ^ and $ match the start and end of each line, not just the whole string.
- Useful when working with multi-line text.
### `re.DOTALL`
```python
import re

text = "line1\nline2"

# Without DOTALL
print(re.search(r"line1.+line2", text))  # None

# With DOTALL
print(re.search(r"line1.+line2", text, flags=re.DOTALL))  # Match found
```
- Makes . (dot) match newline characters (\n) as well.
- Normally, . matches any character except \n.