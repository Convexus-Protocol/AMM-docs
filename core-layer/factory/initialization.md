# ๐ Introduction

The Factory should be initialized right after its deployment in order to store the deployed contracts bytes.

# ๐ Write Methods

## `setPoolContract`

- ๐ Description: 
  - Set the pool contract bytes to be newly deployed with [`createPool`](/core-layer/factory/pools-management.md#createpool)
- ๐ Access: 
  - Factory Owner

### ๐๏ธ Signature

```java
@External
public void setPoolContract (
  byte[] contractBytes
)
```

- `contractBytes`: The contract bytes to be deployed - It should be a JAR file bytes written in Java-SCORE

### ๐งช Example call

```java
{
  "to": ConvexusFactory,
  "method": "setPoolContract",
  "params": {
    "contractBytes": "0xaabbccddee...", // the JAR file bytes
  },
}
```

# ๐ ReadOnly Methods

## `poolContract`

- ๐ Description:
  - Get the current pool contract bytes of the Factory

### ๐๏ธ Signature

```java
@External(readonly = true)
public byte[] poolContract ()
```