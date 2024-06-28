# Arbitrum Blueberry Verifier Node Guide

## Table of Contents

1. [Introduction](#introduction)
2. [Downloading the Client](#downloading-the-client)
3. [Minting the Node Key](#minting-the-node-key)
   - [Getting the Gas Token](#getting-the-gas-token)
   - [Minting the Key](#minting-the-key)
4. [Delegating the Key](#delegating-the-key)
5. [Getting a Gelato Relay API Key](#getting-a-gelato-relay-api-key)
6. [Running the Node Client on a Personal Computer](#running-the-node-client-on-a-personal-computer)
   - [Using the CLI](#using-the-cli)
   - [Using the .env File](#using-the-env-file)
7. [Running the Node Client on a Server](#running-the-node-client-on-a-server)

## Introduction

A Node Client verifies the accuracy of the data posted by the sequencer on the settlement layer to secure the rollup. It serves as an observation node that monitors the rollup protocol. Follow this guide to run a Node Client on Arbitrum Blueberry.

## Downloading the Client

To begin, download the appropriate client based on your operating system:

- `verifier-linux.zip`
- `verifier-macos.zip`
- `verifier-win.zip`

Unzip the downloaded file to get a folder containing the verifier node application:

- `verifier-linux/verifier-node-app-linux`
- `verifier-macos/verifier-node-app-macos`
- `verifier-win/verifier-node-app-win.exe`

## Minting the Node Key

Before running a node, an ERC2771 node key is required.

### Getting the Gas Token

To mint the node key, you need the custom gas token of the chain. Go to the [Arbitrum Blueberry public page](https://raas.gelato.network/rollups/details/public/arb-blueberry) and request the custom gas token from the faucet. After receiving it on Arbitrum Sepolia, bridge it to the Arbitrum Blueberry chain following the instructions on the page

### Minting the Key

Once you have the gas token, mint the node key by calling the mint function on the contract:

- Navigate to [`MockNodeKey.mint`](https://arb-blueberry.gelatoscout.com/address/0xf4D4a4f8B3F4799E7206511F1A2E112cB2329687?tab=write_proxy).
- Fill in the required parameters:
  - `_to`: Address to mint to
  - `_amount`: Number of NFTs to mint

**Info:** Projects can build a frontend page to make this process seamless and customized for users.

## Delegating the Key

After minting your node key, delegate rights to an `Operator` to sign attestation transactions on your behalf. This ensures secure and efficient operation of your node.

- Navigate to [`DelegateRegistry.delegateERC721`](https://arb-blueberry.gelatoscout.com/address/0x00000000000000447e69651d841bD8D104Bed493?tab=write_contract).
- Fill in the required parameters:
  - `to`: Operator address
  - `contract_`: `0xf4D4a4f8B3F4799E7206511F1A2E112cB2329687` (MockNodeKey address)
  - `tokenId`: ID of node key NFT
  - `rights`: `0x0000000000000000000000000000000000000000000000000000000000000000`
  - `enable`: true
  - `Send native CGT`: `0`

**Info:** For production projects, this step will be done on delegate.xyz.

## Getting a Gelato Relay API Key

The Gelato API Key is crucial for running the node as it allows the node to use Gelato Relay for transaction fees. This simplifies the process by enabling the use of various tokens (like USDC) to pay for gas fees.

To obtain the Gelato API Key:

1. Go to [Gelato Relay](https://app.gelato.network/relay).
2. Connect your wallet to create your account.
3. Click "Create App" and complete the basic details.
4. After creating the app, go to the details and copy the API key.

For a detailed tutorial, watch this [video](https://drive.google.com/file/d/1kY7JSdQXyWttXv-emBcAxJpzeP9fTuT2/view?usp=drive_link).

## Running the Node Client on a Personal Computer

Follow these steps to run the verifier node.

### Using the CLI

1. Open the terminal or command prompt.
2. Navigate to the directory containing the verifier node application.
3. Execute the application:
   - For Linux: `./verifier-node-app-linux`
   - For macOS: `./verifier-node-app-macos`
   - For Windows: `verifier-node-app-win.exe`

4. When prompted, enter the required details:
   - Operator private key
   - Gelato Relay API Key
   - Node key IDs

If you follow all the correct steps, you should see a confirmation screen like this one:

![Image](https://github.com/gelatodigital/verifier-node-arb-blueberry/assets/169910523/c2391d8d-cf8e-4fda-a686-e8e6be8d5adf)

### Using the .env File

To avoid entering settings each time you launch the application, you can use a `.env` file. Follow these steps:

1. Create a `.env` file in the folder containing the node application (e.g., `/verifier-macos`).
2. Add the following environment variables to the `.env` file:

   ```env
   OPERATOR_PK=
   RELAY_API_KEY=
   # Set either one
   #------------1-------------
   NODE_KEY_IDS= # [1,2,3]
   #------------2-------------
   AUTO_KEY_IDS=true
   WALLETS=[]
   # [] (Use all fetched node key IDs delegated to operator) or
   # ["0x...","0x..."] (Use fetched node key IDs that are delegated to operator and owned by defined wallets)
   #-------------------------
   # Optionals
   DEBUG= # true/false
   ONCE= # true/false
   INTERVAL= # In ms (Must be > 60000)
   L1_RPC_URL=
   # Your personal RPC URL of settlement chain (Arbitrum Sepolia RPC)
   # Setting `L1_RPC_URL` is highly recommended for better reliability and performance
   ```

For a detailed tutorial, watch this [video](https://drive.google.com/file/d/1kY7JSdQXyWttXv-emBcAxJpzeP9fTuT2/view?usp=drive_link).
## Running the Node Client on a Server

The verifier node is also available as a Docker image. Follow these steps to run it on a server, for example, on GCP:

1. Create a Cloud Run job pulling the `gelatodigital/blueberry-verifier` image.
2. Set up the environment variables.
3. Add a scheduler trigger to execute the container every hour.

---

This guide should help you set up and run the Arbitrum Blueberry Verifier Node. If you have any further questions or issues, please reach out to the team.
