# break repeating-key XOR

the problem saids, this one is a tough one.

Well, It doesn't matter!

let's just follow the written HOWTO.

## code

```python
# !/usr/bin/python2.7
import string
import collections

def ham_dist(s1,s2):
    assert len(s1)==len(s2)
    s1 = ' '.join(format(ord(x), '08b') for x in s1)
    s2 = ' '.join(format(ord(x), '08b') for x in s2)
    return sum(c1 != c2 for c1, c2 in zip(s1, s2))

def find_keysize(keyrange, ct)
    l = []
    for size in keyrange:
        l.append([size, ham_dist(ct[size:], ct[:size:size+size]) / float(size)])
    l = sorted(l,key=lambda x:x[1])
    l = map(lambda x:x[0], l)
    return l

def make_group(keysize, ct):
    l = []
    for i in range(keysize):
        l.append([])
    for i in range(len(ct)):
        l[i%keysize].append(ct[i])
    return l

def freq_test(data, top=1): # character list data
    result = []
    for i in data:
        tr = collections.Counter(i).most_common(top)
        result.append(map(lambda x:x[0], tr))
    return result       # return frequent bytes for 

def try_decrypt(data, freq_list, keylen): # list data and top frequent appeared item.
# TODO : implementation. (reuse the previously-coded code!)


if __name__ == "__main__"
# main flow : get sufficient keysize
# -> make the problem small(to single-byte xor) 
# -> do the frequency-test
# -> try decryption and filter the non-printable data.
# -> solve it interactively from now on..

    f = open('6_ct.txt')
    ct = f.read()
    ct = str.replce(ct,'\n','')
    ct = ct.decode('base64')\
    f.close()
    f = open('result6.txt')

    l = find_keysize(range(2,41))
    for i in l[:4]: # four candidates for keylength
        f.write("decrypt attempt with key len %d\r\n"%i)
        group = make_group(i,ct)
        freq_bytes = freq_test(group)
        try_decrypt(ct, freq_bytes, i)
    

```