# ipsec

## strongswan
- [ipsec.conf Reference - ipsec.conf Reference - strongSwan](https://wiki.strongswan.org/projects/strongswan/wiki/Ipsecconf)
- [ipsec.conf: conn Reference - ipsec.conf: conn Reference - strongSwan](https://wiki.strongswan.org/projects/strongswan/wiki/ConnSection)
```
esp = <cipher suites>

comma-separated list of ESP encryption/authentication algorithms to be used for the connection, e.g.
aes128-sha256. The notation is encryption-integrity[-dhgroup][-esnmode].
For IKEv2, multiple algorithms (separated by -) of the same type can be included in a single proposal.
IKEv1 only includes the first algorithm in a proposal. Only either the ah or the esp keyword may
be used, AH+ESP bundles are not supported.

Defaults to aes128-sha256 (aes128-sha1,3des-sha1 before 5.4.0). The daemon adds its extensive default
proposal to this default or the configured value. To restrict it to the configured proposal an exclamation mark (!)
can be added at the end.
Note: As a responder, the daemon defaults to selecting the first configured proposal that's also
supported by the peer. By disabling charon.prefer_configured_proposals in strongswan.conf this may
be changed to selecting the first acceptable proposal sent by the peer instead.
In order to restrict a responder to only accept specific cipher suites, the strict flag (!, exclamation mark)
can be used, e.g: aes256-sha512-modp4096!

If dh-group is specified, CHILD_SA rekeying and initial negotiation include a separate Diffe-Hellman
exchange (since 5.0.0 this also applies to IKEv1 Quick Mode). However, for IKEv2, the keys of the CHILD_SA
created implicitly with the IKE_SA will always be derived from the IKE_SA's key material. So any DH group
specified here will only apply when the CHILD_SA is later rekeyed or is created with a separate CREATE_CHILD_SA
exchange. Therefore, a proposal mismatch might not immediately be noticed when the SA is established,
but may later cause rekeying to fail.

Valid values for esnmode are esn and noesn. Specifying both negotiates extended sequence
number support with the peer, the default is noesn.

Refer to IKEv1CipherSuites and IKEv2CipherSuites for a list of valid keywords.


ike = <cipher suites>

comma-separated list of IKE/ISAKMP SA encryption/authentication algorithms to be used, e.g.
aes128-sha256-modp3072. The notation is encryption-integrity[-prf]-dhgroup. In IKEv2, multiple algorithms
and proposals may be included, such as aes128-aes256-sha1-modp3072-modp2048,3des-sha1-md5-modp1024.

The ability to configure a PRF algorithm different to that defined for integrity protection was added with 5.0.2.
If no PRF is configured, the algorithms defined for integrity are proposed as PRF. The prf keywords are the same as
the integrity algorithms, but have a prf prefix (such as prfsha1, prfsha256 or prfaesxcbc).

Defaults to aes128-sha256-modp3072 (aes128-sha1-modp2048,3des-sha1-modp1536 before 5.4.0) for IKEv1.
The daemon adds its extensive default proposal to this default or the configured value. To restrict it to the
configured proposal an exclamation mark (!) can be added at the end.
Refer to IKEv1CipherSuites and IKEv2CipherSuites for a list of valid keywords.

Note: As a responder both daemons accept the first supported proposal received from the peer. In order
to restrict a responder to only accept specific cipher suites, the strict flag (!, exclamation mark)
can be used, e.g: aes256-sha512-modp4096!
```
- [IKEv1 Cipher Suites - strongSwan](https://wiki.strongswan.org/projects/strongswan/wiki/IKEv1CipherSuites)
- [IKEv2 Cipher Suites :: strongSwan Documentation](https://wiki.strongswan.org/projects/strongswan/wiki/IKEv2CipherSuites)

## IKEv1
- [Internet Key Exchange (IKE) Attributes](https://www.iana.org/assignments/ipsec-registry/ipsec-registry.xhtml)
## IKEv2
- [Internet Key Exchange Version 2 (IKEv2) Parameters](https://www.iana.org/assignments/ikev2-parameters/ikev2-parameters.xhtml#ikev2-parameters-5)
- [Internet Key Exchange Version 2 (IKEv2) Parameters](https://www.iana.org/assignments/ikev2-parameters/ikev2-parameters.xhtml#ikev2-parameters-6)
- [Internet Key Exchange Version 2 (IKEv2) Parameters](https://www.iana.org/assignments/ikev2-parameters/ikev2-parameters.xhtml#ikev2-parameters-7)
- [Internet Key Exchange Version 2 (IKEv2) Parameters](https://www.iana.org/assignments/ikev2-parameters/ikev2-parameters.xhtml#ikev2-parameters-8)
