# Nouns DAO contest details

- 10,000 USDC main award pot
- Join [Sherlock Discord](https://discord.gg/MABEWyASkp)
- Submit findings using the issue page in your private contest repo (label issues as med or high)
- [Read for more details](https://docs.sherlock.xyz/audits/watsons)
- Starts December 1, 2022 15:00 UTC
- Ends December 4, 2022 15:00 UTC

# Resources

- [Website](https://nouns.wtf/)
- [Docs](https://nouns.center/)
- [Twitter](https://twitter.com/nounsdao)

# On-chain context

Streamer hasn't been deployed to mainnet yet.
While these contracts support any ERC20 contract, Nouns DAO will primarily use USDC.

```
DEPLOYMENT: mainnet
ERC20: USDC and WETH
ERC721: none
ERC777: none
FEE-ON-TRANSFER: none
REBASING TOKENS: none
ADMIN: n/a
```


## Goerli deployment

| Contract      | Address                                                                                                                      |
| ------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| StreamFactory | [0x51EdD88CD84e93e99d49122144Efc0719D6eE450](https://goerli.etherscan.io/address/0x51EdD88CD84e93e99d49122144Efc0719D6eE450) |
| Stream        | [0x033250f82FAbc5dD0005aB314e3c0124623d9D58](https://goerli.etherscan.io/address/0x033250f82FAbc5dD0005aB314e3c0124623d9D58) |

# Audit scope

- `StreamFactory.sol`
- `Stream.sol`
- `IStream.sol`
- `solady/utils/LibClone.sol`
- `solady/utils/Clone.sol`

# About Nouns DAO

[Nouns](https://nouns.wtf/) is a generative NFT project on Ethereum, where a new Noun is minted and auctioned off every day, all proceeds go to the DAO treasury, and each token represents one vote. To date more than 520 Nouns have been sold on auction, and the DAO has funded a variety of cool builders, coders, artists and more. More intro information can be found on [Nouns Center](https://nouns.center/).

# About Streamer

Streamer is a new component in Nouns' infrastructure for proposal payments. The DAO's treasury is primarily in ETH, while the DAO and its builders often want to configure payments in the form of USDC streams. [Token Buyer](https://github.com/nounsDAO/token-buyer/) is another Nouns project recently launched that helps the DAO convert ETH to USDC trustlessly on chain; Token Buyer alone only supports lump sum payments. Streamer + Token Buyer complete the streaming solution for Nouns.

Streamer solves the problem of allowing payers of token streams to create streams and fund them after the fact,
i.e. first create the stream, then fund it later when funds are available.

## Creating a DAO proposal with Streamer

First the user (in our case the DAO's web UI) calls `predictStreamAddress` to get the future stream contract address.
Then a proposal can be composed with the following two transactions:

1. `StreamFactory.createStream(...)`, where the new stream's address should match `predictStreamAddress`
2. `Payer.sendOrRegisterDebt(...)`, where the payment recipient is the address from `predictStreamAddress`

Once executed, the Token Buyer and Payer contracts work together to fund the new stream asynchronously, and once funded the stream's recipient can withdraw their streamed funds.

## How to run tests

To run smart contract tests, make sure you have [Foundry](https://book.getfoundry.sh/) installed, then run:

```sh
forge install
forge test
```
