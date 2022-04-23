# 📖 Introduction

The Factory gives information about its ownership and the active pools within the Convexus Protocol.

# 📜 Write Methods

## `setOwner`

- 📚 Description: 
  - Updates the owner of the factory
- 🔒 Access: 
  - Factory Owner

### 🖊️ Signature

```java
@External
public void setOwner (
  Address _owner
)
```

- `_owner`: The new owner of the Factory

### 🧪 Example call

```java
{
  "to": ConvexusFactory,
  "method": "setOwner",
  "params": {
    "_owner": "cxcf50d121c8bdbc16437cb569863f51039480a848", // a multisig contract
  },
}
```

## `enableFeeAmount`

- 📚 Description: 
  - Enables a fee amount with the given tickSpacing
  - Fee amounts may never be removed once enabled
- 🔒 Access: 
  - Factory Owner
- 🚩 Requirements:
  - `fee` needs to be lower than `1000000`
  - Tick spacing is capped at `16384` to prevent the situation where tickSpacing is so large that `TickBitmap::nextInitializedTickWithinOneWord` overflows

### 🖊️ Signature

```java
@External
public void enableFeeAmount (
  int fee, 
  int tickSpacing
)
```

- `fee`: The fee amount to enable, denominated in hundredths of a bip (i.e. 1e-6)
- `tickSpacing`: The spacing between ticks to be enforced for all pools created with the given fee amount

### 🧪 Example call

```java
{
  "to": ConvexusFactory,
  "method": "enableFeeAmount",
  "params": {
    "fee": "0xbb8", // 0.3%
    "tickSpacing": "0x36b0", // 14000
  },
}
```

## `setPoolContract`

- 📚 Description: 
  - Set the pool contract bytes to be newly deployed with [`createPool`](create-pool.md#createpool)
- 🔒 Access: 
  - Factory Owner

### 🖊️ Signature

```java
@External
public void setPoolContract (
  byte[] contractBytes
)
```

- `contractBytes`: The contract bytes to be deployed - It should be a JAR file bytes written in Java-SCORE

### 🧪 Example call

```java
{
  "to": ConvexusFactory,
  "method": "setPoolContract",
  "params": {
    "contractBytes": "0xaabbccddee...", // the JAR file bytes
  },
}
```

# 👀 ReadOnly Methods

## `owner`

- 📚 Description:
  - Get the current owner of the Factory

### 🖊️ Signature

```java
@External(readonly = true)
public Address owner()
```

## `poolContract`

- 📚 Description:
  - Get the current pool contract bytes of the Factory

### 🖊️ Signature

```java
@External(readonly = true)
public byte[] poolContract ()
```

## `poolsSize`

- 📚 Description:
  - Get the deployed pools list size

### 🖊️ Signature

```java
@External(readonly = true)
public byte[] poolsSize ()
```

## `pools`

- 📚 Description:
  - Get a deployed pools list item

### 🖊️ Signature

```java
@External(readonly = true)
public Address pools(int index)
```

- `index`: the index of the item to read from the deployed pools list


## `getPool`

- 📚 Description:
  - Get a deployed pool address from its parameters

### 🖊️ Signature

```java
@External(readonly = true)
public Address getPool (
  Address token0, 
  Address token1, 
  int fee
)
```

- `token0`: One of the two tokens in the desired pool
- `token1`: The other of the two tokens in the desired pool
- `fee`: The desired fee for the pool ; divide this value by 10000 to get the percent value


> 📝 Note
> 
> The `token0` and `token1` parameters can be inverted, it will return the same pool address