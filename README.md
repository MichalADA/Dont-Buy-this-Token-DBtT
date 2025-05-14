# Don't Buy This Token - Educational Project on Solana

## 📋 Project Overview
"Don't Buy This Token" is an educational project aimed at creating and implementing a custom token on the Solana blockchain. The goal of this project is to gain practical experience in blockchain development, Docker, Rust, and Solana CLI, while creating a functional token that can be deployed on the Solana mainnet.

## 🎯 Goals
- Creating a token on Solana devnet: Utilizing Solana's test environment to design and deploy a token.
- Deploying the token on mainnet: Spending about $20 to publish the token on Solana mainnet.
- Documenting the process: Providing clear instructions and insights for others interested in creating tokens.
- Developing a portfolio project: Showcasing skills in blockchain development and modern technologies.

## 🛠️ Key Features
- Token creation: Generating a token on Solana devnet and mainnet.
- Metadata integration: Adding metadata such as token name, symbol, and icon.
- Decentralized hosting: Using decentralized storage for token assets (e.g., Pinata/IPFS).
- Learning focus: Prioritizing understanding of blockchain technology and tools.

## 💻 Technologies Used
- Solana Blockchain: Platform for creating and managing the token.
- Docker: Containerization of the development environment for increased portability.
- Rust: Using Rust to work with Solana CLI.
- Pinata/IPFS: Hosting token metadata and assets.

## 📝 Steps to Execute
### 1. Environment Setup
- Install Docker.
- Configure a container with Solana CLI and Rust.

### 2. Creating a Token on Devnet
- Generate accounts for issuance authority and wallet.
- Create and configure the token on devnet.
- Upload metadata to decentralized storage.

### 3. Deploy Token on Mainnet
- Obtain SOL for mainnet deployment.
- Repeat the token creation process on mainnet.

### 4. Documentation and Publication
- Create a GitHub repository with detailed instructions.
- Share insights and experiences gained.

## 🤔 Why "Don't Buy This Token"?
The name emphasizes the experimental nature of the project and serves as a humorous reminder of the many token projects that don't deliver expected results. This project is meant to be an educational experience, not a speculative venture.

## 💎 Token Details
- Name: Don't Buy This Token
- Symbol: DBTt

## 📚 How to Create Your Own Token - Step by Step Guide

### 1. Installing Docker
Docker is my favorite way to deploy virtually everything. It's available on all platforms and very easy to install.

**For Mac**
- Link: https://docs.docker.com/desktop/setup/install/mac-install/

**For Linux/Windows (WSL)**
- Link: https://docs.docker.com/engine/install/ (link for all other distributions)

If you're using Ubuntu (main OS or WSL), follow these steps:

Documentation: https://docs.docker.com/engine/install/ubuntu/

**Set up Docker's apt repository**
```
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

**Install the latest Docker version**
```
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

**Check if it works**
```
sudo docker run hello-world
```

### 2. Configuring Docker Container for Solana
**Create a new folder for your token project**
```
mkdir your-token-name
cd your-token-name
```

**Create a Dockerfile**
```
nano Dockerfile
```

Copy and paste the following Dockerfile code:

```
# Use lightweight base image
FROM debian:bullseye-slim

# Set non-interactive frontend for apt
ENV DEBIAN_FRONTEND=noninteractive

# Install required dependencies and Rust
RUN apt-get update && apt-get install -y \
    curl build-essential libssl-dev pkg-config nano \
    && curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

# Add Rust to PATH
ENV PATH="/root/.cargo/bin:$PATH"

# Check Rust installation
RUN rustc --version

# Install Solana CLI
RUN curl -sSfL https://release.anza.xyz/stable/install | sh \
    && echo 'export PATH="$HOME/.local/share/solana/install/active_release/bin:$PATH"' >> ~/.bashrc

# Add Solana CLI to PATH
ENV PATH="/root/.local/share/solana/install/active_release/bin:$PATH"

# Check Solana CLI installation
RUN solana --version

# Set Solana configuration for Devnet
RUN solana config set -ud

# Set working directory
WORKDIR /solana-token

# Default command to run shell
CMD ["/bin/bash"]
```

**Build Docker image**
```
docker build -t heysolana .
```

**Run container**
```
docker run -it --rm -v $(pwd):/solana-token -v $(pwd)/solana-data:/root/.config/solana heysolana
```

### 3. Creating a Token on Solana Devnet
**Create an account for issuance authority**
```
solana-keygen grind --starts-with dad:1
```

**Set the account as default keypair**
```
solana config set --keypair dad-your-token-acount.json
```

**Switch to devnet**
```
solana config set --url devnet
```

**Check your configuration**
```
solana config get
```

**Get some SOL from the faucet**
To issue new tokens on the Solana network, you need SOL.

Check your Solana address:
```
solana address
```

Go to https://faucet.solana.com/ to receive an airdrop of SOL to the created account. Paste your Solana address and enter the amount of SOL you need. 2.5 will be MORE than enough.

Check your Solana balance:
```
solana balance
```

**Create a mint address**
```
solana-keygen grind --starts-with mnt:1
```

**Mint your TOKEN!**
```
spl-token create-token \
--program-id TokenzQdBNbLqP5VEhdkAS6EPFLC1PHnBqCXEpPxuEb \
--enable-metadata \
--decimals 9 \
mnt-your-mint-address.json
```

### 4. Uploading Metadata
**Prepare token icon**
Make sure it is:
- Square
- 512x512 or 1024x1024 pixels
- less than 100kb

**Upload image to decentralized storage**
- Create an account at https://app.pinata.cloud/
- Go to IPFS Files and upload your file
- Open the file and copy the URL

**Create metadata file**
```
nano metadata.json
```

Copy and paste the following format, filling in your data:

```
{
  "name": "Don't Buy This Token",
  "symbol": "DBTt",
  "description": "Educational project creating a token on the Solana blockchain.",
  "image": "PASTE_YOUR_IMAGE_URL_HERE"
}
```

**Upload metadata file to Pinata**
Follow the same steps as before. Upload to Pinata and copy the URL.

**Add metadata to token**
```
spl-token initialize-metadata \
YOUR_TOKEN_ADDRESS \
"Don't Buy This Token" \
"DBTt" \
URL_TO_YOUR_METADATA
```

**Check your token!**
- Go to Solana Explorer and choose devnet as your cluster
- Paste your token address in the search bar

### 5. Creating Tokens
**Create a token account**
```
spl-token create-account YOUR_TOKEN_ADDRESS
```

**Mint tokens**
```
spl-token mint YOUR_TOKEN_ADDRESS 1000
```

**Check your wallet balance**
```
spl-token balance YOUR_TOKEN_ADDRESS
```

**Send tokens to a friend**
```
spl-token transfer YOUR_TOKEN_ADDRESS 10 RECIPIENT_WALLET_ADDRESS --fund-recipient --allow-unfunded-recipient
```

### 6. Moving to Mainnet (optional)
To create a token on the main Solana network, repeat the above steps, but instead of switching to devnet, use:

```
solana config set --url mainnet-beta
```

Remember that on mainnet, you will need real SOL, which you must purchase.

**Securing the token**
After completing token configuration, you can disable mint and freeze authorities:

```
spl-token authorize YOUR_TOKEN_ADDRESS mint --disable
spl-token authorize YOUR_TOKEN_ADDRESS freeze --disable
```

**Updating metadata (if needed)**
```
spl-token update-metadata YOUR_TOKEN_ADDRESS uri NEW_URL_TO_METADATA
```

## 🔮 Future Plans
- Adding examples of token usage in small decentralized applications.
- Exploring integration with other blockchain ecosystems.
