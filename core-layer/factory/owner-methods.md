# 📖 Introduction

A Factory is ownable and configurable after its deployment.

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

## `updatePool`

- 📚 Description: 
  - Update an existing pool contract given a pool address
- 🔒 Access: 
  - Factory Owner

### 🖊️ Signature

```java
@External
public void updatePool (
  Address pool
)
```

- `pool`: An existing pool address

### 🧪 Example call

```java
{
  "to": ConvexusFactory,
  "method": "updatePool",
  "params": {
    "pool": "cx4f50d121c8bdbc16437cb569863f51039480a848", // a pool address
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
