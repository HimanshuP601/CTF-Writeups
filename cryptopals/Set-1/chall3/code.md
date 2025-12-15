### Single-byte XOR cipher
The hex encoded string:
```bash
1b37373331363f78151b7f2b783431333d78397828372d363c78373e783a393b3736
```
... has been XOR'd against a single character. Find the key, decrypt the message.

You can do this by hand. But don't: write code to do it for you.

How? Devise some method for "scoring" a piece of English plaintext. Character frequency is a good metric. Evaluate each output and choose the one with the best score.

Achievement Unlocked
You now have our permission to make "ETAOIN SHRDLU" jokes on Twitter.

code:
```python
from collections import Counter
from string import ascii_lowercase
import math

# --------------------------------------------------
# Build English frequency model from corpus
# --------------------------------------------------

def get_freq(text: str, letters: str) -> dict:
    counts = Counter()
    text = text.lower()

    for letter in letters:
        counts[letter] = text.count(letter)

    total = sum(counts.values())
    return {letter: counts[letter] / total for letter in letters}


with open("pg84.txt", "r") as f:
    corpus = f.read()

freq = get_freq(corpus, ascii_lowercase)

# --------------------------------------------------
# XOR helper
# --------------------------------------------------

def xor_bytes_const(b: bytes, const: int) -> bytes:
    return bytes([const ^ x for x in b])

# --------------------------------------------------
# Bhattacharyya distance
# --------------------------------------------------

def bhattacharyya_distance(dist1: dict, dist2: dict) -> float:
    bc = 0.0
    for letter in dist1:
        bc += math.sqrt(dist1[letter] * dist2[letter])
    return -math.log(bc)

# --------------------------------------------------
# Scoring function
# --------------------------------------------------

def scoring_fun(word: bytes) -> float:
    curr_freq = {letter: 0 for letter in freq}
    num_letters = 0

    for b in word:
        ch = chr(b).lower()
        if ch in curr_freq:
            curr_freq[ch] += 1
            num_letters += 1

    if num_letters == 0:
        return 0

    curr_freq = {k: v / num_letters for k, v in curr_freq.items()}
    distance = bhattacharyya_distance(freq, curr_freq)

    return 1 / distance

# --------------------------------------------------
# Crack single-byte XOR
# --------------------------------------------------

src = bytes.fromhex(
    "1b37373331363f78151b7f2b783431333d78397828372d363c78373e783a393b3736"
)

best_score = 0
best_plaintext = b""
best_key = None

for key in range(256):
    candidate = xor_bytes_const(src, key)
    score = scoring_fun(candidate)

    if score > best_score:
        best_score = score
        best_plaintext = candidate
        best_key = key

print(best_plaintext)
print("key =", best_key)
print("score =", best_score)
```

