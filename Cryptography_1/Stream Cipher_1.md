# Stream ciphers

stream cipher는 OTP를 기반으로 함.

## One time PAD (OTP) - Vernam

### Messagespace and Keyspace
```
M = C = {0,1}ⁿ        // n bit len string
K = {0,1}ⁿ
key = (random bit string as long as the message)
```

### Encryption and Decryption

```
c := E(k,m) = k xor m
D(k,c) = k xor c = m
```

암호화 복호화 알고리즘은 다음을 만족해야 함
```
D(k,E(k,m)) = m
```
위의 연산은 결국 다음과 같다
```
(k xor k) xor m = m
```

### 장점

- 빠른 암/복호화 속도

### 단점

- key length = message length
- Message가 길어질수록 key length도 길어짐.

### Is OTP secure?

- 암호가 갖추어야 할 요구사항에 대해 생각해 볼 필요가 있음

so...
## What is a good cipher?

- basic idea : CT (CipherText) should reveal no `"information"` about PT (PlainText)

- Cipher (E,D) : enc/dec algorithm over (`K`,`M`,`C`) has perfect secrecy if

<strong>
∀m0,m1 ∈ M, and (len(m1)=len(m0)) and ∀c ∈ C

Pr[E(k,m0)=c] = Pr[E(k,m1)=c] where k is uniform in K
</strong>

즉, c 만 갖고는 m을 유추 못하게. uniform distribution

OTP has perfect securicy.

그러나, 이 조건은 
```
|K| >= |M|      # key length >= Message length
```

## intro for Stream Cipher

idea : replace `Random` key to `Pseudorandom` key

### PRG (Pseudo Random Generator)

PRG `G` is
```
G : {0,1}^s → {0,1}^n       # n >> s
```
which is efficiently computable by a `deterministic` algorithm

즉, seed size 만큼을 key로 받아 expand함.

그러나! `keylen < messagelen` 이므로, 앞서 정의한 perfect secrecy에는 부합하지 못함.

so...

### PRG must be unpredictable

PRG가 생성한 bit stream을 이용해 이후 bit를 예측할 수 없어야 함.

we say PRG is `predictable` if 
```
∃ "eff" alg. A and ∃ 0 <= i <= n-1 s.t.
Pr[A(G(k))[1..i] = G(k)[i+1]] > ½ + ε

for non-negleble ε (e.g. ε = 1/20^30)
```

in practice...

non-negligible
- ε >= 1/2^30
    - likely to happen over 1GB of data

negligible
- ε <= 1/2^80
    - won't happen over life of key

예측 가능한 PRG들이 있음
- glibc random() 
- never use it for crypto!

## Attack on Stream Cipher

### Two Times Pad

- 하나의 key를 두번 이상 사용하는 것!

```
c1 ← m1 xor PRG(k)
c2 ← m2 xor PRG(k)

then, c1 xor c2 = m1 xor x2
```

ascii data, 혹은 formatted data라면 key를 충분히 알아낼 수 있음

#### examples of Two times pad
- WEB : frame 16M 개의 frame을 주기로 key가 반복됨. 

### No integrity
- not an authenticated-encryption

## Real world Stream Ciphers

### RC4 

expand 128bit seed to 2048 bit keys, and generate 1 byte per round

- old..(since 1987)
- HTTPS와 WEP에 쓰임
- weakness
    - 초기 output이 random 이 아님!
        - Pr[2nd bye = 0] = 2/256
        - non-negligible!
    - (0,0)이 나올 확률이 1/256² + 1/256³
    - related key attacks...

### LFSR (Linear Feedback Shift Register)

![lfsr](/img/LFSR.bmp)

- seed = initial state
- 실제로 저렇게 하나만 사용하진 않고, 여러개를 다른 size로 사용한다던가 함.
- 실제로 쓰인 CSS< GSM, Bluetooth같은 경우에..all broken
    - 짧은 key length를 갖는 경우 : 전수조사로 빠른시간 내에 key get!

### Modern Stream Cipher : eStream
```
PRG : {0,1}^s × R → {0,1}ⁿ
```
`R` is a non-repeating value for a given key

같은 key를 다시 사용하는 경우에도 nonce가 저 pair를 unique 하게 해줌.

#### Salsa 20

- SW + HW에 적합.

```
Salsa20 : {0,1}^(128 or 256) × {0,1}^64 → {0,1}ⁿ
```

첫번째 binary string이 key(nonce)

두번째가 nonce
- 실제 함수의 input nonce와 counter를 이용함

```
Salsa20(k;r) := H(k,(r,0)) || H(k,(r,1)) || ... || H(k,(r,n))
```

`||` means concatination!

stream 암호를 굳이 써야 한다면 eStream 계열을 사용해는 것이 좋다.

