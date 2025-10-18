### **1. Hashing**

**Purpose:** Verify integrity (not for hiding or retrieving data).
**One-way:** You can’t reverse it.

**What happens:**
A piece of data (like a password or file) is passed through a hash function (like SHA-256 or bcrypt), which produces a fixed-length string — the *hash*.
Even a small change in input completely changes the output.

**Example:**

```
Input: "hello"
SHA-256 → 2cf24dba5fb0a... (a 64-char hash)
```

If someone sends you the hash, you can check if the original data *matches* (by hashing your version and comparing).
But you can’t get “hello” back from that hash — it’s irreversible.

**Use cases:** Password storage, digital signatures, data integrity checks.

---

### **2. Encoding**

**Purpose:** Make data usable or readable by another system.
**Two-way:** You can decode it back.

**What happens:**
Encoding changes data into a different format using a known scheme — no secret involved.

**Example:**

```
Input: "hello"
Base64 → aGVsbG8=
Decode Base64 → "hello"
```

**Use cases:**

* Transferring binary data as text (e.g., images in JSON, URLs)
* Data serialization (like converting to Base64 or ASCII)

**Not secure at all.** Anyone can decode it.

---

### **3. Decoding**

**Purpose:** Reverse of encoding.
**What happens:**
You just take the encoded string and convert it back to the original using the same algorithm.

**Example:**

```
Encoded: aGVsbG8=
Decoded: hello
```

---

### **4. Encryption**

**Purpose:** Protect confidentiality (hide data from unauthorized people).
**Two-way, but secure:** You can decrypt it only with the right key.

**What happens:**
Encryption uses an *algorithm* + *key* to transform data into unreadable ciphertext.
Decryption reverses it with the same or corresponding key.

**Example:**

```
Plaintext: "hello"
Encrypt with key → "Xh29@#s2"
Decrypt with key → "hello"
```

**Types:**

* **Symmetric encryption:** Same key for encryption and decryption (e.g., AES).
* **Asymmetric encryption:** Public key encrypts, private key decrypts (e.g., RSA).

**Use cases:** Secure communication (HTTPS), data storage, emails, etc.

---

### ✅ **Quick Summary Table**

| Concept        | Direction           | Reversible?  | Purpose                      | Example              |
| -------------- | ------------------- | ------------ | ---------------------------- | -------------------- |
| **Hashing**    | One-way             | ❌            | Verify data integrity        | Password storage     |
| **Encoding**   | Two-way             | ✅            | Data format conversion       | Base64, URL encoding |
| **Decoding**   | Reverse of encoding | ✅            | Retrieve original data       | Base64 decoding      |
| **Encryption** | Two-way (secure)    | ✅ (with key) | Protect data confidentiality | AES, RSA             |
