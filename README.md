_If your looking for the **reference implementation** of the Mumbler Initiative head over to [https://github.com/Mumbler/MumblerAPI](https://github.com/Mumbler/MumblerAPI)._

_If your looking for the **documentation** of the MumblerAPI head over to [https://docs.mumbler.eu](https://docs.mumbler.eu)._


_______



# Mumbler Initiative

Open, secure, simple, authorised-only & web-based communication stack for the web


## Table of contents

-  [Idea / Basic concept](#idea)
-  [Generally](#generally)
-  [Philosophy](#philosophy)
-  [Technical principal](#technical-principal)
   -  [Key generation](#key-generation)
   -  [Authorization](#authorization)
   -  [Encryption](#encryption)
   -  [Decryption](#decryption)
      -  [Request private-key](#request-private-key)
   -  [Additional technical details](#additional-technical-details)
-  [Infrastructure](#infrastructure)
   -  [Limitations](#limitations)
-  [About](#about)
-  [Support](#support)
-  [Code of conduct](#code-of-conduct)

## Idea

The goal of the Mumbler initiative is to resurrect the german law concept of the 
“[Briefgeheimnis](https://de.wikipedia.org/wiki/Briefgeheimnis)” which translates to 
[secrecy of correspondence](https://en.wikipedia.org/wiki/Secrecy_of_correspondence) and is the basis for
privacy of correspondence. For centuries opening and/or reading a correspondence which was intended for 
someone else was a criminal act, often punished by jail time. The french King Louis XV even declared the death 
penalty on breaking the "Briefgeheimnis" (with his edict on 25. Sep 1742).

Today it is a well-known fact that all digital correspondence is stored somewhere and 
is read, analyzed, profiled and last but not least profited off by many parties.

The Mumbler initiative is convinced that it should be of paramount importance to keep the 
information where it belongs:
 
**Only in the hands of the communication parties themselves.**

The Mumbler initiative was created to achieve exactly that. Analog letters have/had the "Briefgeheimnis", 
digital correspondence have Mumbler.

## Generally

No third party must be allowed to read, analyze, profile, profit of (etc...) the digital information transferred. 

Therefore Mumbler ensures that all the relaying servers and networks **must not be able to decipher the information 
transferred under any circumstances and at any given time or state**. 


## Philosophy

The Mumbler Initiative follows three simple but strict rules:

*  keep digital communication simple but secure (everything)
*  access only to those who are intended (including revert the access retroactively)
*  use existing, open and well-proven techniques


## Technical principal

The security and integrity of Mumbler is based on the basic principals of an individual public private key 
infrastructure. These keys get an additional encryption layer on top which enables the usage of the keys from 
any internet browser but technically prohibits the use from any other party which includes the Mumbler 
infrastructure itself.

The client (-browser) initially creates a set of public and private keys to en-/decrypt the various symmetric keys 
which are used to en-/decrypt the actual data blobs. On top the client encrypts the private key with a strong 
derivation function only computable with the users secret credentials. 

The various steps to create, authenticate, en- & decrypt are described below.


### Key generation

These user-keys are only computed once, when the account is generated.
After the computation the keys are stored (additionally encrypted) within the Mumbler infrastructure. 
Therefore the account (and its keys) can be queried from the infrastructure and be used in any browser world-wide. 
Except for the SALT generation, all cryptographic procedures are computed within the clients browser ("locally").

The (initial) key generation process is a six-step process:

1.  Creating an `A-SALT` ("Account-[SALT](https://en.wikipedia.org/wiki/Salt_(cryptography))")
2.  Derive a `authorization-key` as a combination of username and password 
([PBKDF2](https://en.wikipedia.org/wiki/PBKDF2) with A-SALT + n-iterations)
3.  Create an `P-SALT` ("Password-[SALT](https://en.wikipedia.org/wiki/Salt_(cryptography))")
4.  Derive `password-key` from the users password 
([PBKDF2](https://en.wikipedia.org/wiki/PBKDF2) with P-SALT + n-iterations)
5.  Create asymmetric keys and (symmetrically) encrypt `private-key` with derived `password-key` from previous step
6.  Register the encrypted asymmetric key set as well as the random 
[IV](https://en.wikipedia.org/wiki/Initialization_vector) (from the symmetrical encryption process)


### Authorization

The account authorization token is derived using [PBKDF2](https://en.wikipedia.org/wiki/PBKDF2) with n-iterations and 
an additional "Account-[SALT](https://en.wikipedia.org/wiki/Salt_(cryptography))" 
(see steps 1 & 2 of [Key generation](#key-generation) for details). The username + the derived authorization-key is 
used to authorize a login which alone only identifies the user against the infrastructure.

This derived authorization-key can't be used to access the encrypted data. 
It is purely for authentication purposes only.


### Encryption

The encryption of a data blob into a mumbler message object is a four-step process:

1.  Create a random `session-key` as well as a random [IV](https://en.wikipedia.org/wiki/Initialization_vector)
2.  Encrypt the data blob using the `session-key`
3.  Encrypt the `session-key` using the users `public-key` (see step 5 of [Key generation](#key-generation) for details)
4.  Combine encrypted blob data, IV & encrypted `session-key` into mumbler message object


### Decryption

The decryption of a mumbler message object into its original (unencrypted) data blob is a three-step process:

1.  Request the users `private-key` (see [Request private-key](#request-private-key) for details)
2.  Decrypt the `session-key` using the `private-key`
3.  Decrypt the encrypted blob data using the decrypted `session-key` and the IV from the mumbler message object


####  Request private-key

The `private-key` is the most central and secretive aspect of every mumbler session. 
Therefore the `private-key` is held **encrypted during a session**.

If a decryption is requested, the `private-key` will be made available for this decryption process and 
after that its **erased again**. For practicality (and speed purposes) the `private-key` can be held for a few seconds 
after the last request, so multiple subsequent decryption requests can reuse the same `private-key` instance.

Requesting a `private-key` is a four-step process:

1.  Check if session is valid and (still) active
2.  Decrypt the encrypted `private-key` using the `password-key` (see steps 3-5 of 
[Key generation](#key-generation) for details)
3.  Start drop scheduler
4.  After x seconds erase the decrypted `private-key` instance


###  Additional technical details

For details on the Mumbler session management & message object as well as the technical specifications concerning 
"JSON Vulnerability Protection", "XSRF Protection", network flow control, perfect forward secrecy, 
request/response transformations + encodings (etc.) please consider the reference API implementations repository under 
[https://github.com/Mumbler/MumblerAPI](https://github.com/Mumbler/MumblerAPI) or the source code documentation
under [https://docs.mumbler.eu](https://docs.mumbler.eu).


## Infrastructure

Although the data is handled (en-/decrypted, packaged) solely within the clients browser 
Mumbler needs an infrastructure to deliver the data blobs to its designated destination(s).
We therefore operate currently two independent european-based database clusters (Finland & Germany) which are
accessible via Web-API 
(see [https://github.com/Mumbler/MumblerAPI](https://github.com/Mumbler/MumblerAPI) for reference).

Each cluster consists of minimum four independent fully dedicated machines ("nodes"). 
Every bit of data is mirrored between those two locations and every cluster has (at least one) hot-swap node.


### Limitations

To give as many as possible the chance to create and operate accounts on the infrastructure and the
fact, that we currently have no funding for the infrastructure we need to set a limitation on the default account.

These limitations are purely in processing message size and total account size. 
Both factors are heavy on the infrastructure. No other limitations are in place.

The default limitations are as follows:

- Max total account size: **1GB** (1024 * 1024 * 1024 Bytes)
- Max (single) message size: **10 MB** (10 * 1024 * 1024 Bytes)
- Max (single) attachment size: **2 MB** (2 * 1024 * 1024 Bytes)

As shown above, a message can have a maximum size of 10MB. This is calculated on the basis of the complete message object 
(which includes all body and attachment payloads). 
A single attachment can have a maximum size of 2MB and the overall total account size, including all
meta data (folders etc.) can't exceed 1GB.


_We know this is not very much but please be patient. 
As soon as we have a **solid infrastructure funding** we will expand those numbers accordingly._

(We're currently in the progress of setting up a Indiegogo campaign to help found the infrastructure costs. In the 
meantime it would be great if you could [support our efforts](#support)).

If you need more now, please [contact us directly](mailto:webmaster@mumbler.eu). We will figure something out together.

## About

Mumbler was born as an idealist idea to apply the principals of the "Briefgeheimnis" to the digital age. 
We are very low on budget and are currently solely founded through and legally constituted 
within the austrian-based startup company MagnusIT GmbH.

We're currently in the progress of setting up a Indiegogo campaign to help found the infrastructure costs. In the 
meantime it would be great if you could support our efforts by 
[donating a couple of euros](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=EZ2DJPABLJS6J).


## Support

Please support the project with your coding-, design-, brain-time and/or spread the word.

Of course **operating the infrastructure is a big issue** as well. 
It would be great if you could support us by donating a couple of euros:

[![](https://www.paypalobjects.com/en_US/i/btn/btn_donateCC_LG.gif)](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=EZ2DJPABLJS6J)

Although the project has its fully implemented reference API 
(see [https://github.com/Mumbler/MumblerAPI](https://github.com/Mumbler/MumblerAPI)) 
and a fully operational (productive) infrastructure (see [Infrastructure](#infrastructure) for details) 
it still lacks a good UI-Implementation.

We've developed a test suit but nothing fancy in the field of UI-design. 
It would be great if you could support use in that area (as well).

**Thank you very much and we'll hope you’re as excited as we are on the idea of bringing back the “Briefgeheimnis” 
to the digital age.**


## Code of conduct

Lust but not least please read the [COC.md](COC.md) file for details on our code of conduct. Contributing to the
Mumbler Initiative you agree to our code of conduct.
