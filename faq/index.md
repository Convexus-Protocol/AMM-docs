<details>
<summary>❓ How to swap a token against another ? </summary>

Please refer to the [`SwapRouter`](/periphery-layer/swaprouter/index.md) documentation. In most cases, you will need to call the [`exactInputSingle`](/periphery-layer/swaprouter/single-swap/exact-input.md#exactinputsingle) method.
</details>


<details>
<summary>❓ How to get the price of a pool ?</summary>

Please refer to the [`slot0`](/core-layer/pool/state.md#slot0) method. The `sqrtPriceX96` field contains the [Q64.96 price](/commons/q6496.md) of the pool. You can decode it to a human readable price like [this](/commons/q6496.md#how-to-decode-a-q64.96-to-a-floating-point-price).
</details>


<details>
<summary>❓ How to get the list of active pools ?</summary>

Please refer to the [`poolsSize`](/core-layer/factory/pools-management.md#poolssize) and [`pools`](/core-layer/factory/pools-management.md#pools) methods. The `poolsSize` method will return the total number of pools deployed. The `pools` method will return a pool address, given an index in the list.
</details>


<details>
<summary>❓ How to provide liquidity to a pool ?</summary>

Please refer to the [`NonFungiblePositionManager`](/periphery-layer/nfpos-manager/index.md) documentation. You will need first to deposit some funds using [`deposit`](/periphery-layer/nfpos-manager/create-position.md#deposit), then create a new position wrapped in a NFT using [`mint`](/periphery-layer/nfpos-manager/create-position.md#mint). The NFT represents the liquidity you provided to the pool.
</details>


<details>
<summary>❓ How to get the list of my positions ?</summary>

Firstly, get the number of positions with the [`balanceOf`](https://docs.openzeppelin.com/contracts/3.x/api/token/erc721#IERC721-balanceOf-address-) method from the `NonFungiblePositionManager` using your address, then call [`tokenOfOwnerByIndex`](https://docs.openzeppelin.com/contracts/3.x/api/token/erc721#IERC721Enumerable-tokenOfOwnerByIndex-address-uint256-), with an index starting from 0 to the value returned by `balanceOf`. You can get the details of the position using the `positions` method.
</details>

<details>
<summary>❓ How to remove my position from a pool ?</summary>

Please refer to the [`NonFungiblePositionManager`](/periphery-layer/nfpos-manager/modify-position.md) documentation. You will need first to decrease the whole position liquidity using [`decreaseliquidity`](/periphery-layer/nfpos-manager/modify-position.md#decreaseliquidity), collect the tokens previously deposited using [`collect`](/periphery-layer/nfpos-manager/collect-rewards.md#collect) then delete the position with [`burn`](/periphery-layer/nfpos-manager/modify-position.md#burn).
</details>


---

Can't find your answer here ? Feel free to ask your question to the [Convexus #developer](https://discord.convexus.net/) Discord channel !