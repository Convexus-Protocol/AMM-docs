# 📖 Introduction

The `NonFungiblePositionManager` contract manages users positions within a Convexus Pool. Each position mints a NFT owned by the liquidity provider that represents its position.

The liquidity associated with the NFT may be increased or decreased. The NFT owner may uses his NFT to claim rewards from the transaction fees. The NFT may be burned once the liquidity has been decreased to zero and all rewards have been collected.