# 🧩 Bitcoin ECDSA Signature Extractor & Analyzer  

This utility extracts **ECDSA signature components (r, s)** from raw transaction hexadecimal data  
using **regex-based DER decoding**. It allows researchers to isolate signature values for deeper  
ECDSA vulnerability analysis (e.g. nonce reuse, identical r detection, or malformed DER checks).  

> ⚠️ **For educational and security research only.**  
> This tool is designed for **on-chain cryptography analysis**, not unauthorized data extraction.

---

## ⚙️ Features  

✅ Parses **DER-encoded ECDSA signatures** directly from raw hex data  
✅ Uses **regex pattern matching** to locate valid DER sequences  
✅ Extracts and prints **r**, **s**, **z**, **txid**, and **SIGHASH flags**  
✅ Works with **bech32** (bc1…), **P2PKH**, and **P2SH** address formats  
✅ Provides clean, human-readable structured output  
✅ No external dependencies — pure Python implementation  

---

## 📂 File Structure  

| File | Description |
|------|-------------|
| `ecdsa_signature_extractor.py` | Main signature extraction script |
| `Nowy Dokument tekstowy (3).txt` | Input file containing transaction hex |
| `README.md` | This documentation file |

---

## 🧮 How It Works  

1️⃣ **Load Hex Data**  
The script opens a text file containing raw Bitcoin transaction hex data.  

2️⃣ **Find Signatures via Regex**  
A precompiled regex pattern locates valid **DER-encoded signatures**:
30 LL 02 LR RR… 02 LS SS…

where `LL` = total length, `LR` = length of r, and `LS` = length of s.

3️⃣ **Parse r and s Values**  
Each match is decoded via `parse_der()`:
```python
def parse_der(sig_hex):
    r_len = int(sig_hex[6:8], 16)
    r_val = sig_hex[8:8+r_len*2]
    s_marker_index = 8 + r_len*2
    s_len = int(sig_hex[s_marker_index+2:s_marker_index+4], 16)
    s_val = sig_hex[s_marker_index+4:s_marker_index+4+s_len*2]
    return r_val, s_val


4️⃣ Print Structured Output
Each parsed signature is displayed with the provided context:

Adres: bc1qcpflj68s3ahy4xajez4d8v3vk28pvf7qte2jmlftvxzfke2u6mqsge3gvh
r = f8a1b5c089a0…
s = 1a25b43201e9…
z = f8a1b5c089a029321f14b24b1a25b43201e94c82cd5fa9ef59e8b345cf8adab3
SIGHASH_FLAG = 94
txid = 20644af039cc383818a504bd5822ef0ff7cd22773a8b22dd8f86677bcfad1e36
Podatności: Reuse k w różnych podpisach
----------------------------------

🧠 Example Output
Znaleziono podpisów: 3
Zparsowano podpisów: 3

Adres: bc1qcpflj68s3ahy4xajez4d8v3vk28pvf7qte2jmlftvxzfke2u6mqsge3gvh
r = 8c0a12e9...
s = 4e92ab7d...
z = f8a1b5c089a029321f14b24b1a25b43201e94c82cd5fa9ef59e8b345cf8adab3
SIGHASH_FLAG = 94
txid = 20644af039cc383818a504bd5822ef0ff7cd22773a8b22dd8f86677bcfad1e36
Podatności: Reuse k w różnych podpisach
----------------------------------

⚙️ Parameters
Variable	Description	Example
address	Bitcoin address being analyzed	bc1qcpflj68s3ahy4...
z	Message hash (double SHA-256 of transaction)	f8a1b5c0…
SIGHASH_FLAG	Signature flag used in Bitcoin script	94
txid	Transaction ID associated with signature	20644a…
hex_data	Input hex data read from file	(Raw transaction)
⚖️ Ethical Disclaimer

This project is created for cryptographic transparency and ECDSA research.
It should only be used to analyze public blockchain data or your own wallet transactions.

Do ✅:

Study signature encoding structures

Identify nonce reuse patterns in test data

Verify your own transaction safety

Do ❌:

Attempt to extract private keys or exploit others’ transactions

🪪 License

MIT License
© 2025 — Author: [ethicbrudhack]

BTC donation address: bc1q4nyq7kr4nwq6zw35pg0zl0k9jmdmtmadlfvqhr
