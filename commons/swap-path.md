# How to encode a Swap path

A **path** if a sequence of [`tokenIn`, `fee`, `tokenOut`], encoded in bytes.

The `tokenIn` and `tokenOut` addresses must be represented in bytes using its standard format provided in the ICON SDK.

The `fee` must be represented in bytes as a big endian 32-bits integer.

## 🧪 Example

Let's say we want to swap some tokenA to tokenB, then tokenB to tokenC, using a 0.3% fee tier.

- `tokenA` = `cx000000000000000000000000000000000000000a`
- `tokenB` = `cx000000000000000000000000000000000000000b`
- `tokenC` = `cx000000000000000000000000000000000000000c`
- `fee` = `3000`

Converted to bytes:

- `tokenA` = `01000000000000000000000000000000000000000A`
- `tokenB` = `01000000000000000000000000000000000000000B`
- `tokenC` = `01000000000000000000000000000000000000000C`
- `fee` = `00000BB8`

The path format will be: 

> `[tokenA, fee, tokenB, fee, tokenC]`

Hence the final path will be:

`01000000000000000000000000000000000000000A00000BB801000000000000000000000000000000000000000B00000BB801000000000000000000000000000000000000000C`