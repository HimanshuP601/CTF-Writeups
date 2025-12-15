### Detect single-character XOR
One of the 60-character strings in this file has been encrypted by single-character XOR.

Find it.

(Your code from #3 should help.)

code:
```python
import math
from collections import Counter
from string import ascii_lowercase


def get_freq(txt: str, letters: str) -> dict:
    txt = txt.lower()
    count = Counter(txt)
    total = sum(count[c] for c in letters)
    return {letter: count[letter] / total for letter in letters}


with open("pg84.txt") as f:
    data = f.read()


freq = get_freq(data, ascii_lowercase + " ")
print(freq)


def xor_bytes_const(txt: bytes, const: int) -> bytes:
    return bytes([const ^ b for b in txt])


def bhattacharyya_distance(freq1: dict, freq2: dict) -> float:
    bt_cof = 0.0
    for letter in freq1:
        bt_cof += math.sqrt(freq1[letter] * freq2[letter])
    return -math.log(bt_cof)


def string_scoring(word: bytes) -> float:
    cur_freq = {letter: 0 for letter in freq}
    num_letter = 0
    for i in word:
        if chr(i).lower() in cur_freq:
            cur_freq[chr(i).lower()] += 1
            num_letter += 1
    if num_letter != 0:
        cur_freq = {letter: val / num_letter for letter, val in cur_freq.items()}
    else:
        return 0
    distance = bhattacharyya_distance(freq, cur_freq)
    return 1 / distance


with open("chall.txt") as f:
    chunks = set(f.read().split())


def decode_single_byte_xor_cypher(word: str) -> (bytes, int, int):
    src = bytes.fromhex(word)

    max_score = 0
    best_res = b""
    best_key = None
    for i in range(2**8):
        candidate = xor_bytes_const(src, i)
        score = string_scoring(candidate)
        key = i

        if score > max_score:
            max_score = score
            best_res = candidate
            best_key = key
    return best_res, max_score, best_key


if __name__ == "__main__":
    with open("chall.txt") as fh:
        Lines = fh.readlines()

    max_score = 0
    best_word = b""
    best_key = None
    for line in Lines:
        tmp_word, tmp_score, tmp_key = decode_single_byte_xor_cypher(line)

        if tmp_score > max_score:
            max_score = tmp_score
            best_word = tmp_word
            best_key = tmp_key
    print("****************************************************************")
    print(best_word.strip().decode())
    print(f"{max_score}")
    print(f"{best_key}")
    print("****************************************************************")
```

Output:
```bash
{'a': 0.0637471187103264, 'b': 0.011968525709327632, 'c': 0.022106374711871032, 'd': 0.04018192349773668, 'e': 0.10987397411785459, 'f': 0.020790575828869867, 'g': 0.014254487899179295, 'h': 0.04710893718252181, 'i': 0.058584038310724, 'j': 0.0011966142015699007, 'k': 0.004195300786380528, 'l': 0.03032535034337107, 'm': 0.025136049313853787, 'n': 0.058064393099683684, 'o': 0.060197798897303326, 'p': 0.014623960411616215, 'q': 0.0007723167356745972, 'r': 0.049761988191181765, 's': 0.05047471258614073, 't': 0.07241661243763661, 'u': 0.024819018061246624, 'v': 0.009127162904006274, 'w': 0.018242407339869423, 'x': 0.0016137605865793282, 'y': 0.018886004619598253, 'z': 0.0005077267428971889, ' ': 0.1710228667729794}
****************************************************************
Now that the party is jumping
7.943424979642356
53
****************************************************************
```

