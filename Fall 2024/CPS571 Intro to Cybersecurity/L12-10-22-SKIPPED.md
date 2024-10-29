Identification and Authentication

"Access control" is needed to allow intended users in, and others out.
Based on things,
- You know (passwords)
- You have (physical key)
- You are (biometrics)

Username is identification, password is authentication

Access tokens are a key. Think of hotel access cards.

In IAM, there are challenge/response systems,
These can include things like passwords, biometrics, etc. but also think of Captcha!

Time and location can also be used as a security factor. (Call for an extra level of authentication if time/location is unusual)

This has all been about people identifying and authenticating themselves, what about computers' interactions with other computers.

Threats include:
- Imposters
- Altering network address of workstations
- Eavesdropping; man in the middle attacks
- Impersonating a server and offering (potentially malicious) servicers.

To send a message that claims to be from another IP address is "**IP address spoofing**"
**Playback attack** means replaying a message sent by someone else (this can be used on sent encrypted passwords of course!)

To avoid playback attacks, we use a **Nonce** (a number R) used once-in-a-lifetime
To prove Alice really is Alice, she:
1. Identifies herself ("I am Alice")
2. Receives a **nonce** (R) from Bob (sometimes this is called a "challenge")
3. Returns $K_{A-B}(R)$ (shared secret key) to Bob

We can also do this with public key cryptography!
But unfortunately this is STILL vulnerable to a **Man-in-the Middle Attack**!
Trudy can pose as Alice to Bob, and pose as Bob to Alice (and pass everything along, or change data).
This can be difficult to detect
#### Key Management tips:
- Session and Interchange (Master) keys.
	- We have "master keys" to protect "session keys". Similar to asymmetric keys to communicate and protect symmetric keys.
- Limit amount of traffic enciphered with single key

##### Trusted Certification Authority (CA)
### Key Distribution Centre (KDC)
(not as popular anymore). While secure, it is not very scalable.
Can be used to act as intermediary to establish shared secret keys.
- The server shares different secret keys with EACH registered user
	- This is used to establish encrypted communication with KDC

### Notation for Key Exchange Algorithms
$X \implies Y: \{Z || W\}K_{X,Y}$
- X sends Y the message produced by concatenating Z and W enciphered by key $K_{X, Y}$ (shared between X and Y)
$X \implies Y: \{Z\}K_X || \{W\}K_{X,Y}$
- X sends Y message Z (encrypted with X's key) concatenated with W (encrypted with key shared by X and Y)
$r_1$ is a nonce

(Always assume everything transmitted is seen by attackers)

**Classical Symmetric Key Exchange** is done by a trusted third party (Cathy). Alice and Bob communicate the keys from there.
### Needham-Schroeder
![[Pasted image 20241024154707.png]]
**nonce**s are helpful for ensuring messages are "fresh" and not some replay attack
We can write out the Needham-Schroeder protocol like so:
1. $A\implies C : A||B|r_1$ ; Alice sends to Cathy, herself, who she wants to talk to and a nonce
2. $C\implies A: \{A||B||K_{ab}||r_1||\{K_{ab}||A\}K_{bc}\}K_{ac}$
	Cathy responds with Alice, Bob, their shared key, the nonce and (Alice and shared key encrypted with Bob's key) all encrypted with Alice's key
3. $A\implies B: \{K_{ab}||A\}K_{bc}$
For the last couple steps, it's important that its done quickly (so that Trudy isn't messing with things)
1. $B\implies A: \{r_2\}k_{ab}$
2. $A\implies B : \{r_2-1\}k_{ab}$

Next up:
Denning-Sacco Modifcation
