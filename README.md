**Guide to Setting Up a Validator Node for Babylon Testnet**

### Prerequisites

Before you begin, ensure that your system meets the following minimum requirements:

- **Operating System**: Ubuntu 22.04 (recommended)
- **CPU**: 4 cores (6 cores recommended)
- **RAM**: 16 GB (32 GB recommended)
- **Storage**: 400 GB SSD (1 TB SSD recommended)
- **Network**: Stable internet connection with at least 1 Mbps upload/download speed.
- **Software**: `curl`, `git`, `build-essential`, `jq`, and `go` installed.

---

### Step 1: Update System

1. Update the system package list and upgrade installed packages:
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```

2. Install required dependencies:
   ```bash
   sudo apt install curl git build-essential jq -y
   ```

---

### Step 2: Install Go

1. Download the latest Go binary:
   ```bash
   curl -LO https://go.dev/dl/go1.21.1.linux-amd64.tar.gz
   ```

2. Extract and install Go:
   ```bash
   sudo tar -C /usr/local -xzf go1.21.1.linux-amd64.tar.gz
   ```

3. Add Go to your PATH:
   ```bash
   echo "export PATH=/usr/local/go/bin:$PATH" >> ~/.bashrc
   source ~/.bashrc
   ```

4. Verify installation:
   ```bash
   go version
   ```

---

### Step 3: Clone Babylon Repository

1. Clone the Babylon repository:
   ```bash
   git clone https://github.com/babylonchain/babylon.git
   ```

2. Navigate to the directory:
   ```bash
   cd babylon
   ```

3. Checkout the latest testnet branch:
   ```bash
   git checkout bbn-test-5
   ```

---

### Step 4: Build the Node

1. Compile the binary:
   ```bash
   make install
   ```

2. Verify the installation:
   ```bash
   babylond version
   ```

---

### Step 5: Configure the Node

1. Initialize the node:
   ```bash
   babylond init <YOUR_MONIKER> --chain-id bbn-test-5
   ```

2. Download the genesis file:
   ```bash
   curl -o ~/.babylond/config/genesis.json https://testnet-files.babylonchain.io/bbn-test-5/genesis.json
   ```

3. Set seeds and peers in the configuration file:
   ```bash
   sed -i 's/seeds = .*/seeds = "<seed_nodes>"/' ~/.babylond/config/config.toml
   ```

4. Optimize the `config.toml` file:
   - Set `minimum-gas-prices`:
     ```bash
     sed -i 's/minimum-gas-prices = .*/minimum-gas-prices = "0.01ubbn"/' ~/.babylond/config/app.toml
     ```
   - Adjust pruning settings for optimal performance.

---

### Step 6: Start the Node

1. Start the node service:
   ```bash
   babylond start
   ```

2. Monitor logs:
   ```bash
   journalctl -u babylond -f
   ```

---

### Step 7: Create a Validator

1. Ensure your node is fully synchronized. Use:
   ```bash
   babylond status
   ```

2. Fund your wallet with test tokens:
   ```bash
   babylond tx bank send <YOUR_WALLET_ADDRESS> <FAUCET_ADDRESS> <AMOUNT>ubbn --chain-id bbn-test-5
   ```

3. Create the validator:
   ```bash
   babylond tx staking create-validator \
     --amount=1000000ubbn \
     --pubkey=$(babylond tendermint show-validator) \
     --moniker=<YOUR_MONIKER> \
     --chain-id=bbn-test-5 \
     --commission-rate=0.10 \
     --commission-max-rate=0.20 \
     --commission-max-change-rate=0.01 \
     --min-self-delegation=1 \
     --from=<YOUR_WALLET_NAME>
   ```

---



