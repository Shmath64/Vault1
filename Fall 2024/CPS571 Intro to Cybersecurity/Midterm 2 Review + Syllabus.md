Syllabus:
Nov. 19, 8:10, 1h30m, TRS2147

 1. Cryptographic protocols: 27 slides  
      Ch. 3.7,  4.1, 4.2, 4.3,  4.6, 4.10, 4.11, 23.1, 23.2,  23.4,  Schneier, Bruce, Applied Cryptography
  2. Security protocols and policies:  24 slides.  
      Ch 7, Ch 8, Bruce Schneier. Secrets and lies: digital security in a networked world

  3. Digital signatures : 25 slides  
    Ch 10.5 in Mat Bishop, Computer Security: Art and Science   
    Some details are taken from Applied Cryptography by B. Schneier, 2, 2.8 , 23.2   
    but the lecture notes also provide sufficient information.
    
 4. Identification and authentication  without Kerberos 86 slides
---
## 1. Cryptographic Protocols
"Multiple public key cryptography" e.g. MK-RSA (MK =  Multiple Keys)
Like RSA, but multiple keys, $\prod^t_{i=1} K_i\ mod\ \phi(n) = 1$
- $K_1 * K_2 * ... * K_t\ mod \phi(n) = 1$
Can be signed by t-1 people
Message may be encrypted with some keys and decrypted with others;
- use case: multi-signatures, good for a third party verifying signatures


##### Time Stamp Service by Trusted Third Party Trent
Requires:
- Data must be timestamped without regard for physical medium
- Must be impossible to change a single bit without change being apparent
- Must be impossible to timestamp a document with not-current date and time

This is better if:
1. Alice transmits one-way-hash of the document to Trent
2. Trent appends the date and time and digitally signs the result 
3. Trent sends the signed hash with timestamp back to Alice.

#### Group Signatures (emphasises usual privacy)
Used to control excessive use of resources and sending warning messages about road conditions (in vehicular networks)
- Trent generates all public/private key pairs, makes sure they're private,
- Publishes master list of all public keys for the group.
In case of a dispute, Trent can check which public key corresponds to which group member.


***Undeniable Digital Signature***

"Self-Enforcing" means:
- Either party can immediately detect the other's cheating
- No trusted third party is needed.

#### Secret Sharing ("Shared Secret")
Enforce *requirements for collusion* in access. (think 2 keys for missile silo)
- Secret Key is divided into $n$ parts called *shadows*. To recover, $m$ shadows are needed.
Uses "La Grange Interpolating Polynomial" scheme proposed by Adi Shamir.
- Prime P larger than possible shadows and possible secret, P is public.
- If we want to have $m$ shadows controlling the secret, we need a polynomial of degree $m-1$
- $m=3: (ax^2 + bx + M)\ mod\ p$ where M is the secret
- Shadows are obtained by evaluating the polynomial in $m$ points (substitute $x$ for an integer for each shadow)
- To reconstruct $m$ we create a system of equations and solve for A, B, and M
- "(3, 5) Threshold Scheme" means using 5 keys total but 3 are needed to decrypt (so degree 2 polynomial)

**Protocols** are combinations/implementations of:
- Symmetric encryption,
- Message authentication codes,
- Public-key encryption,
- One-way hash functions,
- Digital signature schemes,
- Random number generators.

## 2. Security Protocols and Policies
Types of attacks:
- Passive (Listen and Learn)
- Active (inserting, deleting, modifying, etc)
	- Includes **Man-in-the-middle attack**

No objective measure to compare protocols.
Old and public is preferred to new and proprietary
"Security comes from following the crowd"
"Integrity" refers to making sure the data is *exactly* how the last authorized modifier left it.
This largely boils down to "access control"

"Subjects" try to access "Objects"

#### Bell-LaPadula model:
"Need to know principle", "Read down AND write up"
This concentrates on confidentiality at everything else's expense
Also it ignores the issue of changing classifications

"Separation of duty": separate developer and installer
"Separation of function": developer can not develop software on end user's system. And cannot use end user's data (must be sanitized)
"Chinese Wall Model" emphasizes integrity policy
"Clark-Wilson Model" also is for data integrity (made for commercial applications)

#### Security Kernels (OS security)
Many OSs have built-in security. (Often is best to place security at the lowest layer). Because:
1. Resembles Bell LaPadula policy (no write down and no read up)
2. Often possible to compromise security at a given layer by attacking a layer below
3. Simpler to add security at the core of the system
4. Often faster.

Components of OS security:
- **Reference monitor**: when making an OS call, the reference monitor halts the process and determines whether to allow the call
- **Trusted computing base**: Includes all protection mechanisms inside the computer (hardware, firmware, OS, software applications, everything)
- **Secure Kernel**: Hardware, firmware, OS, applications; everything of the trusted computing base that implements the reference monitor concept.


## 3. Digital Signatures (Applied Cryptography)
- Symmetric key cryptography is best for encrypting data (its faster and has shorter keys)
- Public key cryptography is can provide authenticity and integrity of data in protocols which involve large number of participants.

**Digital Signatures** are analogous to hand-written signatures and MUST: authenticate the **origin** and **contents** of the message to a **third party** 
- Verifiable, non-forgeable, AND non-repudiable. 
	- Therefore, not reusable, it is a function of the document.

"Classical Digital Signatures" is just each user shares a private key with trusted third party (Cathy). Users then can check with Cathy if signatures are valid.

#### Public Key Digital Signatures based on RSA
- Bob signs $m$ by encrypting with his private key $K^-_B$, creating "signed message" $K^-_B(m)$. 

"Digital Signature = signed message digest"
##### Key Points:
- Never sign random documents, always sign *hash* and never *document*
	- This is due to mathematical properties of RSA
- Always sign message first, then encipher
- Use separate pairs of keys for signing and encryptions.
	- Longer keys for signing than encrypting
- RSA encryption is "homomorphic" (can do operations on encrypted text)

When signing random documents:
$(m_1^{db} mod\ n_B)(m_2^{db}mod\ n_B) = ((m_1m_2)^{db}mod\ n_B)$
This means that if Alice can get Bob to sign arbitrary documents, she can forge his signature on other documents.
By getting Bob's signatures on two messages, we can calculate what Bob's signature is on a third message that is the product ($mod \ n_B$) and "prove" he signed it.

If Alice signs document A, but Bob wants her to sign B, he can change his public key and find value $r$ such that, $M_B^r\ mod\ n_B = M_A$ 
To prevent this, include sender and recipient information in the message, changing recipient invalidates the signature.

#### El Gamal Digital Signature
Choose P prime and $g, d < P$
$y = g^d\ mod\ p$
To sign contract $m$:
- Choose random $k$ that is relatively prime to p-1
- Public key is: $(y, g, p)$ and private key is $d$
- $a = g^k\ mod\ p$ 
- Find $b$ such that:
	- $m = (da + kb)\ mod\ (p-1)$
	- This can be done using Euclidean algorithm
	- Remember: $\phi(p) = p-1$
- The signature is the pair: $(a, b)$

To validate signature, check that:
$y^aa^b \ mod\ p = g^m \ mod\ p$

If Eve learns k, m, and signature (a,b), she can use use the Extended Euclidean Algorithm to calculate $d$, the **private key**.

To use a **subliminal channel** in the El Gamal digital signature, it lies in "random" number $k$,

#### Key Points:
- Classical (use same key, or derive one key from another) and public key (encipher and decipher using different keys) cryptosystems
- Cryptographic checksums are used to check **integrity**

### 4. Identification and Authentication
Typically based on something only you:
- Know (passwords)
- Have (tokens)
- Are (biometrics)
Sometimes even where you are or what time it is
(Or any combination of the above - **Multi-Factor Authentication (MFA)**)

"Dictionary attack" is when an attacker calculates the hash of common passwords and compares it against a database

Challenge/Response systems can slow down attacks

##### S/Key
One-time password scheme based on idea of **Lamport**
using hash function $h$
User chooses initial seed $k$:
Key generator calculates hashes starting with $k$
$h(k) = k_1,\ h(k_1) = k_2 ... h(k_{n-1}) = k_n$
Order of passwords are in reverse order of $k$
This makes dictionary attacks very difficult!

#### HMAC-based One-Time Password Algorithm (HOTP)
- Uses counter rather than iterated hash functions,
- Uses HMAC-SHA1 to hash shared secret key $k$ and 8-byte counter $c$ (synchronized)
Process:
- $h =$ HMAC-SHA-1$(k,c)$; h is 20 bytes long
....


##### Computer-Computer Issues:
Threats include:
- Pretending to be another user
- Altering the network address of a workstation
- Eavesdropping (leading to replay / man-in-the-middle attacks)
- Impersonating a server and offering a modified/malicious service

We can use **Nonces** (numbers used **Once**)
These can used with public and symmetric keys
However, still vulnerable to man-in-the-middle

How do two entities establish shared secret key over a network?
a **Key Distribution Centre**
- Shares unique secret key with each registered users,
- Generates R1 which Alice and Bob can use as *session key*
- 

How does one know that a public key is really that person's public key?
a trusted **Certification Authority (CA)**

#### Needham-Schroeder Key Exchange
Involves Nonces and a trusted third party (Cathy)
1. Alice requests key to communicate with Bob from Cathy (and sends a Nonce)
2. Cathy replies with their names, session key ($k_s$), and $\{Alice || k_s\}k_b$ 
3. Alice sends this last part to Bob,
4. Bob replies with a new encrypted Nonce
5. Alice replies with encrypted Nonce-1

**Denning-Sacco Modification**
- Lets say Eve can obtain session key ($k_s$) 
This allows Eve to impersonate Alice and "replay" messages
To solve, lets incorporate a timestamp (T) to detect replay attacks
This can be included in the message containing the session key that was sent from Cathy to Alice, then from Alice to Bob (all encrypted with $k_b$)

#### Encrypted Key Exchange (EKE)
- Defeats offline dictionary attacks on challenges

"Diffie Hellman EKE"

##### Otway-Rees Protocol
Corrects the issue of Eve replaying third message in Needham Schroeder
Doesn't use timestamps (removes issues with Denning-Sacco modification)
Uses integer $n$ as a kind of sequence number to associate all messages with particular exchange. This is the "session sequence number"

##### Bellare-Rogaway (B-R) Protocol
- Focuses on symmetric key exchange
- Uses keyed hash functions to exchange session keys