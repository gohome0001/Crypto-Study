# fixed xor

this problem is simple. just xor given two hex data.

## code

```python
# !/usr/bin/python2.7

def xor_hex_str(x,y):
    x = int(x,16)
    y = int(y,16)
    r = hex(x ^ y)[2:-1]
    return r

input1 = "1c0111001f010100061a024b53535009181c"
input2 = "686974207468652062756c6c277320657965"
result = xor_hex_str(input1,input2)
```