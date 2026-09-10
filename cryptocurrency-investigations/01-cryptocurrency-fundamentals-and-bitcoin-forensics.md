# Cryptocurrency Fundamentals and Bitcoin Forensics

## Part 1: Course Overview and Foundations of Cryptocurrency

**Course Roadmap:** the curriculum progresses from foundations, to wallets/keys, transactions, forensic analysis (Bitcoin and Ethereum), analysis tools, case studies, alternative blockchains, and finally investigative techniques.

**What is Cryptocurrency?**
- A form of digital money
- Operates with no central authority (decentralized)
- Relies on a decentralized network of nodes to function

**The Blockchain:**
- A specialized type of public database known as a "ledger"
- **Distributed Ledger** — every entity (node) in the network keeps its own copy of this ledger
- **Immutability** — once data is recorded, it can never be erased or modified; records are permanent and publicly verifiable

## Part 2: Wallets, Keys, and Forensic Aspects

### Wallet Types

- **Hot Wallets** — connected to the internet, convenient for day-to-day transactions but easier to attack via malware or phishing; examples: mobile wallets (Trust Wallet, MetaMask), web/exchange wallets (Kraken, Coinbase)
- **Cold Wallets** — never or only briefly connected to the internet, providing a higher level of security; examples: hardware wallets (Ledger, Trezor), paper wallets (printed/handwritten), and air-gapped computers

### Forensic Considerations for Wallets

- **Hot Wallets/Exchanges** — investigators can find valuable information like IP-address logs, browser extensions, and detailed account history on exchanges
- **Cold Wallets** — leave a very small digital footprint; investigation often requires physical access to locate hardware devices, written seed phrases, or external backups
- Every wallet handles key storage, seed phrase generation, and transaction signing slightly differently

### Public and Private Keys

- **Private Key** — randomly generated and stored in a wallet file; provides access to funds (signing and sending transactions); comparable to an email password — if lost, funds are permanently inaccessible
- **Public Key** — derived from the private key; serves as the receiving identity on the blockchain and is transformed into a wallet address, similar to an email address

### Hierarchical Deterministic (HD) Wallets (BIP-32, BIP-39, BIP-44)

- **Non-Hierarchical vs. HD Wallets** — in non-hierarchical wallets, every private key must be saved separately; in an HD wallet (BIP-32), a single seed phrase (12–24 words) acts as a backup for the entire wallet
- **Master Keys** — the seed generates a Master Extended Private Key, which then produces "child" private keys, public keys, and addresses
- **BIP-39** — defines the 12-word seed phrase format; an optional "passphrase" can be added for "salting" to increase security
- **BIP-44** — allows for a multi-account hierarchy, meaning multiple different cryptocurrencies (e.g. Bitcoin, Ethereum, Monero) can be managed under a single root seed phrase

**Investigator's Perspective:** addresses that appear unrelated on the blockchain may actually belong to the same HD wallet if they were derived from the same seed.

## Part 3: Transaction Process, Consensus, and Ledger Models

### The Transaction Process

1. **Initiation** — a sender (Alice) signs a transaction with her private key using the recipient's (Bob) public address
2. **Broadcasting** — the transaction is broadcast to the decentralized network of nodes
3. **Verification** — nodes verify the transaction and group it with others into a new block
4. **Finalization** — a consensus mechanism adds the block to the blockchain, and ownership of the funds is officially changed

### Consensus Mechanisms

- **Proof of Work (PoW)** — used by Bitcoin; relies on "miners" solving complex cryptographic puzzles; requires significant Graphics Processing Unit (GPU) power
- **Proof of Stake (PoS)** — used by Ethereum 2.0 and Solana; selects validators based on the amount of cryptocurrency they have staked (locked up as collateral)

### Unspent Transaction Output (UTXO) Model (e.g. Bitcoin)

- **The Logic** — you don't have a "balance" in the traditional sense; you have a collection of unspent chunks of currency from previous transactions
- **The Input/Output System** — to send money, you use a previous "output" as an "input"
- **Change Addresses** — if your input (e.g. 0.5 BTC) is more than what you want to send (e.g. 0.3 BTC), the surplus (minus a miner fee) is sent back to a change address controlled by your wallet
- **Investigator's Note** — this model provides a clear chain of ownership for specific funds

### Account-Based Model (e.g. Ethereum)

- **The Logic** — operates like a traditional bank account where a global state tracks the current balance of every address
- **Gas and Fees** — gas represents the computational effort required to process a transaction, denominated in gwei (a subunit of ETH); complex actions like smart contracts, DeFi trades, or NFT minting require higher fees than simple transfers
- **Smart Contracts** — automated accounts that execute code (e.g. Uniswap) and can process multiple on-chain events
- **Investigator's Note** — this model focuses on contract interactions, fee patterns, and token flows

## Part 4: Bitcoin Forensics — Address Formats and Change Detection

### Bitcoin Address Creation

**The Process:** a private key (secret) derives a public key, which is then transformed through encoding and hashing into a public address (share).

**Private Key Formats** (crucial for investigators to recognize if found in text files):
- Raw Private Key — 256 bits, 64 characters
- WIF (Wallet Import Format) — starts with 5, 6, L, or K
- Mini Private Key — starts with S
- BIP-32 (Master Key) — starts with `xprv`

### Bitcoin Address Formats

| Format | Description | Introduced | Prefix |
|---|---|---|---|
| P2PKH (Pay-to-Public-Key-Hash) | The oldest type | 2009 | starts with "1" |
| P2SH (Pay-to-Script-Hash) | Introduced for multi-signature transactions | 2012 | starts with "3" |
| Bech32 (SegWit) | — | 2017 | starts with "bc1q" |
| Bech32m (Taproot) | The newest format; improves privacy and is case-insensitive | 2021 | starts with "bc1p" |

### Determining the Change Address (Forensic Techniques)

When analyzing a UTXO transaction with multiple outputs, investigators use these heuristics to identify which one is the sender's change:

1. **Repetition of Address** — if an address appears as both an input and an output in the same transaction, it is likely the change address
2. **Consistent Format** — if the input address and one of the output addresses share the same format (e.g. both start with "1"), that output is likely the change
3. **Round vs. Irregular Amounts** — often, the recipient is sent a "round" amount (e.g. 3.0 BTC), while the leftover "irregular" amount (e.g. 1.9 BTC) goes to the change address
4. **Multiple Inputs and Small Change** — if a sender combines several small inputs to reach a target amount, the resulting smallest output is usually the change
5. **Future Activity** — if an output address is later used as an input alongside a known address from the suspect's wallet, that output was confirmed to be change

**Investigator Hint:** if a transaction has no change address, the suspect may have fully spent the balance or switched to a completely new wallet.

## Part 5: Clustering, Mixers, and Demixing Strategies

### Clustering Addresses

- **The Concept** — clustering is the process of grouping multiple blockchain addresses together under the assumption that they are controlled by the same entity or wallet
- **Tools** — tools like WalletExplorer.com are used to identify these groups
- **Investigator's Caution** — never assume clustering tools are perfect; complex wallet structures (sometimes called "hydra clusters") can lead to false associations

### Mixers and CoinJoins

- **Mechanism** — a CoinJoin is a transaction where multiple participants (often 5–100) combine their inputs into a single large transaction to obscure the trail of funds
- **Fixed Outputs** — to maintain anonymity, the outputs are usually for fixed, equal amounts (e.g. everyone receives exactly 0.5 BTC back), making it difficult to tell which output belongs to which sender
- **Identifying CoinJoins** — investigators look for distinct patterns, such as an unusual number of inputs/outputs or transactions where fees are sent to a known coordinator address (fingerprinting)

### Tracing and "Demixing" Strategies

Demixing often relies on human error or software flaws rather than breaking the math of the blockchain. Key techniques include:

- **Address Reuse** — a major mistake where a participant uses an input address as a change address, immediately linking their identity to the mix
- **Toxic Change** — if a participant sends an irregular amount (e.g. 1.2345 BTC) into a mix that only produces 1 BTC round outputs, the remaining "toxic change" (0.2345 BTC) is easily linked back to them
- **Subset Sum Analysis** — a mathematical approach to match combinations of inputs to specific outputs; while some matches are ambiguous, others are deterministic (there is only one possible mathematical combination), allowing for deanonymization
- **Output Clustering** — de-anonymizing a suspect if they later spend a "mixed" output alongside an address that was already known to belong to them
- **PayJoin** — a specialized privacy technique where the recipient joins the input side of the transaction, making it appear like a standard multi-input transaction rather than a payment

### Modern Mixer Landscape (2025)

- **Mixero.io** — offers Bitcoin-to-Monero-to-Bitcoin conversion for higher anonymity
- **Wasabi Wallet** — an open-source wallet that notably collaborates with law enforcement to prevent illicit use
- **JoinMarket** — a decentralized marketplace for CoinJoins available on GitHub

**Investigative Reality:** because mixing is becoming more sophisticated, investigators increasingly rely on external metadata (forum posts, darknet logs), network analysis, and following funds until they reach regulated exchanges.

## Part 6: Blockchain Forks and No-Input Transactions

### Eyes on the Forks

- **The Concept** — a "fork" occurs when a blockchain splits into two separate paths, often due to community disagreements or software updates
- **Major Bitcoin Forks** — Bitcoin Cash (BCH), Bitcoin XT (BXT), Bitcoin Classic, Bitcoin Gold, and Bitcoin SV (BSV)
- **Forensic Significance** — suspects who held Bitcoin at the time of a fork automatically received the same amount in the new forked coins
- **Dual-Chain Activity** — tools like Blockchain.com can show a single search result for one address across multiple chains (e.g. both BTC and BCH), helping investigators identify additional assets a suspect might own

### No Input (Coinbase) Transactions

- **Origin of New Coins** — unlike standard transactions that move existing funds, "No Input" or "Coinbase" transactions are the mechanism by which new bitcoins are created through mining
- **The Reward** — when a miner successfully adds a new block to the blockchain, they receive a block reward (currently 3.125 BTC per block) plus all transaction fees from that block
- **Identifying Mining Activity** — in these transactions, the "From"/"Input" field is effectively empty or listed as "Block Reward"/"Coinbase" with a value of 0.00 BTC, because the coins are being generated for the first time and did not exist previously

## Part 7: Analysis Tools, Service Identification, and Legal Actions

### Blockchain Analysis Tools

- **Blockchain Explorers** (e.g. Blockchain.com) — the primary tools for viewing live transactions, block heights, and network hash rates; particularly useful for seeing dual-chain activity, such as whether a single address has history on both the Bitcoin and Bitcoin Cash blockchains
- **Clustering & Labeling Tools** (e.g. WalletExplorer.com) — specialize in grouping addresses and identifying the entities behind them; provide labels for:
  - Exchanges — Kraken, Binance, Coinbase
  - Mining pools — AntPool, SlushPool, BTCCPool
  - Services/others — CoinPayments, BitPay, various mixers
  - Gambling sites — SatoshiDice, CloudBet
- **Wallet Security Verification** (e.g. WalletScrutiny.com) — allows investigators and users to verify if a wallet (like SeedSigner or Coldcard) is truly open-source and secure by checking its binaries

### Investigating Services and Entities

- **The "Hydra Cluster"** — large entities like exchanges often have massive, complex clusters of addresses that can be difficult to map fully
- **Metadata Leakage** — beyond the blockchain itself, investigators rely on external metadata, such as forum posts, darknet logs, and "fingerprinting" known coordinator wallets used by mixers

### Law Enforcement Seizures and Actions

- **Coordinated Takedowns** — major law enforcement agencies (including the FBI, Europol, BKA, and HSI) conduct coordinated operations to seize domains used for money laundering
- **Case Example (ChipMixer)** — the seizure of services like ChipMixer, where the website was taken down as part of an international law enforcement action
- **KYC and Regulation** — a key strategy for investigators is following the "money trail" until it reaches regulated exchanges, where identity verification (Know Your Customer) may reveal the suspect's real-world identity

---
*Source: coursework notes (AI-compiled study notes), Investigating Cryptocurrencies module.*
