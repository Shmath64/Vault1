
- You don't *really* log into different systems with a single password , instead the service contacts another service that knows you and you log in there.
But how do systems know who is who?

We need more than just a digital signature (proof that the key was used)
We need a trusted third party
- Someone trusted by everyone involved in a protocol to help complete the protocol fairly and securely


### Certificate Authorities (CAs)
Have an *issuance policy* which determines to which principals the CA will issue certificates.

*Verisign* had 4 classes in increasing seriousness.
First 3 are for individuals, fourth is for web servers.
1. Sending and receiving email,
2. Online purchasing
3. Background checks
4. Web sites that must not be compromised

### Registration Authority (RA)
- Is a third party, delegated by CA
- Has the authority to check data to be put into certificate
- Determines whether CA's requirements are met and lets CA know

### Internet Certification Hierarchy
Is a tree arrangement of CA's
Root is the *Internet Policy Registration Authority* or ***IPRA***
This:
- Sets policies for all CAs
- Certifies subordinate CAs (these are called *policy certification authorities (PCAs)*)
	- Each PCA has its own authentication and issuance policies
- Does not issue certificates to individuals or organizations (just to subordinate CAs)

Policy Certification Authorities (PCAs) can be high or low assurance depending on the required level of security.

If a principal's private key is compromised/lost, their certificate can be put on a **Certificate Revocation List (CRL)**

## Problems with all this:
- No single organization that everyone trusts
- We need to use hierarchies of trust to establish validity (all traces back to a root)
- Root Certificates are not signed by anyone, they are embedded in software (Browser, FTP, etc.)

#CBH Finish Key generation and certificates!
