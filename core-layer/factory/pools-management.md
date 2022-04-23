# 📖 Introduction

The Factory is the component responsible of creating new Pools withing the Convexus Protocol. Pools deployed directly by users won't be able to participate in the Convexus Protocol.

# 📜 Write Methods

## `createPool`

- 📚 Description: 
  - Creates a pool for the given two tokens and fee
  - The newly deployed pool will be automatically added to the global [`pools`](settings.md#pools) list.
- 🔒 Access: 
  - Everyone

### 🖊️ Signature

```java
@External
public Address createPool (
  Address tokenA,
  Address tokenB,
  int fee
)
```

- `tokenA`: One of the two tokens in the desired pool
- `tokenB`: The other of the two tokens in the desired pool
- `fee`: The desired fee for the pool ; divide this value by 10000 to get the percent value

> 📝 Note
> 
> `tokenA` and `tokenB` may be passed in either order: `token0`/`token1` or `token1`/`token0`. `tickSpacing` is retrieved from the fee. The call will revert if the pool already exists, the fee is invalid, or the token arguments are invalid.


### 🧪 Example call

```java
{
  "to": ConvexusFactory,
  "method": "createPool",
  "params": {
    "tokenA": "cxcf50d121c8bdbc16437cb569863f51039480a848",
    "tokenB": "cx1eb5386b85e310dd3f0db647a7e85a5b6a015642",
    "fee": "0xbb8" // 3000 = 0.3%
  },
}
```

# 👀 ReadOnly Methods

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