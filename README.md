<<<<<<< HEAD
# BLOCKCHAIN-AND-RUST-LEARNING
=======
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
   git clone -b erc20 https://github.com/abhi152003/speedrun_stylus.git
   ```

   ```bash
   cd speedrun_stylus
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
   git clone -b erc20 https://github.com/abhi152003/speedrun_stylus.git
   ```

   ```bash
   cd speedrun_stylus
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

### 🏠 Home Page - Welcome Interface

![ERC20 Home Page](https://raw.githubusercontent.com/abhi152003/speedrun_stylus/erc20/packages/nextjs/public/erc20-home.png)
*Visit `localhost:3000` to see the welcome page with navigation to Debug Contracts and Block Explorer tabs*

### 🔧 Debug Contracts - Token Operations

![ERC20 Debug Contracts Page](https://raw.githubusercontent.com/abhi152003/speedrun_stylus/erc20/packages/nextjs/public/erc20-debug.png)
*Navigate to "Debug Contracts" tab to mint, transfer, approve, and burn your STK tokens*

### 📊 Block Explorer - Transaction History

![ERC20 Block Explorer Page](https://raw.githubusercontent.com/abhi152003/speedrun_stylus/erc20/packages/nextjs/public/erc20-explorer.png)
*View all your transactions in the "Block Explorer" tab with direct links to Arbiscan*

### 🔗 Arbiscan - Blockchain Verification

![ERC20 Arbiscan Transaction](https://raw.githubusercontent.com/abhi152003/speedrun_stylus/erc20/packages/nextjs/public/erc20-sepolia-tx.png)
*Click "View on Arbiscan" to see your STK token transactions verified on Arbitrum Sepolia blockchain*

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

---

## 🚀 Submitting Your Challenge

After you have completed all checkpoints and are ready to submit your solution, follow these steps:

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

## 🔍 Analyze Your Contract with Radar

**Radar** by [Auditware](https://github.com/Auditware/radar) is a powerful static analysis tool designed to identify security vulnerabilities in Rust-based smart contracts. It uses a rule engine to detect common security issues like unchecked arithmetic, missing access controls, and account validation problems.

### 📦 Installation

Radar requires Docker to be installed and running on your system. Make sure Docker is installed and running before proceeding.

**Install Radar using the official installation script:** (⚠️ **Windows users must use a WSL terminal**)

```bash
curl -L https://raw.githubusercontent.com/auditware/radar/main/install-radar.sh | bash
```

This will install Radar globally on your system. Alternatively, you can install from source:

```bash
git clone https://github.com/auditware/radar.git
cd radar
bash install-radar.sh
```

**Note:** After installation, you must restart your terminal or run the following command so the system recognizes the newly installed `radar`:

```bash
source ~/.bashrc
```

### 🚀 Running Radar on Your Contract

From the root of your project, run Radar with:

```bash
radar -p .
```

This will analyze your entire project, including all contract code using Radar.

Radar will output:
- **Console output**: Real-time findings with severity levels (Low, Medium, High)
- **JSON report**: Detailed results saved to `output.json` in your project directory

**Example Output:**
```
[ Low ] Unchecked Arithmetics found at:
 * /path/to/your/contract/src/lib.rs:49:34-44

[i] radar completed successfully. json results were saved to disk.
[i] Results written to /path/to/output.json
```

> 💡 **Tip**: The JSON output (`output.json`) contains detailed information about each check, including severity, certainty, and locations of issues. Review it carefully to understand what needs to be fixed.

---

> 🏃 Head to your next challenge [here](https://www.speedrunstylus.com/challenge/simple-nft-example).
>>>>>>> erc20
