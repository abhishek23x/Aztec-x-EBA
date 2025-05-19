# Aztec-x-EBA
# Step-by-Step Guide on Aztec Network Sequential Node

---

## I. Requirements

- RAM: 16 GB  
- CPU: 8 cores  
- Storage: 200–1000 GB SSD  

---

## II. Prepare 

- New Metamask Wallet
- Load 1-3 Sepolia ETH 
- Get L1 Execution Layer RPC (EL-RPC): Sepolia 
- Get L1 Consensus Layer RPC (CL-RPC): Beacon

---


## III. You Can Run Through

- Local PC *(Not Recommended)*  
- One-Click Node Deployment Platforms:
  - [MintAir](https://www.mintair.xyz/dashboard)
  - [Easy Node](https://app.easy-node.xyz/)
- VPS

---

## IV. Exclusive Tips

### 1. For Stress-Free Setup:
Use one-click node deployment platforms.

### 2. Prefer VPS? Read Below:

#### VPS Platforms:
- [pq.hosting](https://pq.hosting/) — Expensive but smooth  
- [xorek.cloud](https://xorek.cloud) — Cheap  
- [contabo.com](https://contabo.com/en/vps-server) — Best, but credit card required  

### Smart Strategies to Run the Node:

1. Claim the Apprentice role and stop the node.  
2. Claim both Apprentice and Guardian roles, then stop the node.  
3. Claim both roles and run the node for a few days (5–15 days).  
4. Claim both roles and run the node continuously for 30 days or more.  

### Pros & Cons:

#### 1st Way:
- Pros: Very low cost  
- Cons: Low-tier participation  

#### 2nd Way:
- Pros: Low cost  
- Cons: Medium-tier participation  

#### 3rd Way *(Recommended)*:
- Pros: Decent cost  
- Cons: Good-tier participation  

#### 4th Way:
- Pros: High-tier participation  
- Cons: High cost  

---

## V. Guide & Commands

# Step 1: Update System Packages
```
sudo apt-get update && sudo apt-get upgrade -y
```
# Step 2: Install Required Dependencies
```
sudo apt install curl iptables build-essential git wget lz4 jq make gcc nano \
automake autoconf tmux htop nvme-cli libgbm1 pkg-config libssl-dev \
libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev -y
```

# Step 3: Install Docker (Clean Install)
```
sudo apt update -y && sudo apt upgrade -y
for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do 
  sudo apt-get remove -y $pkg 
done

sudo apt-get update
sudo apt-get install -y ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
  sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
  https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update -y && sudo apt upgrade -y
sudo apt-get install -y docker-ce docker-ce-cli containerd.io \
docker-buildx-plugin docker-compose-plugin

sudo docker run hello-world
sudo systemctl enable docker
sudo systemctl restart docker
```

# Step 4: Install Aztec Node
```
bash -i <(curl -s https://install.aztec.network)
```

# Step 5: Add Aztec to PATH
```
echo 'export PATH="$HOME/.aztec/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

# Step 6: Check Aztec CLI
```
aztec
```

# Step 7: Initialize Alpha Testnet
```
aztec-up alpha-testnet
```

# Step 8: Get Public IP
```
curl ipv4.icanhazip.com
```

# Step 9: Firewall Configuration
```
ufw allow 22
ufw allow ssh
ufw allow 40400
ufw allow 8080
ufw enable
```

# Step 10: Start Screen Session
```
screen -S aztec
```

# Step 11: Start Aztec Node (Replace Placeholder Values)
```
aztec start --node --archiver --sequencer \
  --network alpha-testnet \
  --l1-rpc-urls REPLACE \
  --l1-consensus-host-urls REPLACE \
  --sequencer.validatorPrivateKey 0xREPLACE \
  --sequencer.coinbase REPLACE \
  --p2p.p2pIp REPLACE \
  --p2p.maxTxPoolSize 1000000000
```

# Step 12: Check Proven Block Number
```
curl -s -X POST -H 'Content-Type: application/json' \
-d '{"jsonrpc":"2.0","method":"node_getL2Tips","params":[],"id":67}' \
http://localhost:8080 | jq -r ".result.proven.number"
```

# Step 13: Get Archive Sibling Path (Replace Parameters)
```
curl -s -X POST -H 'Content-Type: application/json' \
-d '{"jsonrpc":"2.0","method":"node_getArchiveSiblingPath","params":["REPLACE","REPLACE"],"id":67}' \
http://localhost:8080 | jq -r ".result"
```

# Step 14: Useful Screen Commands
## Minimize: ```Ctrl + A + D```
## Reattach: ```screen -r aztec```

