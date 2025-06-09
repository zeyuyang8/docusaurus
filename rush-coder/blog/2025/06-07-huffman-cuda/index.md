---
slug: cuda-implementation-of-huffman-coding
title: CUDA Implementation of Huffman Coding
authors: [zeyu]
tags: [cuda, compression]
---

Guide to implement Huffman coding using CUDA.

<!-- truncate -->

import PasswordProtection from '../../../src/components/PasswordProtection';

<PasswordProtection password="letsrush">

## Huffman Coding

Huffman coding is a lossless data compression algorithm that assigns shorter codes to more frequent symbols in a dataset. Theoretically, the average code length is equal to the entropy of the dataset.

The algorithm is based on the following steps:

1. Count the frequency of each symbol in the dataset.
2. Build a Huffman tree from the frequency of each symbol.
3. Assign codes to each symbol in the Huffman tree.

There is a Python [package](https://github.com/soxofaan/dahuffman) called `dahuffman` that implements the Huffman coding algorithm.

## Algorithm

### Encoding

<!-- KEY: Compute the Huffman tree of the data -->

**Computing the Huffman tree**

The average code length after Huffman coding is equal to the entropy of the data. Below is an example of the Huffman tree of the data. The codebook is printed as a table. The entropy of the data is approximately 2.65 bits per symbol.

```bash
Bits Code          Value Symbol
   3 000               0 10
   4 0010              2 9
   5 00110             6 8
   7 0011100          28 6
  13 0011101000000  1856 _EOF
  13 0011101000001  1857 0
  12 001110100001    929 1
  11 00111010001     465 2
  10 0011101001      233 3
   9 001110101       117 4
   8 00111011         59 5
   7 0011110          30 15
   7 0011111          31 7
   2 01                1 12
   3 100               4 14
   3 101               5 11
   2 11                3 13
```

**Representing Huffman trees in lookup tables**

The reason why we need to represent the Huffman tree in lookup tables is for fast decoding.

The objective is to have a lookup table that stores the codebook mapping codes to symbols.

```python

prefixes = ['']

for symbol, (bits, decimal_val) in table.items():
    if not isinstance(symbol, int):
        continue

    # `rjust` is used to pad the specified character to the left of the string
    # to make the string the specified number of bits long
    prefix = bin(decimal_val)[2:].rjust(bits, '0')
    print(f'binary: {bin(decimal_val)} decimal: {decimal_val} prefix: {prefix} bits: {bits}')
```

_Q: Why choose bytes as the unit?_

_A:_ The choice of byte (8 bits) as the unit of processing is due to the hardware efficiency and the ease of parallelization.

- Byte-aligned operations are hardware-efficient as modern computers are optimized for byte-level processing.
- Byte-based operations are easier to parallelize on GPUs, which is crucial for the performance of this compression system.

### Decoding

## CUDA Kernel

## Profiling and Optimization

</PasswordProtection>
