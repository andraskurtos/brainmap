---
aliases: 
tags: 
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2025-02-17 11:59
---
# Information Security

The general objective of Information Security is to keep the system in an  uncompromised state by *preventing attacks*, detecting and reacting to attacks, and recovering from security incidents.

First of all, we would like to prevent attacks being successful, but this **may not always be possible or feasable.**

The next best thing is *detecting attacks* as they are happening, and *quickly responding and reacting* to them. Detection usually aims at detecting the activity of the attacker before it has serious consequences, and *containing it*, *mitigating it*, and eventually hardening the system so that it cannot happen again.

Despite all efforts, attacks may sometimes be successful. In such cases, it is our responsibility to efficiently recover from the incident. Recovery includes:
1. restoring the system to its uncompromised state asap
2. analyzing the incident and understanding how the system was compromised
3. eliminating the vulnerabilities that allowed the attack to be successful.

## More specific goals (AAA)
### Authentication, Authorization and Accounting

Authentication is the *verification of the identity* of an entity (user or process), typically to make an access control decision when the entity is trying to access some service or resource of the system.

Authorization and access control *defines access rights* and *enforces them* through an access control policy. An access control decision may depend on *the verified identity* of the entity, *the nature of the access*, the *access rights associated with* the entity, and other circumstances.

Accounting makes it possible to record, search, and prove retrospectively every access to the system and every operation performet within it. This ensures accountability of the users.

>[!NOTE]+ CIA - Confidentiality, Integrity, Availability
>**Confidentiality**:
>Information can be obtained only by those who have permission to do so.
>**Integrity**:
>Information can be modified only by those who have persmission to do so.
>**Availability:**
>Information should be available whenever needed for legitimate users.

