# Single-byte XOR cipher

## howto

How to get the plain text without key.

well, single byte xor is much easy than multi-bytes.

We can do a frequency test, if a plain text is a english-readable-message

the following list is top 4 of requently used alphabet

- ['E',' ','T','A','O','I','N']

At the ciphertext, we should find the frequently used byte,

and analyse the whole ciphertext with the basis.

or, exhaustive search for the 8bit key.

## code
frequency test
```python
import collections
ct = '1b37373331363f78151b7f2b783431333d78397828372d363c78373e783a393b3736'.decode('hex')
top = collections.Counter(ct).most_common(1)[0][0]

for cand in ['e',' ','t','a','o','i','n']:
    print "attempts for %c" % cand
    key = ord(top)^ord(cand)
    ts = ""
    for i in ct:
        ts += chr(ord(i)^key)
    print "key %d : %s" % (key, ts)
    ts = ""

# key : 88
```
exhaustive search
```python
ct = '1b37373331363f78151b7f2b783431333d78397828372d363c78373e783a393b3736'.decode('hex')
ts = ""
for i in range(255):
    for c in ct:
        ts += chr(ord(c)^i)
    print "key %d : %s" % (i, ts)
    ts = ""
# find the most plain-text-like string and find the key! - key : 88
```
