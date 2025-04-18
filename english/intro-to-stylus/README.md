# Introduction to Stylus

Welcome to the world of **Stylus**, Arbitrum's high-performance smart contract framework that allows you to write smart contracts in **Rust, C, or C++** — compiled to **WebAssembly (WASM)** and deployed alongside Solidity contracts on Arbitrum chains.

This guide is built for **absolute beginners** who want to dive into Stylus, write their first contract, and deploy it smoothly.

---

## 🌐 What is Stylus?

Stylus is a smart contract environment built on top of the Arbitrum Nitro stack. Unlike traditional Ethereum contracts written only in Solidity, Stylus allows you to use **Rust** or other WASM-compatible languages to write smart contracts. These contracts are compiled to **WASM**, providing:

- Faster execution
- Lower gas costs
- Interoperability with Solidity contracts
- Access to advanced tooling and libraries from the Rust ecosystem

---

## Requirements

Before starting, make sure you have these installed:

- [Rust](https://www.rust-lang.org/tools/install)
- [Docker](https://www.docker.com/) (for local testing)
- [Node.js](https://nodejs.org/) (Optional - for frontend interaction)
- Wallet like MetaMask with **Arbitrum Sepolia** added

---

## Quickstart – Hello Stylus!

We’ll walk through writing and deploying a basic Stylus contract — a simple "Hello World"-style counter.

---

### Step 1: Clone the Example Repo

```bash
git clone https://github.com/OffchainLabs/stylus-examples.git
cd stylus-examples/hello-world
```

### Step 2: Understand the Code
Open src/lib.rs. Here's the full annotated contract:

```rust
#![no_main] // Required for Stylus to define custom main entry

// Import Stylus SDK utilities
use stylus_sdk::{contract, storage::StorageU32};

// Declare the contract using macro
contract! {
    pub struct Counter {
        value: StorageU32, // Persistent storage for a 32-bit counter
    }

    impl Counter {
        // Function to increment the counter by 1
        pub fn increment(&mut self) {
            let current = self.value.get(); // Read current value
            self.value.set(current + 1);    // Set updated value
        }

        // Function to return current counter value
        pub fn get(&self) -> u32 {
            self.value.get()
        }

        // Reset counter to 0
        pub fn reset(&mut self) {
            self.value.set(0);
        }
    }
}
```

This is a complete, functional smart contract in Rust — it stores a counter, lets you increment, read, and reset it.

### Step 3: Build the Contract

Compile the Rust contract to a .wasm binary compatible with Stylus:

```bash 
# Install the WebAssembly target
rustup target add wasm32-unknown-unknown

# Build the contract in release mode
cargo build --release --target wasm32-unknown-unknown
```

Your compiled contract will be located at:
```bash
target/wasm32-unknown-unknown/release/hello_world.wasm
```

### 🧪 Step 4: Deploy to Arbitrum Sepolia

You can deploy the contract using a browser-based tool provided by Arbitrum Stylus.

1. Open the deployment portal: [https://stylus.arbitrum.io/](https://stylus.arbitrum.io/)
2. Connect your MetaMask wallet (make sure it's set to the **Arbitrum Sepolia Testnet**).
3. Upload your compiled `.wasm` file from:
```bash
target/wasm32-unknown-unknown/release/your_contract_name.wasm
```
4. Click **Deploy** — the platform will automatically handle ABI generation and deployment.
5. Once deployed, you will receive a **contract address** you can use to interact with your smart contract.

> 💡 **Need Sepolia ETH?**
>
> - Use a [Sepolia Faucet](https://sepoliafaucet.com/) to get test ETH.
> - Then bridge it to **Arbitrum Sepolia** using the [Arbitrum Bridge](https://bridge.arbitrum.io/).

## 🔗 Interacting with the Contract

Once your Stylus contract is deployed, you can interact with it using standard Ethereum tools like **Ethers.js**.

Here's a simple example using **JavaScript (Ethers.js)** in a frontend project (e.g., React or plain JS):

```js
import { ethers } from "ethers";

// Set up provider and signer from MetaMask
const provider = new ethers.providers.Web3Provider(window.ethereum);
const signer = provider.getSigner();

// Replace with your deployed Stylus contract address
const contractAddress = "0xYourDeployedContractAddress";

// Define ABI compatible with the exposed Stylus contract functions
const abi = [
  "function increment()",
  "function get() view returns (uint)",
  "function reset()"
];

// Create contract instance
const contract = new ethers.Contract(contractAddress, abi, signer);

// Example: increment the counter
await contract.increment();

// Example: get the current counter value
const currentValue = await contract.get();
console.log("Current Counter Value:", currentValue);

// Example: reset the counter back to 0
await contract.reset();
```

This allows seamless integration of your Stylus smart contract with any web frontend or decentralized application.

---

## 📁 Explore More

This was just the beginning!  
Browse the rest of this repository for more advanced examples, integrations, and tips to level up your Stylus development journey.

Happy hacking 🚀
