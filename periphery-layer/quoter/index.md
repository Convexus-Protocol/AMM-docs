# 📖 Introduction

The `Quoter` contract works similarly to the `SwapRouter` contract. It exposes methods which allows getting the expected amount out or amount in for a given swap **without executing the swap**.

# **Tokens Swap**

The `Quoter` is able to give a quote for swapping two tokens thanks to the [SwapRouter `exactInput*/exactOuput* methods`](/periphery-layer/swaprouter/index.md). It supports multiple ways of quotations, depending of the user needs:

- [`quoteExactInputSingle`](single-swap/exact-input.md#quoteexactinputsingle): Returns the amount out received for a given exact input but for a swap of a single pool
- [`quoteExactInput`](multihop-swap/exact-input.md#quoteexactinput): Returns the amount out received for a given exact input swap without executing the swap
- [`quoteExactOutputSingle`](single-swap/exact-output.md#quoteexactoutputsingle): Returns the amount in required to receive the given exact output amount but for a swap of a single pool
- [`quoteExactOutput`](multihop-swap/exact-output.md#quoteexactoutput): Returns the amount in required for a given exact output swap without executing the swap

