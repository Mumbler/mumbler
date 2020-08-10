_If your looking for the **angular module implementation** of the mumbler initiative head over to [https://github.com/mumbler/ngx-mumbler-api](https://github.com/mumbler/ngx-mumbler-api)._


_______



# Mumbler Initiative

Open, secure, simple, authorised-only & web-based communication stack for the web


## Table of contents

-  [Idea / Basic concept](#idea)
-  [Generally](#generally)
-  [Philosophy](#philosophy)
-  [Technical principal](#technical-principal)
-  [About](#about)
-  [Support](#support)
-  [Contact](#contact)
-  [Code of conduct](#code-of-conduct)

## Idea

The goal of the mumbler initiative is to resurrect the german law concept of the 
“[Briefgeheimnis](https://de.wikipedia.org/wiki/Briefgeheimnis)” which translates to 
[secrecy of correspondence](https://en.wikipedia.org/wiki/Secrecy_of_correspondence) and is the basis for
privacy of correspondence. For centuries opening and/or reading a correspondence which was intended for 
someone else was a criminal act, often punished by jail time. The french King Louis XV even declared the death 
penalty on breaking the "Briefgeheimnis" (with his edict on 25. Sep 1742).

Today it is a well-known fact that all digital correspondence is stored somewhere and 
is read, analyzed, profiled and last but not least profited off by many parties.

The mumbler initiative is convinced that it should be of paramount importance to keep the 
information where it belongs:
 
**Only in the hands of the communication parties themselves.**

The mumbler initiative was created to achieve exactly that. Analog letters have/had the "Briefgeheimnis", 
digital correspondence have mumbler.

## Generally

No third party must be allowed to read, analyze, profile, profit of (etc...) the digital information transferred. 

Therefore mumbler ensures that all the relaying servers and networks **must not be able to decipher the information 
transferred under any circumstances and at any given time or state**. 

## Philosophy

The mumbler Initiative follows three simple but strict rules:

*  keep digital communication simple but secure (everything)
*  access only to those who are intended (including revert the access retroactively)
*  use existing, open and well-proven techniques


## Technical principal

The security and integrity of mumbler is based on the basic principles of an individual public private key 
infrastructure and an ACL. Each (new) client aka "mumbler" initially creates a set of public and private keys to en-/decrypt 
the various symmetric keys which are used to en-/decrypt the actual data blobs. This step is called __on-boarding__.

In order to send a communique (communication packets) from one mumbler to another the two mumbler account need to authenticate itself against
one another. This step is called __bonding__.

After the mumbler has "on-boarded" and "bonded" it can send and receive communication-packets.

Every communication needs to be transported via HTTPS and authenticated via TOTP (time based one time password) token.
Every mumbler has its own TOTP key.

This is just the tip of the iceberg :-). => For further technical details and implementation go to 
[https://github.com/mumbler/ngx-mumbler-api](https://github.com/mumbler/ngx-mumbler-api)

## About

Mumbler was born as an idealist idea to apply the principals of the "Briefgeheimnis" to the digital age. 
We are very low on budget and are currently solely founded through and legally constituted 
within the austrian-based startup company mumbler gmbh.

## Support

Please support the project with your coding-, design-, brain-time and/or spread the word.

Of course **operating the infrastructure is a big issue** as well. 
It would be great if you could support us by donating a couple of euros:

[![](https://www.paypalobjects.com/en_US/i/btn/btn_donateCC_LG.gif)](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=EZ2DJPABLJS6J)

**Thank you very much and we'll hope you’re as excited as we are on the idea of bringing back the “Briefgeheimnis” 
to the digital age.**

## Contact

Mumbler was created and is currently operated by:

```
mumbler gmbh
Jakob-Haringer-Str. 1
5020 Salzburg

hello@mumbler.eu
```

## Code of conduct

Lust but not least please read the [COC.md](COC.md) file for details on our code of conduct. Contributing to the
mumbler Initiative you agree to our code of conduct.
