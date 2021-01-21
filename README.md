# bitcoin-diff - Difficulty decoding tools


## Installation

```
npm install bitcoin-diff
// or
yarn install bitcoin-diff
```

## Usage

```js
const btcDiff = require('bitcoin-diff')

// Convert difficulty to full target
btcDiff.diffToTarget(diff);

// Convert difficulty to 64-bit MSB of the target
btcDiff.diffToTarget64(diff);

// Convert hash/target to difficulty
btcDiff.hashToDiff(hash);

// Convert nBits to 64-bit MSB of the resulting target
btcDiff.bitsToDiff(bits);

// Convert difficulty to nBits
btcDiff.diffToBits(diff);
```

## Source

- [https://analysis.null.place/how-do-the-bitcoin-mining-algorithms-work/#form7](https://analysis.null.place/how-do-the-bitcoin-mining-algorithms-work/#form7)
- [https://github.com/atomminer/atomminer-core/blob/4df5711d3e9dbe271ba2a00de784b1e1784e0f4d/src/utils/diff.js](https://github.com/atomminer/atomminer-core/blob/4df5711d3e9dbe271ba2a00de784b1e1784e0f4d/src/utils/diff.js)

## What are bits, difficulty, and target?

Bits, difficulty, and target are three different ways of expressing the same thing - namely the amount of work involved in mining. Bits is a compact format used in the block header. It only uses 4 bytes, which saves a lot of space compared to the 32 byte target. Difficulty is the inverse ratio of a given target to the target in the very first Bitcoin block. Difficulty in the Bitcoin source code is a double precision number, which means that it is accurate to 15 significant digits. The Bitcoin source code has functions for converting between bits, target and difficulty.

Bits is a form of floating point notation. The first byte of the bits value is the exponent (called the size during the bits -> target and target -> bits conversions, and shift during the bits -> difficulty conversion) and the final 3 bytes of the bits value are the mantissa (called the word during the bits -> target conversion and compact during the target -> bits conversion). There is no reason for the different naming conventions - the Bitcoin source code is just inconsistent. Setting bit 00800000 in the bits value makes the target negative.

Target is used as the threshold during mining. It is a 32 byte (ie. 256 bit) number.

Difficulty is a human-readable indication of the amount of work involved in mining, relative to the work required to mine the very first Bitcoin block. So for example, a difficulty of 2 means that it will take twice as much work, on average, to mine a block as it took to mine the first block. Difficulty is calculated as:

```
difficulty = first_block_target / current_target
```

The first block target was `00000000ffff0000000000000000000000000000000000000000000000000000`.

As the difficulty increases, the target decreases. This makes sense, since it takes more hashpower to mine a block hash below a lower target. According to the Bitcoin source code, the difficulty can never be negative, so converting a negative bits value to difficulty and back will give a different result to the original value.

With this library you can convert between bits, target and difficulty. 
