# Tutorials & Examples

Here we will list our tutorials and explore various examples to help you get up and running in the easiest and fastest manner.

## SubQuery Examples



## SubQuery Example Projects

| Example                                                                                       | 説明                                                                                                                       | Topics                                                                                                                        |
| --------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------- |
| [extrinsic-finalized-block](https://github.com/subquery/tutorials-extrinsic-finalised-blocks) | Indexes extrinsics so they can be queried by their hash                                                                  | The simplest example with a **block handler** function                                                                        |
| [block-timestamp](https://github.com/subquery/tutorials-block-timestamp)                      | Indexes timestamp of each finalized block                                                                                | Another simple **call handler** function                                                                                      |
| [validator-threshold](https://github.com/subquery/tutorials-validator-threshold)              | Indexes the least staking amount required for a validator to be elected.                                                 | More complicated **block handler** function that makes **external calls** to the `@polkadot/api` for additional on-chain data |
| [sum-reward](https://github.com/subquery/tutorials-sum-reward)                                | Indexes staking bond, rewards, and slashes from the events of finalized block                                            | More complicated **event handlers** with a **one-to-many** relationship                                                       |
| [entity-relation](https://github.com/subquery/tutorials-entity-relations)                     | Indexes balance transfers between accounts, also indexes utility batchAll to find out the content of the extrinsic calls | **One-to-many** and **many-to-many** relationships and complicated **extrinsic handling**                                     |
| [kitty](https://github.com/subquery/tutorials-kitty-chain)                                    | Indexes birth info of kitties.                                                                                           | Complex **call handlers** and **event handlers**, with data indexed from a **custom chain**                                   |
