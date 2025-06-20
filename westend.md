# Westend Validator Node via Docker

This guide explains how to run a Polkadot Westend validator node using Docker.

Westend is the official testnet of Polkadot, ideal for practicing staking and validation.

---

## Requirements

- Docker installed: https://docs.docker.com/get-docker/
- At least 4 CPU cores, 8 GB RAM, and 100+ GB SSD
- A Westend account with WND tokens (from the faucet)

---

## Step 1: Pull the Polkadot Docker image

```
docker pull parity/polkadot:latest
```

---

## Step 2: Create a persistent data directory

```
mkdir -p $HOME/westend-data
```

---

## Step 3: Start the validator node

```
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
```

Change `"MyWestendValidator"` to your preferred validator name.

---

## Step 4: Generate session keys

Enter the container:

```
docker exec -it westend-validator /bin/bash
```

Then generate keys:

```
polkadot key generate --scheme sr25519
```

Save the output securely — you will register these keys on-chain.

---

## Step 5: Get Westend tokens (WND)

Join the faucet chat on Matrix:  
https://matrix.to/#/#westend_faucet:matrix.org

Then request tokens:

```
!drip YOUR_WESTEND_ADDRESS
```

---

## Step 6: Register your validator

1. Open https://polkadot.js.org/apps
2. Switch to the Westend network
3. Go to "Staking" → "Account Actions"
4. Bond tokens
5. Set session keys
6. Click "Validate"

---

## Step 7: Monitor your node

View telemetry here:  
https://telemetry.polkadot.io/#/Westend

---

## Docker commands

Stop the node:

```
docker stop westend-validator
```

Remove the container:

```
docker rm westend-validator
```

---

## License

MIT
