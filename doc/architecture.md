# Architecture

## Introduction

**KORD Guardian services** are micro-services that expose a single HTTP
endpoint. Requests can be sent to this endpoint with a verifiable claim object
in the body. This claim will be verified and, if all verification checks pass,
a verified **KORD Claim** object will be returned. This verified claim can then
be added to a **KORD Graph** in the **KORD Network**. KORD Graphs can be queried
by other services to establish the credentials of a **KORD Agent**.

## Supported languages

Currently, `Node.js` is the only officially supported language. Although KORD
Guardian micro-services could be written in other languages, the supporting
documentation and boilerplate code depend on the
[`kord.js`](https://github.com/kord-network/kord.js) library. Official support
for other languages will be added as other external libraries are developed.

## Data flow

1. A verifiable claim is sent to the KORD Guardian micro-service via an HTTP
   request

   ![1](https://user-images.githubusercontent.com/1913316/37060991-dd341c4c-2189-11e8-8cbd-ef82eb3c87f0.png)

2. The KORD Guardian micro-service MUST recover the sender's Ethereum address
   from the claim signature and verify that it matches the address sent in the
   body of the claim

   ```
   verifyIdentityClaim(address, claimHash, signature)
   ```

3. Additional verification MAY be made on the claim. For example: OAuth,
   TLS Notary, JWT, 2FA, social network statements, national ID document checks.

4. The KORD Guardian micro-service MUST return a verified KORD Claim object

   ```
   createVerifiedIdentityClaimObject(claimMessage, graph, issuer, property, subject)
   ```

   ![2](https://user-images.githubusercontent.com/1913316/37060992-dd4e54ae-2189-11e8-8da6-424f8845bba7.png)

5. The verified KORD Claim MAY then be added to a KORD Graph in the KORD Network
   by the claim subject.
