# PACT Protocol
## Provenance Attestation and Chain of Trust

**Version 1.0 — June 2026**  
**protocol@pbmlabs.com**

---

## Abstract

Every institutional email sent anywhere in the world is automatically cryptographically signed. Every receiving mail server validates that signature and generates a structured report of the result. These reports have been produced continuously, at global scale, since 2011. They have never been captured, persisted, or chained.

PACT is an open protocol that captures these reports, anchors them in an append-only Merkle tree on a public blockchain, and derives mathematically verifiable trust scores for institutional domains — without accessing, processing, or storing any message content, recipient identity, or personal data of any kind.

The result is the first cryptographic record of institutional legitimacy built entirely from infrastructure that already exists, already operates, and requires no behavioral change from any sender or recipient.

---

## 1. The Problem

### 1.1 The End of Document Trust

For decades, verifying a document's origin relied on human judgment: does the letterhead look right, does the domain match, does the formatting seem legitimate? These checks were imperfect but sufficient when fabricating a convincing document required meaningful skill and resources.

Generative AI ended that. A photorealistic pay stub, bank statement, or signed contract can now be produced in seconds at negligible cost, indistinguishable from a legitimate document by any visual or metadata inspection. The attack surface is no longer the document's appearance. It is the absence of any verifiable link between a document and the institutional domain it claims to originate from.

In November 2024, FinCEN issued Alert FIN-2024-Alert004, explicitly identifying AI-generated synthetic financial documents as an active threat to the US financial system. The alert told institutions to increase scrutiny of digital documents. It could not tell them how — because no technical standard for doing so exists.

### 1.2 The Hidden Audit Trail

There is a fact about the global email system that almost no one outside of infrastructure engineering knows.

Since 2011, every major institutional sender has been cryptographically signing their outbound email using DKIM (DomainKeys Identified Mail, RFC 6376). Every receiving mail server — Gmail, Outlook, Yahoo, and hundreds of others — validates that signature on every email it receives. And every one of those receiving mail servers already generates a structured aggregate report of those validations and sends it to the domain owner every 24 hours.

These reports contain: the sending domain, the validation result, the sending IP range, the DKIM selector, the message count, and the reporting period. They contain nothing about the content of any message, the identity of any recipient, or any personally identifiable information.

They are generated today, continuously, for every institutional domain that has published a DMARC record. They have never been systematically captured, persisted, or chained into a provenance layer.

PACT captures them.

---

## 2. The Protocol

### 2.1 Connecting a Domain

A domain connects to PACT by adding a single address to one field in its existing DNS record. That is the only change required. No software to install, no API to integrate, no behavioral change for any sender or recipient. The global mail infrastructure continues operating exactly as before — and now sends a copy of each aggregate report to PACT alongside the domain's existing destination.

For most domain operators, this is a single click through an OAuth-based onboarding flow. For others, it is one DNS field edit.

### 2.2 From Report to Proof

When an aggregate report arrives at PACT, the authentication metadata is extracted and committed as a cryptographic leaf in an append-only Merkle tree. The raw report is discarded immediately after extraction. Only the extracted signals — domain, period, pass and fail counts, selector and IP range identifiers — are preserved, in hashed form.

Once daily, the Merkle root is published to a public blockchain in an immutable smart contract transaction. The record is permanent. It cannot be modified, backdated, or deleted.

### 2.3 Independent Verification

Any party can verify any domain's history at any time, without contacting PACT's operators. The verification requires only the public Merkle root on-chain and the inclusion proof for the leaf in question. The smart contract's verification function is open and callable by anyone. No API key, no subscription, no permission required.

This is not a design choice made for convenience. It is a design constraint made deliberately: a trust record that requires trusting its operator is not a trust record. PACT is verifiable by construction.

---

## 3. The Trust Score

### 3.1 Organic, Not Assigned

Trust scores in PACT are not declared, granted, or assigned by any authority. They emerge mathematically from the accumulated history of verified aggregate reports in the public Merkle tree.

The intuition is direct: a domain that has been sending authenticated email at scale, validated by hundreds of independent mail servers globally, continuously for years, cannot have fabricated that history. The cost of constructing it retroactively is identical to the cost of having operated as a legitimate institution for that period.

No committee governs the score. No application process exists. No registry can be captured. The score is a mathematical consequence of observable, independently verifiable reality.

### 3.2 Three Components

The trust score for a domain is the product of three factors:

**Volume** — the logarithm of total authenticated message count. Logarithmic scaling ensures that doubling message volume does not double the score. Bulk inflation yields diminishing returns.

**Diversity** — the ratio of unique receiving mail servers to total verified events. A domain whose authenticated email is received by hundreds of independent providers globally scores higher than one concentrated in a single infrastructure. This reflects real institutional reach and resists artificial inflation through controlled or coordinated receiver networks.

**Maturity** — an asymptotic function of time since the domain's first verified event in the tree. A domain registered yesterday cannot achieve a high maturity score regardless of message volume. Maturity approaches its maximum over approximately two years of continuous presence. Time cannot be compressed or purchased.

The maturity factor is the most consequential defense. Lookalike domains and synthetic entities face an insurmountable time gap versus the domains they attempt to impersonate. The global mail infrastructure has been certifying `wise.com` for years. No attacker can replicate that accumulation.

### 3.3 Thresholds Are Policy, Not Protocol

PACT produces a measurement. Applications consuming the score define their own acceptance thresholds based on their risk tolerance and use case. The protocol enforces no threshold. This separation of measurement from policy is deliberate — it allows the same underlying trust score to serve a bank's onboarding workflow, a procurement platform's vendor screening, and an automated decision system's counterparty evaluation, each with appropriate calibration.

---

## 4. Privacy by Architecture

### 4.1 A Structural Guarantee

PACT's privacy guarantee is architectural, not contractual. It does not rest on a privacy policy or a compliance certification. It rests on the fact that DMARC aggregate reports — PACT's sole data source — contain no personal data by design.

PACT never accesses, processes, transmits, or stores any message content, subject lines, recipient identities, sender email addresses, or any personally identifiable information. This cannot be changed by policy, misconfiguration, or breach, because the data never enters the system.

The privacy guarantee is enforced by the data source, not by PACT's operators.

### 4.2 What Is Public

Each entry in the Merkle tree commits to the sending domain, the reporting period, aggregate authentication counts, and hashed infrastructure identifiers. Every element is either already public information (domain names) or opaque (hashes that reveal nothing about their preimages to observers).

The on-chain record proves that a domain was authenticated at a certain volume in a certain period. It reveals nothing about any message, any person, or any communication.

### 4.3 Regulatory Posture

Domain names are not personal data under GDPR. The information PACT processes falls entirely outside the scope of data subject rights under current privacy frameworks in the EU, UK, and US. No data processing agreements are required for core protocol operation. Organizations with specific compliance requirements should obtain their own legal guidance.

---

## 5. Proof of Operational Work

### 5.1 A Consensus Mechanism Hidden in Plain Sight

PACT's Sybil resistance can be understood through a mechanism that already operates at global scale without being recognized as such.

Maintaining authenticated institutional email requires a registered domain, DNS infrastructure, DKIM key management, a mail server or provider, and continuous delivery to real recipients over time — recipients whose mail servers then certify the delivery through aggregate reports. These are real operational costs that compound over time and cannot be trivially manufactured.

The receiving mail servers — Gmail, Outlook, Yahoo, and hundreds of others — act as independent validators. They have no relationship with PACT, no incentive to coordinate, and no awareness that their reports are being used as evidence of anything beyond their immediate filtering function. Their aggregate reports are, in effect, an independent and uncoordinated consensus that a domain's authenticated email activity occurred exactly as recorded.

PACT packages that consensus into an immutable on-chain record.

### 5.2 Why the Attack Fails

To achieve a high trust score through illegitimate means, an attacker would need to send authenticated email at institutional volume, consistently, over years, from infrastructure that passes the spam and abuse filters of every major mail provider globally, to recipients spread across hundreds of independent receiver domains — and maintain this without interruption long enough for the maturity factor to accumulate.

That is not an attack. That is legitimate operation. The economic cost of the attack equals the economic cost of being a real institution. PACT's Sybil resistance is not enforced by a gatekeeper — it is enforced by the economics of the global email infrastructure itself.

---

## 6. The Endgame

### 6.1 FICO for Institutional Identity

The credit bureau model is the closest existing analog. A credit score is calculated by a neutral third party from behavioral history, consumed by independent institutions that trust the score rather than the subject, and so infrastructural that most of its users no longer ask why it exists.

PACT builds the same model for institutional domain identity. The behavioral history is authenticated email activity as certified by the global mail infrastructure. The validators are independent mail servers. The output is a trust score consumable by any system that needs to answer a single question: *Is this institutional domain real, operationally stable, and consistent with its claimed history?*

No existing system provides a satisfactory answer to that question.

KYB and document verification rely on self-declared or third-party-validated documents — all fabricable, increasingly so. Domain reputation systems produce real-time filtering signals with no persistent, independently verifiable record. Blockchain analytics address on-chain behavior but are inapplicable to the vast majority of institutional actors whose legitimacy is established off-chain. Credit bureaus address individuals and financial behavior, not institutional operational continuity.

PACT answers the question none of these address: not what an entity declares, but what it has demonstrably done — as independently certified by the global mail infrastructure — over a verifiable period of time.

### 6.2 The Scope

The trust signal PACT produces is not a compliance tool. It is a primitive. Any system requiring institutional trust verification can consume it.

Financial institutions verifying that a counterparty's domain has a credible operational history before a transaction or account opening. Compliance and due diligence teams requiring verifiable evidence of institutional legitimacy beyond documentation. Insurance underwriters using operational maturity as a proxy for security hygiene. Procurement platforms distinguishing established vendors from entities with no verifiable track record.

The most consequential category is forward-looking: automated decision systems and AI agents making procurement, contracting, and payment decisions that previously required human judgment. As these systems scale, they will require trust signals that are cryptographically verifiable, machine-readable, and structurally resistant to manipulation. A signal that cannot be manufactured by producing better documentation or more convincing synthetic content.

PACT is that signal.

### 6.3 The Deeper Architecture

The commercial endgame is the FICO model. The architectural endgame is something more foundational.

Decentralized identity systems have solved cryptographic ownership — anyone can generate a unique identifier whose control is provable. What they have not solved is the empty container problem: a fraudulent entity and a legitimate multinational have identical cryptographic validity on day one. There is no primitive that binds real-world operational history to a cryptographic identity without a trusted intermediary.

PACT is that primitive. It converts the existing email authentication infrastructure into a Proof of Operational Work oracle: a verifiable record that a specific domain has been operating at scale in the real world, as independently certified by the global mail infrastructure, anchored permanently on-chain. This record can be consumed by any identity protocol, any smart contract, or any automated system that needs to answer the question of institutional legitimacy without trusting a centralized authority.

The architecture supports this extension. The protocol is designed so that its Merkle roots and trust scores can be consumed by any downstream system, on any chain, through any interface. The base layer is the trust record. What is built on top is open.

---

## 7. Ecosystem

PACT Protocol is the base layer — open, freely implementable, and independent of any single operator for verification correctness. Any organization can verify the public Merkle tree against the on-chain roots without contacting the protocol's authors.

Three applications are defined in the PACT ecosystem, each consuming the protocol as infrastructure:

**PACT Chain** packages a domain's full provenance history into a portable, independently verifiable credential for compliance submissions, financial institution onboarding, regulatory filings, and due diligence.

**PACT Signal** monitors connected domains for anomalies in their aggregate authentication patterns — failure rate spikes, unrecognized infrastructure, lookalike domain activity — and delivers actionable alerts to domain operators. No message content is accessed at any point.

**PACT Proof** enables document-level authentication with explicit user consent. A user provides a specific received email voluntarily; PACT Proof validates the individual cryptographic signature and generates a certificate of authenticity referencing the sending domain's PACT Protocol trust history. This is always an active, user-initiated action.

The protocol boundary is absolute: PACT Protocol never crosses into message-level data. Applications built on top operate under their own privacy models and consent mechanisms, independently of the protocol.

---

## 8. Status and Availability

PACT Protocol is at the specification and early build stage. The protocol specification is open and freely available. Third-party implementations are encouraged.

The reference implementation is in active development. The MVP milestone is a live public domain provenance page — trust score, authentication history, and independently verifiable Merkle proof — backed by real DMARC aggregate reports anchored on a public blockchain.

The protocol is designed to evolve toward permissionless node operation. The initial reference implementation operates a single node; the architecture supports and anticipates multiple independent nodes. The on-chain verification function ensures that the correctness of any published root is independently verifiable regardless of who operates the publishing infrastructure.

---

*PACT — Provenance Attestation and Chain of Trust*  
*Whitepaper v1.0 — June 2026*  
*protocol@pbmlabs.com*