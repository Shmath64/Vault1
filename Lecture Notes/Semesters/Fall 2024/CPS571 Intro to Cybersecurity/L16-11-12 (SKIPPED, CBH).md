## Viruses 
- Parasitic, and attack themselves to some existing executable content.
#### Phases:
1. Dormant (not all viruses have this)
2. Propagation (copies itself onto other programs)
3. Triggering phase (is activated)
4. Execution phase (function performed)

#### Classification by Target
- Boot sector infector
	- Infects a master boot record (might spread when system is booted)
- File infector
	- Infects files that have executable permissions
- Macro virus
	- Infects files with macro / scripting code that is interpreted by an application
- Multipartite virus
	- Infects files in multiple ways
#### Classification by Concealment Strategy
- **Encrypted Virus**
	- Enciphered except for a small deciphering routine
	- Sometimes, part of the virus encrypts the rest of itself, this hides its intent, and it can generate/use different keys on different targets as it propagates
- **Stealth Virus**
	- Explicitly designed to hide from antivirus software. 
	- Entire virus (including payload) is hidden
- **Polymorphic Virus**
	- Mutates with every "infection" making it impossible to detect by signature
- **Metamorphic Virus**
	- Similar to polymorphic, but really changes its own behaviour and appearance making detection more difficult
	- Virus itself is also obscured;  two instances of virus would look different 
	- Often, different algorithms that accomplish the same goal

#### Macro and Scripting Viruses
- Infect scripting code used to support active content in a variety of user document types
- Platform independent
- Infect documents, not executable portions of code
- Easily spread (documents can be shared normally)
- Traditional file system access controls aren't as effective

#### Worms
- Programs that actively seek out machines to infect
- Some methods of self-replication are:
	- E-mail, file sharing, remote execution, remote file access, remote login
- Typically same phases as a virus (Dormant, Propagation, Triggering, Execution)

### Target Discovery
- Scanning / Fingerprinting (function in propagation phase, searching for other systems)
Scanning strategies include:
- Random (go after random IP addresses)
- Hit list
- Topological (use info on infected victim machine to find more hosts)
- Local subnet (uses the subnet address structure to find other hosts)

**Drive-by-downloads** exploit browser vulnerabilities and attack when viewing a webpage. (not actively propagating like a worm, just "waits")
**Watering-hole** attacks are similar, but highly targeted. (Infect web sites the target is likely to visit)
**Malvertising** is using ads that incorporate malware.

**Click-Jacking** AKA a **user-interface (UI) redress** attack. 
- Collects a user's clicks
- May involve placing a button under a legitimate button.
- Often involves using transparent/opaque layers to trick users into clicking on things they don't want to.
- Also, keystrokes may be hijacked.

### Payload
- Once malware is active on a system, what will it do?
	- Data destruction? (logic bomb)
	- Display unwanted messages/content
	- Encrypt data and demand payment (ransomware)
	- Inflict real-world damage on the system
		- Rewrite BIOS code
		- Target specific industrial control system software

### Remote Control Facility
- Distinguishes bots (controlled by a facility) from worms (self-propagating)
- Many botnets use covert communication channels via protocols such as HTTP
- Also, *distributed control mechanisms* (peer-to-peer) protocols prevent single-point-of-failures.

### Phishing
- Act of impersonating a legitimate entity in order to obtain sensitive information
- Often *fake* websites 
- More viciously, man-in-the-middle attack
- "Spear phishing" is tailored for specific user.