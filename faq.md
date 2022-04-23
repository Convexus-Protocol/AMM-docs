<details>
<summary>❓ How to swap a token against another ? </summary>

Please refer to the [`SwapRouter`](/Convexus-Periphery/Contracts/SwapRouter/docs/README.md) documentation. In most cases, you will need to call the [`exactInputSingle`](/Convexus-Periphery/Contracts/SwapRouter/docs/README.md#swaprouterexactinputsingle) method.
</details>


<details>
<summary>❓ How to get the price of a pool ?</summary>

Please refer to the [`Slot0`](/Convexus-Core/Contracts/Pool/docs/README.md#convexuspoolslot0) method and the structure definition. The `sqrtPriceX96` field contains the [Q64.96 price](/commons/q6496.md) of the pool. You can decode it to a human readable price like [this](/commons/q6496.md#how-to-decode-a-q6496-to-a-floating-point-price).
</details>


<details>
<summary>❓ How to get the list of active pools ?</summary>

Please refer to the [`poolsSize`](/Convexus-Core/Contracts/Factory/docs/README.md#convexusfactorypoolssize) and [`pools`](/Convexus-Core/Contracts/Factory/docs/README.md#convexusfactorypools) methods. The `poolsSize` method will return the total number of pools deployed. The `pools` method will return a pool address, given an index in the list.
</details>


<details>
<summary>❓ How to provide liquidity to a pool ?</summary>

Please refer to the [`NonFungiblePositionManager`](/Convexus-Periphery/Contracts/NonFungiblePositionManager/docs/README.md) documentation. You will need first to deposit some funds using [`deposit`](/Convexus-Periphery/Contracts/NonFungiblePositionManager/docs/README.md#nonfungiblepositionmanagerdeposit), then create a new position wrapped in a NFT using [`mint`](/Convexus-Periphery/Contracts/NonFungiblePositionManager/docs/README.md#nonfungiblepositionmanagermint). The NFT represents the liquidity you provided to the pool.
</details>


<details>
<summary>❓ How to get the list of my positions ?</summary>

Firstly, get the number of positions with the [`balanceOf`](https://docs.openzeppelin.com/contracts/3.x/api/token/erc721#IERC721-balanceOf-address-) method from the `NonFungiblePositionManager` using your address, then call [`tokenOfOwnerByIndex`](https://docs.openzeppelin.com/contracts/3.x/api/token/erc721#IERC721Enumerable-tokenOfOwnerByIndex-address-uint256-), with an index starting from 0 to the value returned by `balanceOf`. You can get the details of the position using the `positions` method.
</details>

