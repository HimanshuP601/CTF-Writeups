###Convert hex to base64

The string:
```bash
49276d206b696c6c696e6720796f757220627261696e206c696b65206120706f69736f6e6f7573206d757368726f6f6d
```
Should produce:
```bash
SSdtIGtpbGxpbmcgeW91ciBicmFpbiBsaWtlIGEgcG9pc29ub3VzIG11c2hyb29t
```
So go ahead and make that happen. You'll need to use this code for the rest of the exercises.

```bash
┌──(himanshu@Kaaammui)-[~/Desktop/Github/CTF-Writeups/cryptopals/Set-1/chall1]-(15-12-2025 20:09:40)
└─$ ipython3
Python 3.13.9 (main, Oct 15 2025, 14:56:22) [GCC 15.2.0]
Type 'copyright', 'credits' or 'license' for more information
IPython 8.35.0 -- An enhanced Interactive Python. Type '?' for help.

In [1]: hex_text = '49276d206b696c6c696e6720796f757220627261696e206c696b65206120
   ...: 706f69736f6e6f7573206d757368726f6f6d'

In [2]: hex_text = bytes.fromhex(hex_text)

In [3]: import base64

In [4]: base64.b64decode(hex_text)
Out[4]: b'"i"\x96X\xa7\x83*.\xad\xba\xda\x8ayb\x91\xe6\xa9\xa2+(\x9e\x8b\xac\x9a\xeb!\xae\x8a&'

In [5]: base64.b64encode(hex_text).decode()
Out[5]: 'SSdtIGtpbGxpbmcgeW91ciBicmFpbiBsaWtlIGEgcG9pc29ub3VzIG11c2hyb29t'

In [6]: 
```

