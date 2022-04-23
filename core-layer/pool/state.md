# 📖 Introduction

Many important information of the current pool state are returned by the [`slot0`](#slot0) method.

# 👀 ReadOnly Methods

## `slot0`

- 📚 Description: 
  - The 0th storage slot in the pool stores many values, and is exposed as a single method to save steps when accessed externally. See the [`Slot0`](#slot0-1) structure definition for more information about the result values.

### 🖊️ Signature

```java
@External(readonly = true)
public Slot0 slot0 ()
```

### 🧪 Example call

```java
{
  "to": ConvexusPool,
  "method": "slot0"
}
```

Result:
```java
{
  "feeProtocol": "0x0",
  "observationCardinality": "0x1",
  "observationCardinalityNext": "0x1",
  "observationIndex": "0x0",
  "sqrtPriceX96": "0x10000a744d2edce243ac72a85",
  "tick": "0x0"
}
```

# ⚙️ Structures

## `Slot0`

```java
class Slot0 {
  BigInteger sqrtPriceX96;
  int tick;
  int observationIndex;
  int observationCardinality;
  int observationCardinalityNext;
  int feeProtocol;
}
```

- `sqrtPriceX96`: The current price of the pool as a sqrt(token1/token0) [Q64.96](/commons/q6496.md) value
- `tick`: The current tick of the pool, i.e. according to the last tick transition that was run. This value may not always be equal to SqrtTickMath.getTickAtSqrtRatio(sqrtPriceX96) if the price is on a tick boundary.
- `observationIndex`: The index of the last oracle observation that was written
- `observationCardinality`: The current maximum number of observations that are being stored
- `observationCardinalityNext`: The next maximum number of observations to store, triggered in observations.write
- `feeProtocol`: The current protocol fee as a percentage of the swap fee taken on withdrawal represented as an integer denominator (1/x)%. Encoded as two 4 bit values, where the protocol fee of token1 is shifted 4 bits and the protocol fee of token0 is the lower 4 bits. Used as the denominator of a fraction of the swap fee, e.g. 4 means 1/4th of the swap fee.
