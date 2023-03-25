+++
title = "Email digital signature"
date = "2020-05-30T18:49:27+01:00"
author = ""
authorTwitter = "" #do not include @
cover = ""
tags = ["certificate", "pki", "email", "signature", "keys"]
keywords = ["", ""]
description = "Here are some notions necessary to work with email digital signature"
showFullContent = false
readingTime = false
hideComments = false
color = "" #color from the theme settings
+++

Here are some notions necessary to work with email digital signature.

## Digital signature
A [digital signature](https://en.wikipedia.org/wiki/Digital_signature) ensures
authentication and integrity of a message. The message is hashed by the sender
and the hash is encrypted by his private key and sent along with the clear
message to the receiver. The receiver then decrypt the encrypted hash with
the public key of the sender (ensures authentication), compute the hash of the
clear message, and compare it to the decrypted hash. If they match, then it
ensures the integrity of the message.

## Digital Certificate
A [public key certificate](https://en.wikipedia.org/wiki/Public_key_certificate),
aka Digital Certificate, is an electronic document used to prove ownership of a
public key. It contains :
 - information about the public key
 - information about the owner
 - the digital signature of the issuer, an entity that has verified the certificate
   content (ex: a Certificate Authority)

There are two major types of digital certificates : 
 - S/MIME certificates, based on the x.509 protocol, that relies on a Public Key Infrastructure (PKI)
 - OpenPGP certificates that relies on a Web of Trust

## Public Key Infrastructure (PKI)
A [public key infrastructure (PKI)](Public Key Infrastructure) is a centralized system that manages
digital certificates and binds *public keys* with *identities* (like people or organizations).
The binding is established through a process of registration and issuance of certificates
at and by a certificate authority (CA).

## Web of Trust
In the [Web of Trust](https://en.wikipedia.org/wiki/Web_of_trust) model, the binding
between a public key and its owner is made using a decentralized trust model where
OpenPGP identity certificates (which include one or more public keys along with owner
information) can be digitally signed by other users who, by that act, endorse the association
of that public key with the person or entity listed in the certificate.
 

Resources : 
 - [How does S/MIME differs from OpenPGP](https://superuser.com/questions/274169/how-does-s-mime-differ-from-pgp-gpg-for-the-purpose-of-signing-and-or-encryptin)
 - [Actalis free S/MIME certificates](https://www.actalis.it/products/certificates-for-secure-electronic-mail.aspx)

 