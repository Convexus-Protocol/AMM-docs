# 📖 Introduction

The Factory should be initialized right after its deployment in order to store the deployed contracts bytes.

# 📜 Write Methods

## `setPoolContract`

- 📚 Description: 
  - Set the pool contract bytes to be newly deployed with [`createPool`](/core-layer/factory/pools-management.md#createpool)
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

## `poolContract`

- 📚 Description:
  - Get the current pool contract bytes of the Factory

### 🖊️ Signature

```java
@External(readonly = true)
public byte[] poolContract ()
```