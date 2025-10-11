# 🚩 Foundation Challenge: 🪙 ERC20 Token Interactor

🪙 Create a comprehensive ERC20 token management system:

👷‍♀️ You'll compile and deploy your first Stylus-based ERC20 smart contract written in Rust. Then, you'll use a modern React app with essential components and hooks to interact with your token. Finally, you'll deploy your ERC20 contract to Arbitrum Sepolia and build a beautiful frontend for token management! 🚀

🌟 The final deliverable is a full-stack application that lets users mint, transfer, approve, and burn ERC20 tokens. Deploy your contracts to Arbitrum Sepolia testnet, then build and upload your app to a public web server.

## Checkpoint 0: 📦 Prerequisites 📚

Before starting, ensure you have the following installed:

- [Node.js (>= v18.17)](https://nodejs.org/en/download/)
- [Yarn](https://classic.yarnpkg.com/en/docs/install/)
- [Git](https://git-scm.com/downloads)
- [WSL (for Windows users)](https://www.geeksforgeeks.org/how-to-install-wsl2-windows-subsystem-for-linux-2-on-windows-10/)
- [Curl](https://gcore.com/learning/how-to-install-curl-on-ubuntu)
- [Rust](https://rustup.rs/) (including `rustc`, `rustup`, and `cargo`) - Install with (⚠️ **Must use WSL terminal to run these commands**):

  ```bash
  curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
  ```

  ```bash
  source ~/.bashrc  # or restart your terminal
  ```

- cargo-stylus  
  -> Install cargo-stylus with the below command:

  ```bash
  cargo install cargo-stylus
  ```

  > ⚠️ **Note for Ubuntu users**: If you face issues related to pkg-config while trying to install cargo-stylus, run these commands:

  ```bash
  sudo apt update
  sudo apt install pkg-config
  sudo apt install libssl-dev
  sudo apt install build-essential
  ```

- [Foundry](https://getfoundry.sh/introduction/installation/) - Required for smart contract development

### Foundry Installation Steps:

#### 1. Open your WSL terminal.

#### 2. Install Foundry using the official install script:

```bash
curl -L https://foundry.paradigm.xyz | bash
```

#### 3. Add Foundry to your shell profile

After installation, you'll see instructions to add Foundry to your shell profile (like .bashrc or .zshrc). Usually, you can do:

```bash
export PATH="$HOME/.foundry/bin:$PATH"
```

Add the above line to your ~/.bashrc or ~/.zshrc file, then reload your shell:

```bash
source ~/.bashrc
```

```bash
source ~/.zshrc
```

#### 4. Install Foundry binaries

```bash
foundryup
```

---

### 🔧 Version Requirements

Ensure your tools are ready to use:

#### Check your versions - ⚠️ **Must use WSL terminal to run these commands**

```bash
cargo stylus --version
```

```bash
cargo --version
```

```bash
rustup --version
```

```bash
rustc --version
```

```bash
curl --version
```

```bash
cast --version
```

```bash
forge --version
```

### 🚩 Challenge Setup Instructions

#### For Ubuntu/Mac Users:

1. Open your terminal.
2. Clone the repository:

   ```bash
   git clone <your-repo-url>
   ```

   ```bash
   cd Stylus-ERC20-App
   ```

   ```bash
   yarn install
   ```

#### For Windows Users (Using WSL):

1. Open your WSL terminal.
2. Ensure you have set your Git username and email globally:

   ```bash
   git config --global user.name "Your Name"
   ```

   ```bash
   git config --global user.email "your.email@example.com"
   ```

3. Clone the repository:

   ```bash
   git clone <your-repo-url>
   ```

   ```bash
   cd Stylus-ERC20-App
   ```

   ```bash
   yarn install
   ```

### 🛠️ Troubleshooting Common Issues

#### 1. `stylus` Not Recognized

If you encounter an error stating that `stylus` is not recognized as an external or internal command, run the following command in your terminal:

```bash
sudo apt-get update && sudo apt-get install -y pkg-config libssl-dev
```

After that, check if `stylus` is installed by running:

```bash
cargo stylus --version
```

If the version is displayed, `stylus` has been successfully installed and the path is correctly set.

#### 2. 🚨 Fixing Line Endings and Running Shell Scripts in WSL

> ⚠️ This guide provides step-by-step instructions to resolve the Command not found error caused by CRLF line endings in shell scripts when running in a WSL environment.

Shell scripts created in Windows often have `CRLF` line endings, which cause issues in Unix-like environments such as WSL. To fix this:

**Using `dos2unix`:**

1. Install `dos2unix` (if not already installed):

   ```bash
   sudo apt install dos2unix
   ```

2. Convert the script's line endings:

   ```bash
   dos2unix run-sepolia-deploy.sh
   ```

3. Make the Script Executable:

   ```bash
   chmod +x run-sepolia-deploy.sh
   ```

4. Run the Script in WSL:
   ```bash
   bash run-sepolia-deploy.sh
   ```

---

## 🚀 Submitting Your Challenge

After you have completed the setup and are ready to submit your solution, follow these steps:

1. **Create a New GitHub Repository**

   - Go to [GitHub](https://github.com/) and create a new repository (public or private as required by the challenge).

2. **Set Your Local Repository's Remote URL**

   - In your project directory, update the remote URL to your new repository:
     ```bash
     git remote set-url origin https://github.com/yourusername/your-repo.git
     ```

3. **Push Your Code to GitHub**

   - Add and commit any changes if you haven't already:
     ```bash
     git add .
     git commit -m "Initial commit for challenge submission"
     ```
   - Push your code:
     ```bash
     git push -u origin main
     ```

4. **Submit Your Challenge**
   - Copy your repository link in the following format (without `.git` at the end):
     ```
     https://github.com/yourusername/your-repo
     ```
   - Use this link to submit your challenge as instructed.

---

## 💫 Checkpoint 1: Environment Setup & Contract Deployment

### 1. Configure Environment Variables

> ⚠️ **IMPORTANT**: You need to create `.env` files in **TWO different locations** with different configurations!

#### 📁 Location 1: For Contract Deployment (`packages/erc20/.env`)
Create a `.env` file in the `packages/erc20/` directory:

```bash
# Required: Your wallet's private key (without 0x prefix)
PRIVATE_KEY=your_private_key_here
```

#### 📁 Location 2: For Frontend (`packages/nextjs/.env`)
Create a `.env` file in the `packages/nextjs/` directory:

```bash
# Required: Arbitrum Sepolia RPC URL
NEXT_PUBLIC_RPC_URL=https://sepolia-rollup.arbitrum.io/rpc

# Required: Your wallet's private key for contract interactions
NEXT_PUBLIC_PRIVATE_KEY=your_private_key_here
```

> 💡 **Note**: You can use the same private key in both files, but make sure to add it to **BOTH locations** before running any scripts!

#### 🔧 Step-by-Step Environment Setup:

1. **Create the first `.env` file for contract deployment:**
   ```bash
   # Navigate to erc20 directory
   cd packages/erc20
   
   # Create .env file
   touch .env
   
   # Add your private key (without 0x prefix)
   echo "PRIVATE_KEY=your_actual_private_key_here" > .env
   ```

2. **Create the second `.env` file for frontend:**
   ```bash
   # Navigate to nextjs directory
   cd packages/nextjs
   
   # Create .env file
   touch .env
   
   # Add both required variables
   echo "NEXT_PUBLIC_RPC_URL=https://sepolia-rollup.arbitrum.io/rpc" > .env
   echo "NEXT_PUBLIC_PRIVATE_KEY=your_actual_private_key_here" >> .env
   ```

> 🚨 **Critical**: Both `.env` files must be created **BEFORE** running the deployment script!

#### ✅ Verify Your Environment Setup:

Before proceeding, verify that both `.env` files exist and contain the correct content:

```bash
# Check erc20 .env file
echo "Checking packages/erc20/.env:"
cat packages/erc20/.env

# Check nextjs .env file  
echo "Checking packages/nextjs/.env:"
cat packages/nextjs/.env
```

You should see output similar to:
```
Checking packages/erc20/.env:
PRIVATE_KEY=your_actual_private_key_here

Checking packages/nextjs/.env:
NEXT_PUBLIC_RPC_URL=https://sepolia-rollup.arbitrum.io/rpc
NEXT_PUBLIC_PRIVATE_KEY=your_actual_private_key_here
```

> ⚠️ **If either file is missing or empty, create them now before continuing!**

### 2. Get Arbitrum Sepolia ETH

You'll need test ETH on Arbitrum Sepolia to deploy contracts and pay gas fees:

1. **Bridge from Ethereum Sepolia**: Use the [Arbitrum Bridge](https://bridge.arbitrum.io/)
2. **Faucets**: Try [Arbitrum Sepolia Faucet](https://faucet.quicknode.com/arbitrum/sepolia)

### 3. Deploy the Smart Contract

```bash
# Navigate to the contract directory
cd packages/erc20

# Make the deployment script executable
chmod +x run-sepolia-deploy.sh

# Deploy to Arbitrum Sepolia
./run-sepolia-deploy.sh
```

**Expected Output:**
```
Checking connection to Arbitrum Sepolia...
Connected to Arbitrum Sepolia!
Deploying the Stylus contract using cargo stylus...
Stylus contract deployed successfully!
Transaction hash: 0x...
Contract address: 0x...
Deployment completed successfully on Arbitrum Sepolia!
```

### 4. Update Contract Address

After deployment, update the contract address in `packages/nextjs/app/debug/_components/DebugContracts.tsx`:

```typescript
const contractAddress = "0x..."; // Replace with your deployed contract address
```

---

## Checkpoint 2: 🎨 Frontend Magic

### Start the Frontend Application

```bash
# Navigate to the frontend directory
cd packages/nextjs

# Start the development server
yarn dev
```

Visit `http://localhost:3000` to access the application.

> ⛽ You'll be redirected to the homepage with a beautiful interface

The interface allows you to:

1. **Mint Tokens**: Create new tokens for yourself or specific addresses
2. **Transfer Tokens**: Send tokens between addresses  
3. **Approve Tokens**: Allow other addresses to spend your tokens
4. **Burn Tokens**: Permanently destroy tokens from circulation
5. **Track Transactions**: View all operations in the Block Explorer

> Navigate to the "Debug Contracts" page to start interacting with your ERC20 token

> After performing transactions, you can view all your transaction history in the "Block Explorer" tab

💼 Take a quick look at your deploy script `run-sepolia-deploy.sh` in `packages/erc20/run-sepolia-deploy.sh`.

📝 If you want to edit the frontend, navigate to `packages/nextjs/app` and open the specific page you want to modify. For instance: `/debug/page.tsx`. For guidance on [routing](https://nextjs.org/docs/app/building-your-application/routing/defining-routes) and configuring [pages/layouts](https://nextjs.org/docs/app/building-your-application/routing/pages-and-layouts) checkout the Next.js documentation.

---

## Checkpoint 3: 💾 Deploy your contract! 🛰

🛰 Your ERC20 contract is deployed to Arbitrum Sepolia using the `run-sepolia-deploy.sh` script

The deployment script will:
1. **Check Connection**: Verify connectivity to Arbitrum Sepolia
2. **Deploy Contract**: Use cargo stylus to deploy your Rust-based ERC20 contract
3. **Extract Details**: Capture transaction hash and contract address
4. **Confirm Success**: Display deployment confirmation

> This deployment uses your provided private key to deploy the contract to Arbitrum Sepolia testnet.

## Checkpoint 4: 🚢 Ship your frontend! 🚁

🚀 Deploy your NextJS App

```bash
yarn vercel
```

> Follow the steps to deploy to Vercel. Once you log in (email, github, etc), the default options should work. It'll give you a public URL.

> If you want to redeploy to the same production URL you can run `yarn vercel --prod`. If you omit the `--prod` flag it will deploy it to a preview/test URL.

⚠️ Run the automated testing function to make sure your app passes

```bash
yarn test
```

---

## Checkpoint 5: 📜 Contract Verification

You can verify your smart contract by running:

```bash
cargo stylus verify -e https://sepolia-rollup.arbitrum.io/rpc --deployment-tx "$deployment_tx"
```

> It is okay if it says your contract is already verified.

## ⚡️ Cache Your Deployed Contract for Faster, Cheaper Access

> 📖 Contracts deployed on Arbitrum Sepolia can use this command for gas benefits, time savings, and cheaper contract function calls. Our backend will benchmark and place bids on your behalf to ensure your contract is not evicted from the CacheManager contract, fully automating this process for you.

Before caching your contract, make sure you have installed the Smart Cache CLI globally:

```bash
npm install -g smart-cache-cli
```

After deploying your contract to Arbitrum Sepolia, you can cache your contract address using the `smart-cache` CLI. Caching your contract enables:
- 🚀 **Faster contract function calls** by reducing lookup time
- 💸 **Cheaper interactions** by optimizing access to contract data
- 🌐 **Seamless access** to your contract from any environment or system

> 💡 **Info:** Both the `<address>` and `--deployed-by` flags are **mandatory** when adding a contract to the cache.

### 📝 Simple Example

```bash
smart-cache add <CONTRACT_ADDRESS> --deployed-by <YOUR_WALLET_ADDRESS_WITH_WHOM_YOU_HAVE_DEPLOYED_CONTRACT>
```

### 🛠️ Advanced Example

```bash
smart-cache add 0xYourContractAddress \
  --deployed-by 0xYourWalletAddress \
  --network arbitrum-sepolia \
  --tx-hash 0xYourDeploymentTxHash \
  --name "StylusToken" \
  --version "1.0.0"
```

- `<CONTRACT_ADDRESS>`: The address of your deployed contract (**required**)
- `--deployed-by`: The wallet address you used to deploy the contract (**required**)
- `--network arbitrum-sepolia`: By default, contracts are cached for the Arbitrum Sepolia network for optimal benchmarking and compatibility
- `--tx-hash`, `--name`, `--version`: Optional metadata for better organization

> ⚠️ **Warning:** If you omit the required fields, the command will not work as expected.

> 💡 For more options, run `smart-cache add --help`.

For more in-depth details and the latest updates, visit the [smart-cache-cli package on npmjs.com](https://www.npmjs.com/package/smart-cache-cli).

---

## 🔧 Smart Contract Details

### Contract Structure

The smart contract is built using Stylus and implements a standard ERC20 token with additional minting and burning capabilities:

```rust
// Main contract functions
pub fn mint(&mut self, value: U256) -> Result<(), Erc20Error>
pub fn mint_to(&mut self, to: Address, value: U256) -> Result<(), Erc20Error>
pub fn burn(&mut self, value: U256) -> Result<(), Erc20Error>
```

### Token Specifications
- **Name**: StylusToken
- **Symbol**: STK
- **Decimals**: 18
- **Network**: Arbitrum Sepolia
- **Standard**: ERC20 with mint/burn extensions

### Arbitrum Sepolia Details
- **Chain ID**: 421614
- **RPC URL**: `https://sepolia-rollup.arbitrum.io/rpc`
- **Block Explorer**: [Arbiscan Sepolia](https://sepolia.arbiscan.io/)
- **Bridge**: [Arbitrum Bridge](https://bridge.arbitrum.io/)

## 📚 Additional Resources

- [Stylus Documentation](https://docs.arbitrum.io/stylus)
- [Arbitrum Sepolia Faucet](https://faucet.quicknode.com/arbitrum/sepolia)
- [Arbiscan Sepolia Explorer](https://sepolia.arbiscan.io/)
- [Next.js Documentation](https://nextjs.org/docs)
- [Tailwind CSS Documentation](https://tailwindcss.com/docs)

---

> 🏃 Head to your next challenge [here](https://www.speedrunstylus.com/challenge/simple-nft-example).
