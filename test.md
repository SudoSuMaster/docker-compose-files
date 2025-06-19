
# 🐳 Westend Validator Node via Docker

This repository provides a quick-start guide to running a **Polkadot Westend validator node** using Docker.

Westend is the official **testnet for Polkadot**, perfect for learning how to validate without risking real funds.

---

## 🚀 Quickstart

### ✅ Requirements

- Docker installed ([Install guide](https://docs.docker.com/get-docker/))
- At least **4 CPU cores**, **8GB RAM**, and **100GB+ SSD**
- Westend test tokens (WND) — available from the faucet (see below)

---

## 📦 Step 1: Pull the Polkadot Docker Image

```bash
docker pull parity/polkadot:latest

> You can replace latest with a specific version, e.g. parity/polkadot:v1.16.1




---

🗃️ Step 2: Create a Data Volume

mkdir -p $HOME/westend-data


---

⚙️ Step 3: Run the Node

docker run -d \
  --name westend-validator \
  -v $HOME/westend-data:/polkadot-data \
  -p 30333:30333 \
  -p 9933:9933 \
  -p 9944:9944 \
  parity/polkadot:latest \
  --chain=westend \
  --base-path /polkadot-data \
  --name "MyWestendValidator" \
  --validator \
  --telemetry-url "wss://telemetry.polkadot.io/submit/ 0" \
  --rpc-cors all \
  --rpc-methods=Unsafe \
  --unsafe-rpc-external \
  --ws-external \
  --rpc-external

> Modify "MyWestendValidator" to your own validator name.




---

🔑 Step 4: Generate Session Keys

Option 1: Using CLI inside the container

docker exec -it westend-validator /bin/bash
polkadot key generate --scheme sr25519

Save these keys to be used on-chain when registering your validator.

Option 2: Use Polkadot-JS UI

1. Go to https://polkadot.js.org/apps


2. Switch to Westend


3. Use the Developer → "Extrinsics" section to set session keys




---

💰 Step 5: Get Westend Test Tokens

Join the Westend Faucet Matrix room: 👉 #westend_faucet:matrix.org

Then request tokens by typing:

!drip YOUR_WESTEND_ADDRESS


---

🗳️ Step 6: Register as a Validator

1. Visit https://polkadot.js.org/apps


2. Switch to Westend


3. Navigate to Staking → Account Actions


4. Bond tokens


5. Set session keys


6. Click Validate




---

📡 Step 7: Monitor Your Node

View your node on telemetry: 🔗 Westend Telemetry Dashboard


---

🧼 Useful Docker Commands

Stop the node:

docker stop westend-validator

Remove the container:

docker rm westend-validator


---

📄 License

MIT License


---

🙋 Need Help?

Open an issue or reach out via Polkadot Discord.

---

Would you like me to create a full GitHub repo with this structure (`README.md`, maybe a `docker-compose.yml`, `.env`, etc.) as a downloadable `.zip` or ready for push?

