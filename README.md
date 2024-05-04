# 2025 Safeguarding XMPP Manifesto [DRAFT]
LICENSE: CC0  
VERSION: 20240504-00

The use of “MUST”, “MUST NOT”, “REQUIRED”, “SHALL”, “SHALL NOT”, “SHOULD”, “SHOULD NOT”, “RECOMMENDED”, “MAY”, and “OPTIONAL” is per the IETF standard defined in RFC2119.

# Notice
This is not sponsored or endorsed by the XMPP Standards Foundation. This is an unofficial writing by a community member.

# Synopsis
As we look forward to the future we must be proactive in strengthening and guaranteeing a minimum baseline of the network.
This document outlines mandatory and recommended requirements for publicly federating servers to achieve that.

# Servers

## Abuse & Spam Control
* Public servers MUST make abuse contacts available via XEP-0157 and respond to reports in a reasonable time frame
* Real-time JID blocklists SHOULD be utilized
  * https://xmppbl.org is RECOMMENDED
* Long-term server blocklists SHOULD be utilized
  * https://github.com/JabberSPAM/blacklist is RECOMMENDED

## Connections
* Encryption is REQUIRED for all connections
* Available cipher suites MUST support forward secrecy
* TLS 1.3 MUST be supported
* TLS 1.2 SHOULD be disabled if unlikely to impact users
* Legacy SSL (1.0/2.0/3.0) or TLS (1.0/1.1) MUST NOT be supported
* Certificates MUST be enforced for S2S connections
* Channel binding MUST be available if supported by server software
* STUN services SHOULD be located on non-standard ports to minimize use in STUN amplification attacks
* Stanza size limits MUST be enforced for C2S and S2S connections
  * These limits MUST not be more than 2,097,152 bytes
  * 262,144 bytes is RECOMMENDED for C2S
  * 524,288 bytes is RECOMMENDED for S2S

## Miscellaneous
* Passwords MUST be hashed with a sufficiently strong mechanism
* Limits MUST be enforced on failed password attempts
  * The corresponding IP address SHOULD be blocked entirely to prevent processing any further traffic
* E2EE mechanisms (eg. OMEMO, OTR, or PGP) MUST NOT be made inoperable
* HTTP Upload paths MUST contain sufficiently random strings to prevent bulk enumeration
* HTTP Upload paths SHOULD NOT contain plain owner usernames to prevent privacy leaks
  * They however may be preferred to make abuse reporting easier

## Registration
* If public registrations are allowed some form of captcha SHOULD be utilized
* Weak passwords SHOULD be rejected
* For small/personal instances it is RECOMMENDED to disable public registration in favor of invitation mechanisms (XEP-0401)
* Registrations SHOULD be limited per a given time frame (eg. one hour or day) per single IP address or range
* Registrations SHOULD be watched if public registrations are allowed

# Host Systems

## System Security
* Remote access MUST NOT utilize passwords and MUST utilize keys for authentication
  * Multi-factor authentication (MFA) SHOULD additionally be used if possible
  * Keys SHOULD be password protected or located on a hardware backed store
     * These SHALL NOT count as MFA
* Daemons SHOULD be sandboxed if systemd is in-use
* Daemons SHOULD be confined via a MAC such as SELinux
* Distinct public facing services SHOULD be isolated to their own (virtual) machine
  * This applies regardless of being sandboxed or confined, all three are RECOMMENDED simultaneously
* Program allowlisting and integrity checks SHOULD be enforced via eg. fapolicyd
* Logging SHOULD be appropriately balanced to maintain integrity, accountability, and user privacy
* Secure Boot SHOULD be used if supported by host and operating system
* Memory SHOULD be encrypted if supported by CPU and/or hypervisor
* All disks SHOULD be encrypted if possible
* On disk swap MUST NOT be used
  * Alternatives such as ZRAM are RECOMMENDED
* Multiple authenticated time sources SHOULD be used

## System Updates
* Host and guest operating systems MUST NOT be end-of-life
* System updates MUST be performed at least once a month
  * Updates SHOULD be performed more frequently such as weekly or daily
  * Use of live patching SHALL NOT take place of this requirement
* The system MUST be restarted after critical component updates such as the kernel
* Full system reboots MUST be performed at least once a month
* All public facing services MUST be restarted if their dependencies or themself are updated
  * Rebooting after updates is the easiest way to ensure this

## System Firewall
* Known malicious IP addresses SHOULD be blocked
  * This can cause issues however for users behind shared IPs such as CG-NAT or consumer VPNs
     * Consider offering a Tor Onion service if you chose to additionally block Tor exit nodes
  * https://github.com/stamparm/ipsum is RECOMMENDED
  * https://stopforumspam.com is RECOMMENDED for spam
  * https://voipbl.org is RECOMMENDED for VoIP abuse

# Supporting Infrastructure

## DNS
* If DNSSEC is supported by the TLD, it SHOULD be used
* Fingerprints of certificates SHOULD be available via TLSA records

## PKI
* CAA records MUST be present
* CAA records SHOULD bind to a singular `accounturi` if supported by CA

# Clients

## Connections
* DNSSEC SHOULD be enforced for lookups
* Certificate fingerprints MUST be verified if TLSA records are available
* Channel binding MUST be perpetually enforced for a server if ever utilized
* Clients SHOULD robustly attempt reconnection to servers and MUCs after they have been rebooted

## Messages
* Some form of E2EE MUST be supported
* 1:1 and private group chats SHOULD be E2EE by default

## Privacy
* The user MUST be warned about JID disclosure before joining a non-anonymous MUC
* The client MUST NOT ever automatically download any files from strangers or in public MUCs
* The client SHOULD NOT send chat state indicators to strangers or in public MUCS by default
* The client SHOULD store received files in non-public storage on the device by default
* The client SHOULD relay calls to or from strangers by default

# Comments

Please provide feedback via the following: 

* https://github.com/divestedcg/safeguarding-xmpp-2025/issues
* https://gitlab.com/divested/safeguarding-xmpp-2025/-/issues
* https://codeberg.org/divested/safeguarding-xmpp-2025/issues
* https://xmpp.org/community/channels/operators/

# Signers

This document is still a draft pending comments and cannot be signed.
