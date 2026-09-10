# Advanced Blockchain Forensics: Revision Series

## Part 1: Cross-Chain, Stablecoin, and Multi-Chain Forensics

Practical forensic logic required to trace activity across different blockchains, with specific emphasis on stablecoins, the Solana network, and cross-chain messaging protocols.

### 1. USDT and Cross-Chain Bridges

**Definition and Importance:** USDT (Tether) is a fiat-referenced stablecoin that exists across multiple blockchains, such as Ethereum, Tron, and Solana. It is critical in investigations because it has high liquidity, stable value (tracking fiat), and is frequently used by criminals to move proceeds across chains.

**Key Forensic Fields to Record:**
- Blockchain/Network — essential for selecting the correct explorer
- Token Contract/Mint Address — to verify the asset is genuine USDT and not an imitation
- Transaction Hash/Signature — the primary on-chain identifier (referred to as a "signature" on Solana)
- Decimals — necessary for converting raw units into human-readable values

**Understanding Cross-Chain Bridges:** bridges enable value or messages to move between ledgers that do not natively share a record.

**Bridge Models:**
- **Lock-and-Mint** — assets are locked on the source chain, and a "wrapped" version is minted on the destination
- **Burn-and-Release** — wrapped assets are burned on one chain to release the original asset on another
- **Liquidity Bridges** — users deposit into a pool on one chain and receive a payout from a pool on another

**Common Mistakes:** investigators must avoid assuming similar amounts on different chains are the same funds without proof, as fees and slippage can change values. They must also distinguish between native and "wrapped" tokens.

### 2. Solana Forensics

**Core Concepts:** unlike Ethereum's account-based model, Solana uses accounts, programs, instructions, and signatures. A single transaction can contain multiple instructions; if one fails, the entire transaction reverts.

**Key Artefacts:**
- Signature — the primary transaction identifier
- Program IDs — identifying the specific logic invoked (e.g. Token Program vs. a DEX)
- Associated Token Account (ATA) — on Solana, tokens like USDT are held in accounts derived from the user's wallet address and the token mint
- Inner Instructions — essential for DeFi or bridge tracing, as they reveal program-to-program calls not visible in top-level summaries

**Forensic Workflow:** investigators should record the fee payer, list all signers, and separate value movement from administrative actions like account creation or closure.

### 3. Wormholescan (Cross-Chain Messaging)

**Mechanism:** Wormhole is a messaging protocol. It uses Guardians to observe source-chain messages and sign a VAA (Verified Action Approval), which is then submitted to the destination chain for execution.

**Key Terms for Investigators:**
- Emitter — the contract/program on the source chain that started the operation
- Sequence Number — a unique identifier used with the emitter to track a specific message
- Redeem/Completion Transaction — the specific transaction on the target chain that "consumed" the VAA to release funds

**Practical Workflow:**
1. Search by source transaction hash or message ID
2. Extract emitter and sequence number
3. Verify the transaction in the native explorers of both the source and destination chains, preserving independent screenshots

**Reporting Errors:** avoid calling Wormholescan a "wallet" or "exchange" — it is an explorer/API tool. Ensure relayers are identified, as they may submit destination transactions but do not own the funds.

### 4. Summary Checklist for Students

- **Bridges** — always preserve both source and destination records and document bridge-specific identifiers like VAAs or nonces
- **Solana** — never confuse a token account address with a human wallet address; review every instruction in the instruction tree
- **Wormholescan** — capture the emitter, sequence number, and proof status (e.g. "redeemed")

## Part 2: Crypto Asset Identification, Seizure, and Recovery

Identification, preservation, and forensic handling of crypto-related evidence, from physical kiosks to digital wallet artefacts.

### 1. Identifying Blackbox Services and Bitcoin ATMs

**Blackbox Services:** services where incoming and outgoing blockchain activity is visible, but internal ownership, customer records, and liquidity sources remain opaque.
- Examples: informal brokers, P2P cash traders, unlabelled exchange wallets, and Telegram/WhatsApp exchangers
- Forensic Indicators: pooled wallets, lack of a clear legal entity, changing deposit addresses, and frequent use of private chat channels
- Attribution Risk: these services create risk because they sit between victims and regulated entities; they should be documented as service nodes rather than final suspects

**Bitcoin ATMs (CVC Kiosks):** physical kiosks allowing cash-to-crypto conversion.
- Victim-Scam Pattern — fraudsters use impersonation and urgency to instruct victims to withdraw cash and deposit it into a kiosk using a provided QR code
- Key Artefacts — printed receipts (containing tx hashes), QR codes (encoding destination addresses), CCTV from the location, and operator KYC records
- Forensic Workflow — investigators should correlate kiosk timestamps with bank withdrawals, phone logs, and blockchain confirmation times

### 2. Seizing Crypto Assets: Wallets, MetaMask, and Seeds

**Defining Seizure:** seizure may involve physical taking of devices, digital imaging of logs, or the on-chain securing of funds by moving them to a government-controlled wallet.

**Wallet Types to Recognise:**
- Browser Extensions (e.g. MetaMask, Phantom) — artefacts include extension folders, locked/unlocked state, and connected sites
- Mobile Wallets (e.g. Trust Wallet) — require mobile forensic procedures and network isolation to prevent remote wipes
- Hardware Wallets (e.g. Ledger, Trezor) — physical USB devices; never guess PINs as they may trigger a device wipe
- Paper/Metal Backups — 12/18/24-word seed phrases must be handled as high-risk evidence

**MetaMask Specifics:**
- Local Password — unlocks the vault on a specific device
- Secret Recovery Phrase (SRP) — restores the entire wallet and all derived accounts on any device
- Connected Sites — identifies interactions with DeFi, NFT marketplaces, or phishing sites

### 3. Passphrase-Protected Ethereum Wallets (Keystore Files)

**Keystore Structure:** Ethereum private keys are often stored in JSON-based keystore files using the Web3 Secret Storage format.

**Core Elements:**
- Ciphertext — encrypted private key data
- KDF (Key-Derivation Function) — typically scrypt or PBKDF2, designed to slow down password guessing
- MAC — used to verify if a derived key is correct before attempting decryption

**Passphrase Recovery:** recovery depends on testing candidate passwords (from notes, password managers, or browser stores) against the file. Strong, random passwords may be unrecoverable.

### 4. Scanning Devices for Artefacts

**Artefact Locations:** investigators search browser profile data, wallet databases, screenshots of seed phrases, QR codes, and exchange-related emails.

**Pattern Matching:**
- Ethereum addresses — start with `0x` followed by 40 hex characters
- Bitcoin addresses — legacy, SegWit, or bech32 formats
- BIP-39 words — lists of 12/18/24 words that may be seed phrases

**Forensic Principle:** a pattern match is only a lead. It must be validated against transaction history and case context; do not report every match as a wallet.

### 5. Public Databases and OSINT

**OSINT Sources:** blockchain explorers, official sanctions lists (e.g. OFAC), scam-report databases (e.g. Chainabuse), and social media.

**OSINT Workflow:**
- Screen addresses against known illicit clusters or exchange labels
- Capture evidence: use screenshots with timestamps and archived URLs
- Wording: use cautious language like "The address is publicly reported as..."

### 6. Integrated Investigation Workflow (Summary)

1. **Intake** — record victim reports and communication channels
2. **Preservation** — secure devices and hash images
3. **On-Chain Tracing** — follow funds to services or exchanges
4. **Device Examination** — find keys, seeds, and QR codes
5. **OSINT Enrichment** — screen against public labels and sanctions
6. **Legal/Service Actions** — prepare requests to exchanges or ATM operators
7. **Recovery/Securing** — secure assets under authority
8. **Reporting** — explain methods, limitations, and confidence levels

## Part 3: Crypto Crimes, Scams, Hacks, and Fraud Case Studies

Classification of cryptocurrency crimes, forensic investigation principles, and detailed analysis of three major public case studies.

### 1. Types of Crypto Crimes: Scams, Hacks, and Fraud

Cryptocurrency crime is not a single offence type. Correct classification is vital because the evidence, reporting routes, and prevention lessons differ.

- **Scams** — usually involve manipulating or deceiving a victim into authorising a transfer of funds themselves
- **Hacks** — usually involve unauthorised access or exploitation of technical or operational weaknesses (e.g. compromising private keys or smart contract bugs)
- **Fraud** — focuses on deception, misrepresentation, or the dishonest use of entrusted funds (e.g. fake investment platforms or "rug pulls")

**Common Categories and Forensic Indicators:**
- Investment/Romance Scams — victims are persuaded to deposit crypto into fake platforms; indicators include newly created addresses, staged profit screenshots, and inability to withdraw
- Phishing & Wallet-Drainers — tricking users into revealing seed phrases or signing malicious approvals; indicators include suspicious domains and token transfers immediately following a signature
- Exchange/Wallet Hacks — compromising infrastructure or keys; indicators include large, unusual withdrawals and compromised SIEM/cloud logs
- DeFi/Smart Contract Exploits — exploiting logic or oracle flaws; indicators include rapid multi-step transactions, flash loans, and specific contract event logs
- Laundering & Sanctions Evasion — moving proceeds through mixers, bridges, and "peeling chains" to obfuscate the trail

### 2. Forensic Foundations and Investigation Principles

- **Evidence Preservation** — investigators must document seed phrases, wallets, chat logs, timestamps, and transaction hashes early
- **Separation of Evidence** — on-chain records show value movement; off-chain evidence (IP logs, KYC, telecom records) helps identify people and intent
- **Core Artefacts** — transaction hashes (unique identifiers), wallet addresses, token contract addresses (to confirm asset authenticity), and device/chat evidence
- **Workflow** — define the incident type, build a timeline (using UTC), trace funds to service touchpoints (exchanges, mixers), and evaluate attribution with clearly stated confidence levels

### 3. Case Study: BitMart Hack (December 2021)

- **The Incident** — approximately USD 196 million was stolen from BitMart's Ethereum and Binance Smart Chain (BSC) hot wallets
- **Root Cause** — a stolen private key compromised the hot wallets
- **Forensic Lessons** — hot wallets (connected to online systems) are high-risk points for operational liquidity; tracing must cover multiple chains when assets are taken from different networks
- **Preventive Controls** — use cold storage for the majority of assets, multi-signature (multi-sig) or Multi-Party Computation (MPC) for signing, and strict withdrawal limits/anomaly alerts

### 4. Case Study: Crypto 2FA Bypass

- **Concept** — SMS-based two-factor authentication (2FA) is vulnerable to SIM swapping, where an attacker takes control of a victim's phone number to intercept one-time codes and reset passwords
- **The Nicholas Truglia Case** — a notable example where attackers stole over USD 20 million via SIM swapping
- **Attack Chain** — reconnaissance → SIM swap → account recovery (via SMS reset) → withdrawal and laundering
- **Forensic Evidence** — telecom records (SIM replacement time), email password reset logs, and exchange login IP addresses
- **Defensive Lessons** — prefer hardware security keys over SMS 2FA; use withdrawal allowlists and time delays for new addresses

### 5. Case Study: Ronin Bridge Hack (March 2022)

*See [[ronin-bridge-lazarus-group]] for a full transaction-level investigation of this case.*

- **The Incident** — attackers stole 173,600 ETH and 25.5 million USDC (approx. USD 620 million) from the Ronin bridge, linked to the Axie Infinity ecosystem
- **Root Cause** — validator key compromise and a low validator threshold; only five signatures were required to approve withdrawals, and the attacker gained control of enough keys to meet this threshold
- **Attribution** — U.S. authorities attributed the heist to the Lazarus Group (DPRK state-sponsored actors)
- **Forensic Lessons** — bridges are attractive targets because they hold large asset pools; security must include people and processes (to counter social engineering), not just smart contract code

### 6. Integrated Investigation Checklist

A professional crypto-crime report should include:
- Exact incident summary (scam vs. hack)
- Detailed flow-of-funds table (date, chain, tx hash, from/to, asset, and observation)
- Off-chain evidence (telecom logs, device imaging, malware analysis)
- Attribution assessment based on official government statements or confirmed technical links

## Part 4: Ethereum Forensics

The forensic complexities of the Ethereum network, moving beyond simple balance checks to understand smart contracts, token standards, and privacy protocols.

### 1. Ethereum Fundamentals and the Account-Based Model

**Core Logic:** unlike Bitcoin's UTXO model, Ethereum uses an account-based model where the network maintains a global "state" of balances and smart contract data.

**Account Types:**
- Externally Owned Accounts (EOAs) — controlled by private keys (user wallets)
- Contract Accounts — controlled by code; they cannot initiate transactions themselves but execute logic when called by an EOA or another contract

**Gas and EVM:** gas measures the computational effort of a transaction, while the Ethereum Virtual Machine (EVM) is the runtime environment that executes contract bytecode.

**Proof-of-Stake (PoS):** Ethereum uses validators who stake ETH to secure the network, which is relevant for investigating validator deposits and rewards.

### 2. Understanding Ethereum Transactions

- **Transaction Fields** — beyond the transaction hash, investigators must examine the From/To addresses, Value (ETH sent), Input Data (function calls), and Nonce (transaction counter)
- **Receipts and Logs** — after execution, a receipt is generated containing event logs; these are critical because token movements (ERC-20/ERC-721) are recorded here rather than in the "Value" field
- **Internal Calls (Traces)** — contracts often trigger "internal transactions" (message calls) to other contracts; these are not independent transactions and must be viewed via execution traces to see the full movement of funds

### 3. Smart Contract Forensics

- **Bytecode and ABI** — contracts exist as bytecode on-chain; the Application Binary Interface (ABI) is required to decode this bytecode into human-readable functions and events
- **Proxies** — many protocols use proxy contracts where the user-facing address delegates logic to a separate implementation contract, meaning the code at the address might not be the code actually being executed
- **Risk Patterns** — common vulnerabilities include reentrancy (repeatedly entering a function before state updates), oracle manipulation, and approval abuse

### 4. Non-Fungible Tokens (NFT) Forensics

- **Standards** — ERC-721 (unique items) and ERC-1155 (multi-token standard)
- **Metadata** — NFTs link to off-chain data (images/JSON) via URI (often IPFS or HTTP); forensic reports must record the token ID and contract address, as names and images can be easily faked
- **Scam Indicators** — rapid transfers following an `ApprovalForAll` event often indicate a wallet-drainer scam

### 5. Tornado Cash and zk-SNARKs

**Mechanism:** Tornado Cash uses zk-SNARKs (Zero-Knowledge Succinct Non-Interactive Arguments of Knowledge) to prove a deposit was made without revealing which one, creating an anonymity set.

**Key Components:**
- Commitment — a public value linked to a secret deposit
- Nullifier — a value revealed during withdrawal to prevent double-spending
- Relayer — a third party that submits the withdrawal to hide the connection between the recipient and gas funding

**Forensic Limitation:** tracing is non-deterministic; investigators must look for timing patterns, amount similarities, or downstream consolidation to find leads.

### 6. Decentralized Finance (DeFi) Tracing

- **Liquidity Pools and AMMs** — Automated Market Makers (AMMs) use liquidity pools where users swap tokens based on mathematical formulas (e.g. x × y = k)
- **Approvals and Routers** — users must approve a router contract to spend their tokens before a swap can occur; tracing a swap requires following the path from the user through the router and various pool contracts
- **Flash Loans** — uncollateralized loans that must be repaid within the same transaction, often used in sophisticated DeFi exploits to manipulate prices

### 7. Ethereum Staking and Validator Investigation

- **Staking Artefacts** — investigations involve mapping execution layer addresses (which fund deposits) to consensus layer validator indices
- **Withdrawal Credentials** — determine where rewards and exited ETH are sent; modern credentials typically point back to an execution-layer address

## Part 5: Privacy Coins and Obfuscation Tactics

How anonymity-enhanced cryptocurrencies (AECs) and behavioral tactics are used to hide transaction details, and how investigators can still find leads through entry/exit points and endpoint evidence.

### 1. Monero (XMR) Fundamentals

**Privacy by Default:** unlike Bitcoin or Ethereum, Monero is designed to hide the sender, receiver, and amount for every transaction.

**Core Privacy Mechanisms:**
- Stealth Addresses — one-time destination addresses generated for every transaction; outsiders cannot link payments to a recipient's public address by searching the ledger
- Ring Signatures — a group of possible outputs appears as the signer of a transaction, making the true source of funds ambiguous to analysts
- Ring Confidential Transactions (RingCT) — mandatory protocol that hides the transaction amounts, preventing amount-based tracking
- Key Images — a mechanism that prevents double-spending without revealing which specific output was actually spent

**Keys and Access:**
- Private View Key — allows visibility of incoming transactions but has limitations for reconstructing outgoing activity or full balances
- Private Spend Key — authorizes the actual movement of funds

### 2. Monero Investigative Strategies

**The Problem:** the on-chain graph is intentionally ambiguous and incomplete for tracing.

**Investigation Workflow:**
- Entry and Exit Points — tracing focuses on where Monero is converted from/to fiat or transparent assets (BTC, ETH, USDT) via regulated exchanges or swap services
- Endpoint Evidence — identifying wallet software, seed phrases, exported keys, or transaction screenshots on seized devices
- Behavioral Mistakes — users may reuse usernames, discuss exact transaction details in chats, or move funds in patterns aligning with external events like ransom demands
- Reporting — investigators must state what is proven versus what is inferred; ring members are "plausible" participants, not confirmed senders

### 3. Zcash (ZEC) Shielding and Deshielding

**Selective Privacy:** Zcash allows users to choose between transparent and shielded activity.

**Address Types:**
- Transparent (t-addresses) — behave like Bitcoin; amounts and relationships are public
- Shielded (z-addresses) — use zk-SNARKs to validate transactions without revealing sender, receiver, or amount
- Unified Addresses — bundle multiple receiver types (transparent and shielded) to simplify usability

**Transaction Visibility Matrix:**
- t → t: fully visible (normal tracing applies)
- t → z (shielding): transparent source and amount entering the pool are visible; receiver is hidden
- z → z (fully shielded): sender, receiver, and amount are all hidden
- z → t (deshielding): receiver and amount exiting the pool are visible; shielded source is hidden

### 4. Obfuscation Tactics

- **Mixers/Tumblers** — services that pool funds from many users to weaken direct links between deposits and withdrawals
- **Chain Hopping** — rapidly moving value across different assets or blockchains through exchanges or bridges
- **Peel Chains** — breaking large funds into many smaller outputs/repeating movements to make tracing tedious
- **Nested Services** — using services with no-KYC or those built on top of other exchanges to reduce the availability of records

### 5. Commercial Analytics Tools

- **Capabilities** — visualizing fund flows, identifying service clusters (exchanges, mixers), and assigning risk scores to addresses
- **Limitations** — coverage varies by chain; attribution is often probabilistic; privacy coins/shielded transactions significantly reduce the data available to these tools
- **Reporting Policy** — tool output should be treated as a lead; use cautious language like "associated with" or "identified by tool as" rather than claiming absolute proof of identity

### 6. Practical Forensic Artefacts

- **Device Artefacts** — browser history, clipboard data, wallet app logs, and screenshots of QR codes
- **Service Records** — KYC data, login IPs, and withdrawal records from exchanges
- **Communications** — ransom notes, marketplace invoices, and chat logs that link a human actor to a specific transaction time or amount

## Part 6: Public Blockchain Explorers and Tracing Tools

The practical use of web-based explorers and specialized graphical software to verify, track, and visualize the movement of assets across different ledger models.

### 1. Introduction to Public Blockchain Explorers

**Definition:** web portals used to view real-time and historical on-chain data such as blocks, transactions, addresses, and smart contracts.

**Forensic Value:**
- Triage and Verification — confirming if a transaction occurred, its status (pending/failed/confirmed), and exact timestamps
- Timeline Building — establishing a chronological sequence of events using UTC timestamps
- Evidence Collection — capturing transaction hashes (txids), block heights, and gas/fee information for reporting

**Limitations:** explorers do not show private keys, KYC records, or real-world identities. Labels (e.g. "Binance") are often third-party intelligence and require independent verification.

### 2. Bitcoin Explorers and the UTXO Model

**UTXO (Unspent Transaction Output):** unlike a bank balance, Bitcoin funds exist as individual "banknotes" (UTXOs). A transaction consumes old outputs (inputs) and creates new ones.

**Change Outputs:** in a transaction, leftover value is typically returned to the sender at a new address. Identifying this "change" is critical for following the true flow of funds.

**Common Tracing Patterns:**
- Peel Chain — a large amount is "peeled" into small payments, with the remainder continuing as change
- Consolidation — multiple inputs are combined into one output, suggesting common control
- Fan-out — funds are split into many outputs, often seen in scam distributions or obfuscation attempts

**Clustering Heuristic:** the assumption that if a transaction has multiple inputs, they are all controlled by the same entity. Caution: collaborative transactions like CoinJoins intentionally break this rule.

### 3. Ethereum Forensics via Etherscan

**Account-Based Model:** unlike Bitcoin, Ethereum tracks state changes in account balances.

**Key Artefacts on Etherscan:**
- Normal Transactions — direct ETH movements initiated by a user
- Internal Transactions — ETH movements triggered by smart contract execution (e.g. a swap)
- Logs and Events — critical for tracing ERC-20 (tokens) and ERC-721/1155 (NFTs); a transaction may have "0 ETH" value but move millions in tokens via logs

**Smart Contract Analysis:**
- Verified Source Code — allows investigators to read the human-readable logic; verification only proves the code matches the bytecode, it does not guarantee the code isn't malicious
- Proxies — many DeFi protocols use proxy contracts that delegate logic; investigators must identify the actual "implementation" contract used at the time of the crime

### 4. Graphical Tracing Tools

**Purpose:** tools (e.g. Chainalysis Reactor, TRM Forensics, Elliptic) convert complex data into visual nodes and edges.

**Advanced Features:**
- Entity Attribution — grouping addresses into "clusters" belonging to known services (exchanges, mixers)
- Pathfinding — automatically finding the shortest route between a victim's wallet and a cash-out point
- Cross-Chain Tracing — visualizing movement through bridges and swaps across different blockchains in one view

**Interpretation:** nodes represent addresses or entities; edges represent the flow of value. Investigators must always verify the underlying transaction hash behind every visual link.

### 5. Forensic Reporting Principles

- **Observation vs. Interpretation** — a report should state "Address A sent funds to Address B" (fact) rather than "The suspect paid the scammer" (interpretation) without corroborating evidence
- **Language** — use cautious terms like "appears consistent with" or "attributed by tool as" when dealing with probabilistic data or heuristics
- **Preservation** — always record the explorer used, the date/time of access, and provide screenshots or raw data exports

---
*Source: coursework revision notes (AI-compiled study notes), Investigating Cryptocurrencies module.*
