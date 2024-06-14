# ACCIO Method Specification

---
- Docs Version : 0.0.1 (10 June 2024)
- Editors  : SmartM2M. Co.,Ltd - Minho Kwon
- Contributor : Asep Awaludin, Ismail, Minho Kwon, Hyeonhui Lee, Hoseop Lee
- Contact : (call) +82 51-518-2143 / (e-mail) smartm2m@smartm2m.co.kr
- Address : 97, Centum jungang-ro, Haeundae-gu, Busan, Republic of Korea (48058)
- Reviewer  : SmartM2M. Co.,Ltd - Myeongkil Kim (13 June 2024)
- Dev Version : ACCIO v1.2.1, Keystone v1.0.0
---

## Index

1. [Abstract](#1-abstract)
2. [DID Format](#2-did-format)
3. [CRUD](#3-crud)
4. [Security & Privacy Considerations](#4-security--privacy-considerations)
5. [Appendix A: Our Solution (ACCIO)](#5-appendix-a-our-solution-accio)

---

## 1. Abstract

---
To build and operate an enterprise blockchain effectively, we are developing a SaaS-type blockchain technology solution, such as Blockchain as a Service (BaaS). This technology allows for easy configuration changes, offering flexibility and high accessibility.

In particular, with the increasing demand for Self-Sovereign Identity (SSI) based Decentralized Identity (DID) authentication technology, we are planning to integrate powerful DID technology into our BaaS platform. This will be provided as an all-in-one solution, meeting the evolving needs of our users and enhancing the overall functionality and security of blockchain implementations.

`ACCIO` provides digital credential technology for Self-Sovereign Identity (SSI) and Verifiable Claims. By combining with our Blockchain-as-a-Service (BaaS) platform, ACCIO can replace traditional centralized credential systems with a reliable blockchain-based solution. Within the `ACCIO` system, various types of certificates are issued to ensure secure and verifiable identities.

DID (Decentralized Identifiers) is used as the unique identifier of the certificate. Also, DID allows obtaining public key information for the secure exchange of information between users. Its core technologies are built on Hyperledger Indy (Blockchain) and BaaS (Blockchain-as-a-Service) with Hyperledger Fabric.

## 2. DID Format

---

The name string that shall identify this DID method is: `m2m`

### (1) M2M DID Format

This method have the following ABNF syntax.

```
m2m-did          = "did:m2m:" m2m-identifier
m2m-identifier = base58-encoded-id
base58-encoded-id = 22 * (base58char)
base58char        = "1" / "2" / "3" / "4" / "5" / "6" / "7" / "8" / "9" / 
                    "A" / "B" / "C" / "D" / "E" / "F" / "G" / "H" / "J" / 
                    "K" / "L" / "M" / "N" / "P" / "Q" / "R" / "S" / "T" / 
                    "U" / "V" / "W" / "X" / "Y" / "Z" / "a" / "b" / "c“ / 
                    "d" / "e" / "f" / "g" / "h" / "i" / "j" / "k" / "m" / 
                    "n" / "o" / "p“ / "q" / "r" / "s" / "t" / "u" / "v" / 
                    "w" / "x" / "y" / "z"
```

- "base58-encoded-id" is a string that represents the base58 encoding of a public key generated through ECC (Elliptic-curve Cryptography)
- Examples of m2m DID :
    
    Ex1) `did:m2m:Th7MpTaRZVRYnPiabds81Y`
    
    Ex2)`did:m2m:V4SGRU86Z58d6TV7PBUe6f`
    

### (2) Sample of M2M DID Document

DID Document corresponding to Ex1) above is as follows.

```json
{
    "@context": [
        "https://www.w3.org/ns/did/v1"
    ],
    "id": "did:m2m:Th7MpTaRZVRYnPiabds81Y",
    "verificationMethod": [
        {
            "id": "did:m2m:Th7MpTaRZVRYnPiabds81Y#key-1",
            "type": "Ed25519VerificationKey2018",
            "controller": "did:m2m:Th7MpTaRZVRYnPiabds81Y",
            "publicKeyBase58": "~7TYfekw4GUagBnBVCqPjiC"
        }
    ],
    "service": [
        {
            "id": "did:m2m:Th7MpTaRZVRYnPiabds81Y#did-communication",
            "type": "did-communication",
            "serviceEndpoint": "http://3f2c1d2e.ngrok.io"
        }
    ]
}
```

## 3. CRUD

---

### Create

- “Create” generate keypair, DID and DID Document and register that in M2M.
- The detailed steps for Creation as follows :
    1. Create keypair through ECC.
    In the current version, support ed25519 and secp256r1.
    2. Create DID as above Section 2.DID Format.
    3. Create DID Document included verification methods, services, etc.
- The first part of the service must necessarily include the agent server access url of each user.
    - The Agent is a service that provides an APIs to interact with the ACCIO  Platform and use the SSI capabilities.

### Read

- “Read” resolve DID Document, which is metadata corresponding to DID.
- It satisfies the specifications below :
    1. W3C DIDs v1.0 - Section 7.1. DID Resolution
    2. DIF DID Resolution v0.3
    

### Update

- “Update” occurs when DID Document properties (e.g. verificationMethod, service, etc.) are modified or deleted.
- This update(re-create and register) DID Document based on data to be add/delete.
- The following is updated DID Document for “verificationMethod”and “service” property, respectively.
    1. Update “verificationMethod”
        
        ```json
        {
            "@context": [
                "https://www.w3.org/ns/did/v1"
            ],
            "id": "did:m2m:Th7MpTaRZVRYnPiabds81Y",
            "verificationMethod": [
                {
                    "id": "did:m2m:Th7MpTaRZVRYnPiabds81Y#key-1",
                    "type": "Ed25519VerificationKey2018",
                    "controller": "did:m2m:Th7MpTaRZVRYnPiabds81Y",
                    "publicKeyBase58": "~7TYfekw4GUagBnBVCqPjiC"
                },
                {
                    "id": "did:m2m:Th7MpTaRZVRYnPiabds81Y#key-2",
                    "type": "Ed25519VerificationKey2018",
                    "controller": "did:m2m:Th7MpTaRZVRYnPiabds81Y",
                    "publicKeyBase58": "~UhP7K35SAXbix1kCQV4Upx"
                }
            ],
            "service": [
                {
                    "id": "did:m2m:Th7MpTaRZVRYnPiabds81Y#did-communication",
                    "type": "did-communication",
                    "serviceEndpoint": "http://3f2c1d2e.ngrok.io"
                }
            ]
        }
        ```
        
    2. Update “service”
        
        ```json
        {
            "@context": [
                "https://www.w3.org/ns/did/v1"
            ],
            "id": "did:m2m:Th7MpTaRZVRYnPiabds81Y",
            "verificationMethod": [
                {
                    "id": "did:m2m:Th7MpTaRZVRYnPiabds81Y#key-1",
                    "type": "Ed25519VerificationKey2018",
                    "controller": "did:m2m:Th7MpTaRZVRYnPiabds81Y",
                    "publicKeyBase58": "~7TYfekw4GUagBnBVCqPjiC"
                }
            ],
            "service": [
                {
                    "id": "did:m2m:Th7MpTaRZVRYnPiabds81Y#did-communication",
                    "type": "did-communication",
                    "serviceEndpoint": "http://3f2c1d2e.ngrok.io",
                    "accept": [
                        "didcomm/aip2;env=rfc19"
                    ]
                },
                {
                    "id": "did:m2m:Th7MpTaRZVRYnPiabds81Y#MyService",
                    "type": "MyService",
                    "serviceEndpoint": "https://accio.technology/en"
                }
            ]
        }
        ```
        

### Deactivate

- m2m does support “Deactivate” function that permanently disuse DID.
- The detailed steps for “Deactivate” as follows :
    1. Delete all 'service' properties in the DID Document
    2. Change the publicKeyBase58 value of the verification key in the DID Document to `'FFFFFFFFFFFFFFFFFFFFFF'` (22 characters)
    
    ```json
    {
    	  "@context": [
    	    "https://www.w3.org/ns/did/v1"
    	  ],
    	  "id": "did:m2m:Th7MpTaRZVRYnPiabds81Y",
    	  "verificationMethod": [
    	    {
    	      "id": "did:m2m:Th7MpTaRZVRYnPiabds81Y#key-1",
    	      "type": "Ed25519VerificationKey2018",
    	      "controller": "did:m2m:Th7MpTaRZVRYnPiabds81Y",
    	      "publicKeyBase58": "~FFFFFFFFFFFFFFFFFFFFFF"
    	    }
    	  ]
    }
    ```
    

## 4. Security & Privacy Considerations

---

### **Residual Risks**

- Private keys used for signing must be kept confidential. If a key is compromised, the user must promptly deactivate the associated DID.

### Keep Personal Data Private

- All stored data in the DID Document is considered public. DID document does not include any personal information about the user.
- Information related to user personal data is stored only on the user's device(ex. ACCIO  Mobile) and the user's agent, accessible to the user alone
    - On the user's agent, the user's personal data is encrypted using the user's private key before storage.

## 5. Appendix A: Our Solution (ACCIO)

---

### (1) What is ACCIO

- ACCIO provides digital credential solutions for building the SSI ecosystem based on BaaS. This supports entities (Verifier/Holder/Issuer) to establish their own agents and infrastructure
    ![accio-did-overview#1](https://github.com/m2mblockchain/DID-Method-m2m/assets/130521354/e74a6edc-f42d-48c3-ac39-14d54e365d2b)
    
- Core components
    - Agent
        - The agent is capable of issuing, holding, and verifying verifiable credentials (VCs) for various entities while also managing decentralized identifiers (DIDs) and connections based on DIDs.
        - ACCIO supports the establishment of agents based on BYOC(Bring Your Own Cloud)/BYOS(Bring Your Own Server) for providers(Issuers and Verifiers).
        - Holders are provided with a service that offers multi-tenancy(likely as a sub-wallet) agent and key management capabilities through a mobile wallet.
    - Mediator
        - The mediator is a server that facilitates interactions required for VC issuance and VP verification between agents.
    - Controller
        - The controller is a service server that controls agents via RESTful APIs, with the capability to connect to Provider Legacy Systems.
    - Provider Service
        - The Provider Service includes servers like the VCs Template Manager for each issuer and the VPs (Verifiable Presentation) Template Manager server for verifiers, alongside an authentication server for each provider, among other components.
    - VDR
        - This VDR utilizes the Hyperledger Indy blockchain and is operated by provider organization using the ACCIO BaaS platform.
- Features
    - Agent-based BaaS
        - A Scala-developed agent that adheres to Aries/W3C standards, offering multi-tenancy, DIDComm V2, HTTP event notifications, and Hyperledger Indy ledger integration. It also supports W3C-compliant DID methods, credential management (JWT and Anoncreds).
        - The self-agent supports BaaS-based BYOC/BYOS functionality.
            
        ![accio-did-#2](https://github.com/m2mblockchain/DID-Method-m2m/assets/130521354/eb47b74a-8c60-407d-a0f1-1270a90518c9)

            
    - Mediator support for DIDComm v2:
        - A decentralized communication protocol enabling secure and private messaging via DIDComm v2, including support for BasicMessage 2.0 and MediatorCoordination 2.0.
    - Support DID operations
    - Verifiable credentials
        - Issuance, updating, revocation, and verification for Hyperledger AnonCreds.
- Technical Certification
    - GS Software Quality Certification Grade 1 (C.No. 24-0129, Korean TTA)

## Reference

---

[1] W3C Decentralized Identifier v1.0, https://w3c.github.io/did-core/

[2] DIF DID Resolution v0.3, https://w3c-ccg.github.io/did-resolution/

[3] ACCIO BaaS, https://accio.technology/en
