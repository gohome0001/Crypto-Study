# convert hex to base64

given string is hex format data.

and should generate properly-encoded bas64 data.

these are the given rules. 
- Operate on raw bytes
- use hex and base64 for pretty-printing.

but..will ignore it.

## how base64 works.

## **encoding**

if there's a binary data like this
`11010110 01010111 11011101 10101010(2)`

then, base64 encodes it from MSB to LSB, for 6 bits at a time.

and it has a substitution table.

ref : 
[https://en.wikipedia.org/wiki/Base64#Base64_table](https://en.wikipedia.org/wiki/Base64#Base64_table)

## padding 

Usually, binary datas are processed as multiple of two,

Base64 can have paddings for two bits.

It appends `=` char at the end. for each two bits.

## implementation(?)

well, we know how it works.

but..lets just do it with well-implemented library.

```python
# !/usr/bin/python2.7
hex_data = "49276d206b696c6c696e6720796f757220627261696e206c696b65206120706f69736f6e6f7573206d757368726f6f6d"
result = hex_data.decode('hex').encode('base64')
```
