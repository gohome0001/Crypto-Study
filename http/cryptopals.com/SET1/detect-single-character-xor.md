# detect single-character xor

well.. It saids the encryption key is a single character.

but I will just put it as a single byte ^^..

It will be simple with the previous code I wrote.

## code

```python
# !/usr/bin/python2.7
import string
import collections

f = open("4_ct.txt")
data = f.read()
f.close()
f = open("result.txt",'wb')
data = data.split("\r\n") # CRLF

for ct in data:
    f.write("CT : "+ct+"\r\n")
    ct=ct.decode('hex')
    top = collections.Counter(ct).most_common(1)[0][0]
    for cand in ['e',' ','t','o','i','n','E','T','A','O','I','N']:
        key = ord(top)^ord(cand)
        ts = ""
        for i in ct:
            ts += chr(ord(i)^key)
        if all(c in string.printable for c in ts):
            f.write("key %d : %s\r\n" % (key, ts))
            ts = ""
        else:
            continue
    f.write("\r\n---------next ct---------\r\n")
```
CT : line 171 string.
key : 53
PT : Now that the party is jumping

Don't know why the last CT occurs error;;

