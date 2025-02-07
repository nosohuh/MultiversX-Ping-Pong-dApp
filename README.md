<div align="center">
  <img src="https://github.com/user-attachments/assets/c1220ec9-672b-4b07-8361-f35f12176ea6" alt="MultiversX Image" width="50%" />
  
  ## MultiversX Blockchain dApp

  This dApp demonstrates a simple token lock and claim mechanism on the MultiversX blockchain.

  Users can deposit tokens ("ping") for a set period, after which they can reclaim them ("pong").
  
![dapp-problem-2fb6fd7db44fbfc6fcdf3e398fa8fd8e](https://github.com/user-attachments/assets/c50d87e8-6fcd-4747-be77-12b8ca507203)
  
</div>

<br>
**official document** : https://docs.multiversx.com/developers/tutorials/your-first-dapp/#create-wallet

## 🔧 Prerequisites
- Rust ≥ 1.78.0
- Node.js ≥ 20
- yarn
- multiversx-sc-meta

  
You will use `sc-meta` to:

1. **Create a wallet** to handle your transactions.
2. **Build and deploy a contract** to the blockchain.


## 📁 Project Structure
![folder-structure-44588320bbf503cc91a989bb9cea0ccf](https://github.com/user-attachments/assets/3c10d4ff-0eb8-424c-8f2b-39c15d62a9a7)

## Create Wallet

To deploy a smart contract to the blockchain, you will need a wallet. A PEM file is recommended for simplicity and ease of testing.

Make sure you are in the `ping-pong` folder and then run the following commands:

```bash
mkdir -p wallet
sc-meta wallet new --format pem --outfile ./wallet/wallet-owner.pem
```

2. Build & Deploy Smart Contract    

```bash
git clone https://github.com/multiversx/mx-ping-pong-sc contract
cd contract/ping-pong
sc-meta all build
```
## Deploy the Smart Contract

Next, let's deploy the smart contract to the blockchain.

Ensure that the `wallet-owner.pem` file is placed in the `wallet/` folder and that the smart contract has been built.

Before deploying, you will need to modify the wallet from which transactions are made. By default, transactions are made from a test wallet. To use the wallet you created earlier, follow the steps below:

1. Navigate to the following directory: `/ping-pong/contract/ping-pong/interactor/src`.

2. In the `interact.rs` file located at `/ping-pong/contract/ping-pong/interactor/src`, modify the `alice_wallet_address` variable in the `new` function as follows:

   - **Before:**

     ```rust
     let alice_wallet_address = interactor.register_wallet(test_wallets::alice()).await;
     ```

   - **After:**

     ```rust
     let alice_wallet_address = interactor
         .register_wallet(Wallet::from_pem_file("/ping-pong/wallet/wallet-owner.pem").unwrap())
         .await;
     ```
```rust
cargo run deploy --ping-amount 1000000000000000000 --duration-in-seconds 180

deploy::output:
   Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.48s
     Running `/ping-pong/contract/target/debug/ping-pong-interact deploy --ping-amount 1000000000000000000 --duration-in-seconds 180`
sender's recalled nonce: 12422
-- tx nonce: 12422
sc deploy tx hash: b6ca6c8e6ac54ed168bcd6929e762610e2360674f562115107cf3702b8a22467
deploy address: erd1qqqqqqqqqqqqqpgqymj43x6anzr38jfz7kw3td2ew33v9jtrd8sse5zzk6
new address: erd1qqqqqqqqqqqqqpgqymj43x6anzr38jfz7kw3td2ew33v9jtrd8sse5zzk6
 ```
**!!!!If it doesn't work, ensure the path to the "wallet-owner.pem" file in the `alice_wallet_address` variable in the `interact.rs` file is correct!!**


3. Set Up Frontend

```bash
cd dapp
npm install --global yarn
yarn add vite --dev
```
Update the smart contract address in src/config/config.devnet.ts

4. Run Development Server

```bash
yarn start:devnet
```
## 🔍 Smart Contract Features
### Main Functions

- ping() - Deposit tokens
- pong() - Withdraw tokens after lock period

### View Functions

- didUserPing - Check if user has made a deposit
- getPongEnableTimestamp - Get unlock timestamp
- getTimeToPong - Get remaining lock time
- getAcceptedPaymentToken - Get accepted token type
- getPingAmount - Get required deposit amount
- getDurationTimestamp - Get lock duration
- getUserPingTimestamp - Get user's deposit timestamp
## 🎮 How to Use
- Connect your wallet using Web Wallet, xPortal, or other supported methods
- Click "Ping" to deposit tokens
- Wait for the lock period to expire
- Click "Pong" to withdraw your tokens
## ⚠️ Important Notes
- Test thoroughly on devnet before mainnet deployment
- PEM wallets should only be used for testing
- Ensure proper wallet funding through the MultiversX Devnet Faucet

<img width="1440" alt="success_tx-9ed679c409e2fc0fc35721e10afa6fb2" src="https://github.com/user-attachments/assets/192b525e-c859-455c-a987-ebaf9774e521" />

