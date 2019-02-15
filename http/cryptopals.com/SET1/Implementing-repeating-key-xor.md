# Implementing repeating-key xor

simple.

but we should know, this encryption method is also vulnerable to frequency tests.

## code

```python
# !/usr/bin/python2.7

def get_key(strlen, key):
    for i in range(strlen):
        yield key[(i % len(key))]

def enc_dec(text,key):
    key = list(get_key(len(text),key))
    result = map(lambda x,y:chr(ord(x)^ord(y)), text,key)
    return "".join(result)
    

text = "My Favorate Yogurt is plain taste Yogurt!"
key = "CTASTE"

enc_dec(text,key)



```