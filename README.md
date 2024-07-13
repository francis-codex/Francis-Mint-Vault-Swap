# 

## Project intro

This project automates the creation and management of Metaplex Core Collections and assets
It accomplishes this through three key functionalities:

1. Minting: The project utilizes dedicated programs to mint both collections and individual assets within the Metaplex Core standard.
2. Vaulting: Minted NFTs are then secured within a vault using the "mint-vault" program. This program likely offers features like access control or timed release for the vaulted assets.
3. Swapping: Finally, the project integrates a "swap" program that allows users to exchange their vaulted Collections for SOL (Solana's native token).

In essence, this project provides a streamlined workflow for creating Metaplex Core Core Collections and assets, safeguarding them in a vault, and enabling users to seamlessly convert them into SOL.

The charges for the mint-swap includes:

1. Locking Fee: Secure your assets in our vault for a one-time fee of 1 SOL.
2. Swap Fee: Easily exchange assets within the vault for a flat fee of 2 SOL.


## Technical

When developing change your solana dev environment to match the following

```bash
solana-install init 1.18.8
# solana-cli 1.18.8 (src:e2d34d37; feat:3469865029, client:SolanaLabs)

avm use 0.29.0
# anchor-cli 0.29.0
```

Build Project

1. Install required yarn dependencies

```bash
yarn install
```

2. Build the project to generate types, idl and deployable sbf program

```bash
anchor build
```

3. The test are split into two

-   the [mint-vault tests](./tests/mint-vault.ts) which mint the asset and lock the asset in the program vault
-   the [swap tests](./tests/swap.ts) for swapping the assets to SOL (Solana's native token)

Solana limits requests to its network (RPC calls) to prevent overload. Because of these limits, the tests must be run individually, one at a time, to avoid exceeding the rate and causing errors.

Make sure to Update the `uploadAssetFiles` method in [utils.ts](./tests/utils.ts) path with your file path for the image

Call the available instructions one by one update the variables appropriately with the required values for the public keys. These might be `collection` `asset` or `previous owner` 

Available instructions are

1. MintVault program

-   `init` - **only call this if you deploy a new program**. Starts the program by setting up its communication rules (protocol state) and accounts for managing digital resources (asset manager accounts).
-   `create_collection` - creates a collection for the assets that will be minted
-   `mint_asset` - mints asset from previously created collection
-   `lock_in_vault` - locks an asset in the program vault. Protocol takes a flat fee of 1 SOl for locking the asset
-   `purchase` - Unlock and acquire an asset from a program vault for a one-time fee of 2 SOL.

2. Swap program

-   swap. The user initiates a swap by transferring their CPI tokens into the mint-vault program. This program then calls the purchase IX function to exchange a locked asset within the vault for SOL.

### Transactions Breakdown

-   [init](https://explorer.solana.com/tx/cGZWoTWW2iihzAEBG2g2m3gdbkHU1NEidva2gsnJ79DtHjq4P8ND9J4etzQJeu7ptvGd4HxkZaBPHN19QjVuwjC?cluster=devnet)

-   [collection create](https://explorer.solana.com/tx/2wgL1f5k2YtBjDxYbJdFwceZKt18BUNK42XjWP1u3rWiLHT78yfvRMeQEGogozwJnSx2B8MNrUeJUJk9zPrmChQW?cluster=devnet)

    [collection created - EVhj14d1vKAP8ZAdgbkvCYqpcxtVAFYY2Z17sJhbM3k2 ](https://mpl-core-ui.vercel.app/explorer/collection/EVhj14d1vKAP8ZAdgbkvCYqpcxtVAFYY2Z17sJhbM3k2?env=devnet)

-   [asset mint](https://explorer.solana.com/tx/4G3PukKuFZQZim5dgDcGmvqXxWSLwyEqV9f9JnXqKerDsAX6y6C2mRLW2WX4rQ7PjptwX6qGCiXPJ58TRHX541jD?cluster=devnet)

    [asset minted - CBMg87CRTWA1qenc7inasnnAQDLL5Tu5n8P7geFkiSkj](https://mpl-core-ui.vercel.app/explorer/CBMg87CRTWA1qenc7inasnnAQDLL5Tu5n8P7geFkiSkj?env=devnet)

-   [vault lock](https://explorer.solana.com/tx/p4jBwenbZXP7e16FC1q5MTgEqfWv2fAbKDCCq4mxdbfh1y6xQu8vymxe1LyTo9pWAuLWV2wk7M8LrabcVX4Rtam?cluster=devnet)

-   [swap](https://explorer.solana.com/tx/57F2V6mFSRMvMDzWTtomPK3XzgoX5TSnpywqnFVp88e4fRnrsCeTWupBbeweFEtJUbwfw9RfyxgoFLfBb3pPQuXN?cluster=devnet)
