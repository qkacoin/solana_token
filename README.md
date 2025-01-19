# solana_token
how to create a solana_token in ubuntu20.04

# Solana CLI Guide and initai a token on devnet

## System

Ubuntu 20.04

---

## Install Solana CLI

### 1. Download Solana CLI
```bash
curl -sSfL https://release.anza.xyz/stable/install | sh
```

### 2. Configure PATH
Replace `/home/ubuntu` with your actual home directory:
```bash
export PATH="/home/ubuntu/.local/share/solana/install/active_release/bin:$PATH"

echo 'export PATH="/home/ubuntu/.local/share/solana/install/active_release/bin:$PATH"' >> ~/.bashrc
```

### 3. Test the Installation
```bash
solana --version
```
Expected output:
```
solana-cli 2.0.23 (src:7f759d1b; feat:607245837, client:Agave)
```

---

## Create a Primary Address

```bash
solana-keygen grind --starts-with wwk:1
```
- `--starts-with wwk:1` ensures the address starts with `wwk`. The `:1` part is unclear.  
- You can replace `wwk` with other letters. Numbers haven't been tested.

Example output:
```
Searching with 2 threads for:
	1 pubkey that starts with 'wwk' and ends with ''
Wrote keypair to wwkbpuojzGQxpRJNRGpPZk2tB7Qnrboe2RksyMFbN8z.json
```
The generated `.json` file contains the private key. View its content:
```bash
cat wwkbpuojzGQxpRJNRGpPZk2tB7Qnrboe2RksyMFbN8z.json
```

Example private key:
```
[224,96,79,43,...]  # Full key in square brackets
```
To import into a mobile wallet, copy the entire key, including brackets.

---

## Set the Primary Wallet Address

```bash
solana config set --keypair wwkbpuojzGQxpRJNRGpPZk2tB7Qnrboe2RksyMFbN8z.json
```

Example response:
```
Config File: /home/ubuntu/.config/solana/cli/config.yml
RPC URL: https://api.mainnet-beta.solana.com 
Keypair Path: wwkbpuojzGQxpRJNRGpPZk2tB7Qnrboe2RksyMFbN8z.json 
```

---

## Check Wallet Address and Balance

```bash
solana address

solana balance
```

---

## Switch to Devnet

```bash
solana config set --url devnet
```

Example response:
```
RPC URL: https://api.devnet.solana.com 
```

---

## Check Solana Configuration

```bash
solana config get
```

---

## Get Testnet SOL

Go to [Solana Faucet](https://faucet.solana.com/), enter your wallet address, and link a GitHub account.

---

## Create a Mint Address

```bash
solana-keygen grind --starts-with mnt:1
```

Example output:
```
Wrote keypair to mntL4KLTfycJ5WgxxpA5dBTCBSfZqEqPAC4EaT1hN5j.json
```

---

## Create a Token

```bash
spl-token create-token --program-id TokenzQdBNbLqP5VEhdkAS6EPFLC1PHnBqCXEpPxuEb --enable-metadata --decimals 9 mntL4KLTfycJ5WgxxpA5dBTCBSfZqEqPAC4EaT1hN5j.json
```

Output:
```
Creating token mntL4KLTfycJ5WgxxpA5dBTCBSfZqEqPAC4EaT1hN5j under program TokenzQdBNbLqP5VEhdkAS6EPFLC1PHnBqCXEpPxuEb
Address:  mntL4KLTfycJ5WgxxpA5dBTCBSfZqEqPAC4EaT1hN5j
Decimals:  9
```

---

## Upload Metadata to IPFS

### 1. Upload Icon to IPFS
- Register at [Pinata](https://app.pinata.cloud/).
- Upload your token icon (PNG, <100KB).
- Get the public link to the uploaded file.

### 2. Create Metadata File
Create `metadata.json`:
```json
{
    "name": "AI all over the world",
    "symbol": "AIOW",
    "description": "Just a cool little coin, just for me.",
    "image": "https://your-icon-link"
}
```

### 3. Upload Metadata File
Upload `metadata.json` to Pinata and get its link.

---

## Add Metadata to the Token

```bash
spl-token initialize-metadata mntL4KLTfycJ5WgxxpA5dBTCBSfZqEqPAC4EaT1hN5j "AI all over the world" "AIOW" https://your-metadata-link
```

---

## Disable Future Minting

```bash
spl-token authorize mntL4KLTfycJ5WgxxpA5dBTCBSfZqEqPAC4EaT1hN5j mint --disable
```

---

## Prevent Freezing of Accounts

```bash
spl-token authorize mntbDZof8kRCoSkjW6tR9J7mvBBM4FWLQ2ysEJowAyh freeze --disable
```

---

## Restrict Pool Withdrawal

Burn LP tokens to limit withdrawal permissions:
```bash
spl-token burn LPTokenAddress 100
```

---
