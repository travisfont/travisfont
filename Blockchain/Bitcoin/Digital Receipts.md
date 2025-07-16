# Digital Receipts

## Introduction to Georgia’s Bitcoin-Based Land Registry 

The **NAPR (National Agency for Public Registry)** of Georgia used the **Bitcoin blockchain** to store digital property titles and receipts.
This initiative was part of a collaboration with **BitFury** to enhance transparency and security in land registry records.  

By anchoring hashes of property transactions into the Bitcoin blockchain, Georgia ensured an immutable and verifiable record of ownership.
This approach leveraged Bitcoin's robust security and decentralization to prevent fraud and streamline bureaucratic processes.  

## Overview to Georgia’s Bitcoin-Based Land Registry

The **National Agency of Public Registry (NAPR)** of Georgia, in partnership with **BitFury** and the **Blockchain Trust Accelerator**, implemented a groundbreaking system to store **digitized land titles** on the **Bitcoin blockchain**. 

### **Why Bitcoin?**
   - Bitcoin’s blockchain was chosen due to its **immutability, security, and decentralization**.
   - Unlike private or permissioned blockchains, Bitcoin offers **tamper-proof** records without relying on a single authority.

### **How It Worked**
   - **Step 1: Digitizing Land Records**  
     Property titles and transactions were recorded in NAPR’s database.
   - **Step 2: Hashing the Data**  
     Each record was cryptographically hashed (converted into a fixed-size string of characters) using **SHA-256**.
   - **Step 3: Anchoring to Bitcoin**  
     The hashes were embedded into Bitcoin transactions via **OP_RETURN** (a feature allowing small data storage).  
     - This did **not store the full document** but created an **immutable proof** of its existence at a specific time.
   - **Step 4: Verification**  
     Anyone could verify a property record by:
     1. Checking the NAPR database.
     2. Confirming the hash matches the one stored on Bitcoin’s blockchain.

### **Benefits**
   - **Fraud Prevention** – Tampering with land records became nearly impossible without detection.
   - **Transparency** – Citizens could independently verify property ownership.
   - **Cost & Efficiency** – Reduced bureaucracy and paperwork.

### **Timeline & Impact**
   - **2016-2017**: Pilot phase began, registering over **160,000 land titles**.
   - **2018**: Expanded to cover most of Georgia’s land registry.
   - **Global Influence**: Inspired similar projects in **Sweden, Ukraine, and India**.

### **Why Not a Different Blockchain?**
   - **Ethereum/Smart Contracts?** – Bitcoin was chosen for its **security** and simplicity (no need for smart contracts).
   - **Private Blockchain?** – Would lack Bitcoin’s **decentralized trust** model.

### **6. Current Status**
   - The system remains active, with new property transactions still being anchored to Bitcoin.
   - Georgia is considered a **global leader** in blockchain-based land registry innovation.

## How Georgia Used Bitcoin’s OP_RETURN for Land Registry


The NAPR (National Agency of Public Registry) of Georgia, in collaboration with **BitFury**, used Bitcoin’s **OP_RETURN** function to anchor land title hashes securely.

---

### **1. What is OP_RETURN?**  
- A Bitcoin scripting opcode that allows **80 bytes of arbitrary data** to be embedded in a transaction.  
- Unlike storing full documents, it’s used for **proof-of-existence**—just a cryptographic fingerprint (hash) of the data.  
- Transactions with OP_RETURN are **provably unspendable**, meaning they don’t clutter Bitcoin’s UTXO set.  

---

### **2. Step-by-Step Process**  

#### **Step 1: Digitizing Land Records**  
- Each property title was assigned a **unique digital record** in NAPR’s database.  
- Critical details (owner name, parcel ID, transaction history) were stored **off-chain** (in NAPR’s servers).  

#### **Step 2: Generating a Cryptographic Hash**  
- The land record was hashed using **SHA-256**, producing a **unique 64-character string** (e.g., `a1b2c3...`).  
- Even a tiny change in the document would produce a **completely different hash**.  

#### **Step 3: Embedding the Hash in Bitcoin**  
- BitFury’s **Exonum** (a blockchain framework) bundled multiple hashes and periodically anchored them to Bitcoin via **OP_RETURN**.  
- Example transaction:  
  ```
  OP_RETURN 4e415052 789a6f4d2e... (NAPR prefix + hashed data)
  ```
  *(The "4e415052" is "NAPR" in hexadecimal.)*  

#### **Step 4: Verification**  
- To confirm a land title’s authenticity:  
  1. Retrieve the document from NAPR’s database.  
  2. Generate its SHA-256 hash.  
  3. Check if this hash exists in a Bitcoin block (using a **block explorer** like [Blockstream.info](https://blockstream.info)).  

---

### **3. Why OP_RETURN and Not Another Method?**  
- **Cheaper than full on-chain storage** (only ~40-80 bytes per transaction).  
- **More secure than sidechains** (directly inherits Bitcoin’s security).  
- **No smart contract risk** (unlike Ethereum, where bugs could affect records).  

---

### **4. Other Countries Using Blockchain for Land Registry**  
| **Country**  | **Blockchain Used** | **Status** | **Key Partner** |
|-------------|--------------------|------------|----------------|
| **Georgia** | Bitcoin (OP_RETURN) | Live since 2016 | BitFury |
| **Sweden**  | Private Ethereum-based | Pilot (2017-2020) | ChromaWay |
| **Honduras** | Bitcoin (Planned, stalled) | Canceled | Factom |
| **Ghana**   | Custom blockchain | Pilot (2018) | Bitland |
| **Ukraine**  | Bitcoin + Ethereum | Pilot (2021) | BitFury |

---

### **5. Challenges & Lessons Learned**  
✅ **Successes**:  
   - **No successful fraud cases** reported since implementation.  
   - **Faster transactions** (reduced manual verification).  

⚠️ **Challenges**:  
   - **Scalability**: Bitcoin’s block size limits how much data can be stored.  
   - **Dependence on Off-Chain Data**: If NAPR’s database is compromised, hashes alone aren’t enough.  

---

### **6. Could This Work with Lightning Network or Taproot?**  
- **Lightning**: Not suitable (needs on-chain anchoring).  
- **Taproot**: Could improve efficiency but doesn’t change the core OP_RETURN model.  

---

### **Final Thoughts**  
Georgia’s system proves that **even Bitcoin’s "limited" scripting can revolutionize bureaucracy**. =
Other governments are watching closely—especially those with **weak property rights** or **corruption issues**.  
