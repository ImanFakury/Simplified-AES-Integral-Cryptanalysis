# Simplified AES (S-AES) & Integral Cryptanalysis

This repository contains a Python implementation of a 3-round **Simplified Advanced Encryption Standard (S-AES)** block cipher and an **Integral (Square) Cryptanalysis** key-recovery attack developed for the Cryptography and Network Security course at Sharif University of Technology.

## Overview

1. **Simplified AES (S-AES) Block Cipher (`encrypt` / `decrypt`):**
   - Operates on 16-bit blocks ($2 \times 2$ state matrix of 4-bit nibbles) with four 16-bit round keys across 3 rounds.
   - Implements finite field multiplication in $GF(2^4) = GF(2)[x]/(x^4 + x + 1)$ (`gf4_mul`).
   - Implements forward and inverse cryptographic primitives: 4-bit Nibble Substitution (`SBOX` / `INV_SBOX`), `ShiftRows`, `MixColumns` (matrix multiplication by $\begin{pmatrix} 1 & 4 \\ 4 & 1 \end{pmatrix}$ and inverse $\begin{pmatrix} 9 & 2 \\ 2 & 9 \end{pmatrix}$ over $GF(2^4)$), and `AddRoundKey`.

2. **3-Round Integral (Square) Attack (`recover` / `run_once`):**
   - Generates chosen-plaintext $\Lambda$-sets of 16 plaintexts where the first nibble is active (iterating through all values $0\text{--}15$) and the remaining three nibbles are held constant (`fixed_tail`).
   - Performs partial decryption (`decrypt2`) of the resulting ciphertexts through the final two rounds for candidate final-round keys $K_3 \in \{0, \dots, 15\}^4$.
   - Exploits the zero-sum (balanced) integral distinguisher property ($\bigoplus_{i=0}^{15} s_i = 0$) across all four nibbles to filter out invalid keys.
   - **Performance:** Across 32 randomized trials, the attack narrows the $2^{16} = 65,536$ keyspace down to an average of **16.91 surviving candidate keys** (~**0.03%** of the total keyspace) using only a single $\Lambda$-set of 16 chosen plaintexts.

## Reference
* M. A. Musa, E. F. Schaefer, and S. Wedig, "A Simplified AES Algorithm and Its Linear and Differential Cryptanalyses," *Cryptologia*, vol. 27, no. 2, pp. 148–177, 2003.
