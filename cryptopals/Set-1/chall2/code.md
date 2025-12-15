### Fixed XOR
Write a function that takes two equal-length buffers and produces their XOR combination.

If your function works properly, then when you feed it the string:
```bash
1c0111001f010100061a024b53535009181c
```
... after hex decoding, and when XOR'd against:
```bash
686974207468652062756c6c277320657965
```
... should produce:
```bash
746865206b696420646f6e277420706c6179
```

```bash
┌──(himanshu@Kaaammui)-[~/Desktop/Github/CTF-Writeups/cryptopals/Set-1/chall2]-(15-12-2025 20:24:41)
└─$ ipython3                                         
/usr/lib/python3/dist-packages/IPython/core/interactiveshell.py:937: UserWarning: Attempting to work in a virtualenv. If you encounter problems, please install IPython inside the virtualenv.
  warn(
Python 3.13.9 (main, Oct 15 2025, 14:56:22) [GCC 15.2.0]
Type 'copyright', 'credits' or 'license' for more information
IPython 8.35.0 -- An enhanced Interactive Python. Type '?' for help.

In [1]: t1 = bytes.fromhex('686974207468652062756c6c277320657965')

In [2]: t2 = bytes.fromhex('1c0111001f010100061a024b53535009181c')

In [3]: from Crypto.Util.strxor import strxor

In [4]: strxor(t1,t2).hex()
Out[4]: '746865206b696420646f6e277420706c6179'

In [5]: 
```
