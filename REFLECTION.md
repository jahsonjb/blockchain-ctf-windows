Got it JB ✅ — here’s a polished `REFLECTION.md` draft for your **Windows-only Blockchain CTF build**. I’ve kept the format portfolio-ready, with headings, flow, and emphasis on *why* as much as *what*. You can drop this straight into your repo.

---

```markdown
# 🔐 Windows-Only Blockchain CTF — Reflection

**By: Jahson Jno-Baptiste — a note to future me**

---

## Why I Built This

Future me, here’s the full story of the Windows-only Blockchain CTF build—why I scoped it the way I did, what I actually did step by step, and what I’d keep or change next round.  

The point wasn’t “blockchain programming.” It was **chained problem solving**: each block provides material to unlock the next, ending with a clean AES decrypt and the flag. This lab was designed to be **approachable on stock Windows**, reproducible without extra environments, and useful as a portfolio artifact.

---

## Toolchain & Design Choices

- **Windows-first**: No WSL, no compilers, no package managers. Built to run in a classroom or student machine without friction.  
- **OpenStego**: Simple GUI for hiding/extracting files in PNGs.  
- **CyberChef**: Reliable SHA-3-256 hashing and AES-256-CBC with explicit field formats.  
- **PowerShell**: Quick base64 decode and file integrity hashing.  
- **PNG images**: Chosen over JPG to avoid compression artifacts and ensure exact stego extraction.  

The thematic "blockchain" was metaphorical: Block 1 → Block 2 → Block 3, where each step depended on the last.

---

## Architecture of the Blocks

### Block 1 — On-Ramp
- Hidden file: `1.txt` with base64 string `Q3liM3JDaDRsbDNuZzM=`.  
- Stego passphrase: `StartHere`.  
- Decoded value: `Cyb3rCh4ll3ng3` (used as Block 2 password).  
- Lesson: Prove stego pipeline works + base64 fundamentals.

### Block 2 — Raising the Ceiling
- Hidden file: `2.txt` with instructions:  
  > Compute SHA-3-256 of the exact phrase `ZeroTrust`. Use the 32-byte digest as AES-256 key. IV = all zeros.  
- Digest produced:  
```

41ef59838c9ede053273aba142bf711c20407d68089c5a637f177afe33a9a64e

```
- Lesson: Reinforce exact inputs (no trailing spaces/newlines) and proper field formats.

### Block 3 — Payoff
- Hidden file: `3.enc` (AES-256-CBC ciphertext of flag).  
- Stego passphrase: `ZeroTrust`.  
- Decrypt in CyberChef with:
- Key = SHA-3 digest (hex)  
- IV = 32 zeros (hex)  
- Mode = CBC, Padding = PKCS7  
- Output flag:  
```

FLAG{U\_F0und\_Th3\_53cr3t}

````

---

## Authoring Notes

- Staged in `C:\CTF` with consistent filenames (`SecPlus1.png`, `SecPlus2.png`, `SecPlus3.png`).  
- Embedded message files with OpenStego → Data Hiding.  
- Verified each step immediately with Data Extraction + PowerShell/CyberChef.  
- Published SHA-256 hashes via `Get-FileHash` for reproducibility:
```powershell
Get-FileHash .\SecPlus1.png -Algorithm SHA256
````

---

## Verification Pass (Solver Mode)

1. **Block 1**: Extract → decode → get next password.
2. **Block 2**: Extract → follow instructions → compute SHA-3 digest → prep AES key.
3. **Block 3**: Extract → AES decrypt with digest + zero IV → reveal flag.

Screenshots of extraction, hashing, decryption, and Explorer views were captured for clarity in `report-assets/`.

---

## Pitfalls & Fixes

* **SHA-3 vs Keccak**: Always select **SHA3-256** (not Keccak-256). Wrong choice = wrong key.
* **Whitespace sensitivity**: Phrase must be exactly `ZeroTrust`.
* **AES fields**: Ensure Key/IV fields are set to **Hex**, not Text.
* **Stego tab mix-ups**: Always check whether you’re in *Data Hiding* or *Data Extraction*.

---

## Why a Zero IV and Simple Key Derivation?

This is a **training puzzle**. The fixed IV keeps focus on hashing → AES-CBC flow, not protocol nuance.
In real-world crypto: random IV + KDF (PBKDF2/Argon2/HKDF) + AEAD (e.g., GCM) would be required.

---

## Publishing Like a Professional Project

* **Badges**: MIT license, maintained, platform: Windows.
* **Repo layout**: `README.md`, three PNGs, optional checksums.
* **README sections**: Overview, How to Solve, Troubleshooting, Integrity Hashes, Authoring Notes.
* **Screenshots**: Consistent, planned shots for clear storytelling.

---

## Lessons Learned

* **Constrain the toolchain** → accessibility goes up.
* **Be explicit with formats** → hex vs text confusion is the #1 solver trap.
* **Verify like a player** → authoring isn’t enough; solving proves documentation.
* **Ship integrity hashes** → builds trust and reproducibility.
* **Screenshots matter** → clarity improves adoption.

---

## Next Iteration Ideas

* Add a **Block 4** with a tiny brute-force salt or hash-prefix search.
* Switch final crypto to **AES-GCM** for authenticity.
* Add a lightweight **PowerShell verifier** for image hashes and automated checks.
* Integrate **CI tests (Pester)** to validate artifacts.

---

## Takeaway

This build was about **clear thinking and clean execution**:
Stego → Base64 → SHA-3-256 → AES-256-CBC.

Approachable, reproducible, and reliable on stock Windows. If you ever need to rebuild it quickly, start a folder at `C:\CTF`, drop in the three PNGs, embed the message files with the known passphrases, verify the chain end-to-end, and publish fresh hashes.

The puzzle isn’t about cleverness; it’s about discipline—and that’s why it belongs in your portfolio.
