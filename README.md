# KYxFi_MVP
A compliance layer for tokenization
Proposal Title

KYXFI: Know Your Asset (KYA), a verification layer for real-asset tokenization on Drunix

**Problem Understanding**

Tokenization can prove that a token exists, who holds it and how it moved. It cannot prove what the token represents in the real world.

For Indian real estate, the proof that matters is scattered:
  land records held separately by each state (for example, Gujarat's AnyROR)
  encumbrance records at sub-registrar offices
  RERA registrations
  company filings on MCA
  valuation reports
  the issuer's own documents

These sources often disagree, go out of date, or are missing entirely. Today every investor, platform and lender repeats this diligence by hand, and the result is never shared or kept current.

As tokenization grows on platforms like Drunix, this gap becomes the main risk. A token can be technically valid while the claim behind it (clean title, no mortgage, correct owner) is unproven or false. Retail investors buying small fractions through UPI cannot do legal diligence themselves. Without a trusted evidence layer, fraud risk rises, regulators stay cautious, and tokenized real assets fail to scale.

**Solution Description**

KYXFI adds a Know Your Asset (KYA) layer to tokenization: no token without evidence.

For every asset, KYXFI builds a Verification Record that links real-world evidence to on-chain facts. Every claim carries exactly one status:

Evidenced: confirmed against an authoritative source
Disclosed: stated by the issuer but not independently confirmed
Correlated: consistent across independent sources
Conflicting: sources disagree
Unknown: expected evidence is missing

Claims cover identity, ownership, encumbrance, RERA registration, valuation, custody and token binding.

On Drunix, the record is shared across the network's participants:
  the issuer submits the asset and its evidence
  KYXFI acts as verifier and correlates the evidence
  a registrar or lawyer confirms land-record and legal claims
  investors and platforms read each claim's status before buying

**Smart contract rules enforce trust at the ledger level:
**
Minting gate: tokens can only be minted when critical claims (title, encumbrance) are evidenced.
Purchase check: a UPI-settled fractional purchase goes through only if the asset's record is current.
Automatic pause: if a claim later changes to Conflicting, for example a new mortgage appears, further sales pause and holders are alerted.

The result is tokenized property that investors, banks and regulators can inspect claim by claim, instead of trusting a PDF.

**Implementation Approach**

Network setup: a Drunix test network with four organizations (Issuer, Verifier, Registrar/Legal, Investor/Platform), using separate endorsement policies for each role.
Chaincode:
Asset and Claim records with status transitions, and multi-party endorsement for status changes.
A Verification Record entry holding the hash of the signed report.
A mint function that checks the required claims before issuing tokens.
Pause and resume triggered by claim conflicts.
Evidence service: document upload with SHA-256 hashing and a record of where each file came from. AI-assisted extraction of fields from title documents, 7/12 extracts and RERA certificates. Rule-based cross-checks of IDs, names, areas and dates across sources.
Screening: check issuer, SPV and director names against public sanctions lists.
Payments: a UPI purchase flow that queries the asset's KYA status before settlement. It uses a mock first and switches to NPCI APIs once access is granted.
Web app: an issuer workspace, a reviewer queue, and an investor view showing coverage, gaps and conflicts.
Demo scenario: an illustrative Gujarat land parcel. The mint succeeds once title and encumbrance are evidenced. When a conflicting encumbrance record is added, sales pause automatically.

**Technology Stack**

Ledger: NPCI Drunix (Hyperledger Fabric fork); chaincode in Go
Backend: Node.js/TypeScript with the Fabric Gateway SDK; Supabase (PostgreSQL, storage, auth) for off-chain evidence and metadata
Front end: React, built with Lovable
Evidence processing: OCR plus LLM-based field extraction with human review; SHA-256 hashing
Screening: OFAC, UN and Indian sanctions lists
Payments: a UPI integration layer for NPCI APIs, mocked during the prototype
DevOps: Docker, GitHub Actions

**Expected Impact**

Investor protection: retail investors buying fractions through UPI see exactly what is proven, disputed or missing before they pay.
Fraud reduction: tokens cannot be minted on assets with unproven title or hidden encumbrances, and existing tokens pause when a problem appears.
Lower diligence cost: one shared, reusable record replaces repeated manual checks by each platform, bank and investor.
Regulatory confidence: gives SEBI, IFSCA and RBI-regulated entities a standard, auditable disclosure format for tokenized assets.
Ecosystem enabler: any tokenization platform on Drunix can plug into KYA rather than build its own diligence, which speeds adoption of Drunix for real assets.
Scalable beyond real estate: the same model applies to gold, commodities, invoices and infrastructure assets, and cross-border to GIFT City and UAE markets.
