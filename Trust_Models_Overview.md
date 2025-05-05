# 

[**Summary	1**](#summary)

[**Trust models	1**](#trust-models)

[Single CA Trust Model	1](#single-ca-trust-model-\(c\))

[Hierarchical Trust Model (HTM)	2](#hierarchical-trust-model-\(htm\))

[Mesh Trust Model (MTM)	3](#mesh-trust-model-\(mtm\))

[Bridge Trust Model (BTM)	5](#bridge-trust-model-\(btm\))

[Hybrid Pki Model	5](#hybrid-pki-model)

# 

# 

# Summary {#summary}

This document is a summary of my perspective on establishing the trust models with PKI.

# Trust models {#trust-models}

## Overview

| Order | Trust Model | Complexity Level | Description |
| ----- | ----- | ----- | ----- |
| 1 | Single CA Trust Model (SCaTM) | Very Low | One CA issues all certificates; easy to set up and manage. |
| 2 | Direct Trust (DTM) | Low | Trust is configured manually between entities (e.g., pinned certs, self-signed certs). |
| 3 | Web of Trust (WoT) | Moderate | Decentralized trust built by users signing each other's keys; complex to evaluate trust paths. |
| 4 | Hierarchical Trust Model (HTM) | Moderate–High | Root CA issues to intermediates → end-entities; scalable and widely used. |
| 5 | Mesh Trust Model (MTM) | High | Multiple CAs trust each other directly; harder to manage at scale. |
| 6 | Bridge Trust Model (BTM) | Very High | Links multiple PKI hierarchies via a bridge CA; complex cross-certification logic. |
| 7 | Hybrid PKI Model | Variable–Very High | Combines elements of multiple models; highest complexity depending on implementation. |

## Single CA Trust Model (c) {#single-ca-trust-model-(c)}

This is the simplest trust model, where all the end users trust a single CA server. The CA is signing all the end user certificates. 

The benefit with this model \- it's the simplest one and simplest one to deploy.

The downsides of this model are:

1. It's not suitable for large organizations, represented by a diverse and large community of users.   
2. If the CA’s public key changes, then the entire system needs to be reconfigured.

## Direct trust model (DTM)

In the Direct Trust Model, trust is explicitly and manually configured between parties. There is no CA hierarchy or intermediate validation path. A user or system directly trusts a specific certificate or public key, often by preloading or pinning it.

* **Structure**: One-to-one trust relationship between parties  
  **Trust basis**: Manual configuration or implicit trust in known certificates or keys  
  **Example**: Self-signed certificates in internal systems, pinned public keys in IoT devices

All validations begin with a predefined trusted certificate or key that the application accepts as authoritative. There is no external validation or delegation involved.

This model is widely used in small-scale systems, closed environments, or embedded systems, where trust relationships are static and manually controlled. It is simple and secure when the number of parties is small, but does not scale well and requires manual key distribution and rotation.

It is easy to implement, has no single point of failure, but becomes unmanageable in large, dynamic environments.

## Web of Trust (WoT)

A decentralized trust model where users or entities validate each other’s identities by signing each other's public keys. There is no central authority, and trust is built through a network of endorsements. Each participant decides whom to trust and how much to trust them, often based on the number and strength of signatures on a key.

* **Structure**: Decentralized user-to-user or node-to-node trust network  
* **Trust basis**: Personal trust and transitive trust through key signatures  
* **Example**: PGP (Pretty Good Privacy), GnuPG key exchanges

All validations begin with a locally trusted key (a trust anchor manually accepted by the user). Trust can be transitive—if you trust Alice, and Alice trusts Bob, you might also choose to trust Bob.

This model is well-suited for communities where manual trust decisions are feasible and preferred. It eliminates the need for centralized CAs, but it does not scale well and makes revocation and identity validation difficult.

This trust model provides resilience and autonomy but requires users to be actively involved in trust decisions, which can be a barrier for large-scale deployment.

## Hierarchical Trust Model (HTM) {#hierarchical-trust-model-(htm)}

Is also known as the tree model. It is represented by a tree structure, where at the very top is the start of the trust \- the root CA. Any new level of CAs added are called subordinate CA’s. Depending on the context, they are also known as the intermediate CAs.

* **Structure**: Root CA \-\> Intermediate CA \-\> End  User  
* **Trust anchor:** Root CA  
* **Example**: HTTPS/TLS  Certificate

   
Any intermediate CA’s trust only the uplink CA, and the trust model goes all the way to the root certificate. 

This is the most common trust model that can be found out there. Compared to the Single CA trust model, the hierarchical trust model is scalable. 

## 

## Mesh Trust Model (MTM) {#mesh-trust-model-(mtm)}

Consists of multiple CAs, where each CA node trusts each other directly and equally. There is no hierarchy and this set up is also known as peer-to-peer cross certification, Each CA is autonomous in this setup.  The trust relationship  might not be unconditional. A CA can limit its trust and this can be done by specifying the limitation in the certificate it issues. 

* **Structure**: Peer to Peer trust between the CAs or Nodes  
* **Trust basis**: Direct bilateral agreements  
* **Example**: Federated entity in government or consortiums 

All the validations of the CA start with the local CA certificate. 

The application of this system can be found in systems where peer-to-peer trust is required, such as federated identity systems, blockchain  networks or multi CA PKI environment. 

## 

This trust model is easily extendable, however it is costly to maintain and complicated from a management perspective. The positive side is that there is no single point of failure. In this model, the number of trust relationships is proportional to the number of CA nodes, \- what might bring scalability problems.  

## Bridge Trust Model (BTM) {#bridge-trust-model-(btm)}

In the bridge trust model, we have at least two (but considerably more) Root CAs that communicate with each other. The main purpose is to establish cross certification. The Bridge Trust Model is used to allow certification processes between organizations, departments or any other groups.

* **Structure**: Multiple PKI hierarchies linked via a "Bridge CA"  
* **Trust Basis**: Cross-certification through a bridge authority  
* **Example**: U.S. Federal Bridge CA for cross-agency trust

End entities know their paths to the BCA and they only need to determine the path from the BCA to the target entity.  

This model is less complex than the mesh, but much more complex than the single CA mode. The certificates are more complex and the issue is how to distribute the certificates and the statuses.

## Hybrid Pki Model {#hybrid-pki-model}

As the name indicates, this model allows the mixing of any previous mentioned models.

* **Structure**: Combines elements of hierarchical, mesh, or bridge models  
* **Trust Basis**: Custom to fit system requirements  
* **Example**: Global PKIs with federated elements and regional root CAs

