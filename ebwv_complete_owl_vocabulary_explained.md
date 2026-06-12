# Proposed Complete EBWV OWL Vocabulary — Conceptual Explanation and Modelling Rationale

**Role of this document:** explanatory design note for the proposed European Business Wallet Vocabulary (EBWV) OWL Vocabulary.  
**Primary audience:** business-domain experts, public-sector product owners, attestation designers, ontology engineers, JSON-LD/VC implementers and application-profile authors.  
**Version context:** based on the uploaded EBWV Turtle vocabulary dated 2026-06-08 and the modelling refinements discussed for economic operators, legal entities, public-sector bodies, sites, organisational subunits, evidence and attestations.

---

## Table of contents

1. [Purpose of the EBWV](#1-purpose-of-the-ebwv)
2. [Core design principle: semantics are independent of technical format](#2-core-design-principle-semantics-are-independent-of-technical-format)
3. [Layered architecture: SKOS, OWL, SHACL and serialisation artefacts](#3-layered-architecture-skos-owl-shacl-and-serialisation-artefacts)
4. [Relationship to EU Core Vocabularies and W3C ORG](#4-relationship-to-eu-core-vocabularies-and-w3c-org)
5. [Current uploaded EBWV: useful elements and needed corrections](#5-current-uploaded-ebwv-useful-elements-and-needed-corrections)
6. [Proposed complete class model](#6-proposed-complete-class-model)
7. [Core subject model for Business Wallets](#7-core-subject-model-for-business-wallets)
8. [Economic operator model](#8-economic-operator-model)
9. [Legal entity, legal person, natural person and sole trader](#9-legal-entity-legal-person-natural-person-and-sole-trader)
10. [Public-sector and Union entities](#10-public-sector-and-union-entities)
11. [Organisation, organisational unit, branch, establishment and local unit](#11-organisation-organisational-unit-branch-establishment-and-local-unit)
12. [Sites, addresses and locations](#12-sites-addresses-and-locations)
13. [Identifiers and the corrected use of legalIdentifier](#13-identifiers-and-the-corrected-use-of-legalidentifier)
14. [Names and naming properties](#14-names-and-naming-properties)
15. [Attestations, credentials and attestation subjects](#15-attestations-credentials-and-attestation-subjects)
16. [Evidence and verifiable supporting material](#16-evidence-and-verifiable-supporting-material)
17. [Periods, dates, validity and temporal coverage](#17-periods-dates-validity-and-temporal-coverage)
18. [Monetary amount and quantitative business values](#18-monetary-amount-and-quantitative-business-values)
19. [Role, mandate, authorisation and representation concepts](#19-role-mandate-authorisation-and-representation-concepts)
20. [Sector-specific attestation modules](#20-sector-specific-attestation-modules)
21. [Recommended core object properties](#21-recommended-core-object-properties)
22. [Recommended core datatype properties](#22-recommended-core-datatype-properties)
23. [Recommended SKOS concept schemes](#23-recommended-skos-concept-schemes)
24. [Use in W3C Verifiable Credentials and JSON-LD @context](#24-use-in-w3c-verifiable-credentials-and-json-ld-context)
25. [Use in SD-JWT, JSON Schema, XML, APIs and other formats](#25-use-in-sd-jwt-json-schema-xml-apis-and-other-formats)
26. [SHACL application profiles and validation](#26-shacl-application-profiles-and-validation)
27. [Naming and modelling conventions](#27-naming-and-modelling-conventions)
28. [Recommended implementation roadmap](#28-recommended-implementation-roadmap)
29. [Appendix A: proposed core class list](#appendix-a-proposed-core-class-list)
30. [Appendix B: proposed core property list](#appendix-b-proposed-core-property-list)
31. [Appendix C: example W3C VC credentialSubject using EBWV semantics](#appendix-c-example-w3c-vc-credentialsubject-using-ebwv-semantics)
32. [Appendix D: references](#appendix-d-references)

---

## 1. Purpose of the EBWV

The **European Business Wallet Vocabulary (EBWV)** should provide a reusable semantic foundation for electronic attestations, verifiable credentials and other machine-processable business data products used in the European Business Wallet ecosystem.

The EBWV should not be designed as a vocabulary only for one attestation type such as an ESG certificate, VAT registration, company certificate, posted worker notification or certificate of origin. Instead, it should provide a **generic business semantics layer** that can be reused across many wallet-enabled business processes.

The EBWV should support, among others:

- identification of Business Wallet owners;
- identification of economic operators, public-sector bodies and Union entities;
- company certificates and business-register attestations;
- VAT, EORI, tax and sectoral identifiers;
- industry classifications such as NACE and national equivalents;
- social-security and posted-worker attestations;
- mandates, authorisations, powers of attorney and representational rights;
- trade, logistics, customs and supply-chain attestations;
- Digital Product Passport evidence chains;
- ESG and sustainability-related certificates;
- bank-account, payment and financial attestations;
- public-administration submissions and B2G data exchange.

The vocabulary should make the meaning of data elements clear and reusable regardless of whether the physical payload is a W3C Verifiable Credential, JSON-LD, SD-JWT, XML, CSV, API response, database record or PDF-generated human-readable representation.

---

## 2. Core design principle: semantics are independent of technical format

The central design principle is:

> **The semantic meaning of a business attribute must remain the same regardless of the technical format used to transmit, store, sign, disclose or display the data.**

This means that the EBWV OWL Vocabulary should define the shared meanings of classes and properties. Technical formats should then bind to those meanings.

For example, the concept of a legal identifier should remain the same in all of the following representations:

```json
{
  "legalIdentifier": {
    "identifier": "FI1234567-8",
    "scheme": "Finnish Business ID"
  }
}
```

```turtle
:company ebwv:legalIdentifier :identifier123 .
:identifier123 skos:notation "FI1234567-8" .
```

```xml
<LegalIdentifier scheme="FinnishBusinessID">FI1234567-8</LegalIdentifier>
```

The payload syntax changes, but the semantic reference remains the EBWV/EU Core-compatible property `ebwv:legalIdentifier`.

For W3C Verifiable Credentials, the JSON-LD `@context` can bind JSON keys to EBWV IRIs. For SD-JWT or mDoc-style formats, the semantic binding can be expressed through a schema-level reference, a `semanticDataSpecification` URL, `$comment`, documentation metadata or implementation rulebook. For XML and APIs, the same vocabulary can be referenced in schema annotations, OpenAPI descriptions, documentation or mapping tables.

---

## 3. Layered architecture: SKOS, OWL, SHACL and serialisation artefacts

The EBWV should be understood as a layered semantic foundation.

### 3.1 SKOS Terminology layer

The SKOS terminology layer defines business terms and controlled concepts. It should include preferred labels, alternative labels, definitions, notes and semantic relations such as broader, narrower and related concepts.

Examples:

- economic operator;
- sole trader;
- self-employed person;
- legal entity;
- legal identifier;
- branch;
- establishment;
- local unit;
- registered site;
- VAT identifier;
- EORI identifier;
- certification scope;
- evidence type;
- attestation legal category.

SKOS is especially suitable for:

- controlled lists;
- business terminology;
- multilingual labels;
- non-strict conceptual relations;
- code lists and enumerations.

### 3.2 OWL Vocabulary layer

The OWL vocabulary defines reusable classes and properties. It should be generic and stable.

Examples:

- `ebwv:EconomicOperator`;
- `ebwv:LegalEntity`;
- `ebwv:PublicSectorBody`;
- `ebwv:Site`;
- `ebwv:Address`;
- `ebwv:ElectronicAttestationOfAttributes`;
- `ebwv:legalIdentifier`;
- `ebwv:businessIdentifier`;
- `ebwv:attestationSubject`;
- `ebwv:hasSite`.

The OWL layer should define broad semantic meaning, not overly specific payload constraints. OWL domain and range statements should be helpful but not so rigid that they prevent cross-domain reuse.

### 3.3 SHACL application-profile layer

SHACL shapes define attestation-specific constraints. This is where cardinalities, mandatory fields, datatypes, controlled code-list restrictions and profile-specific ranges should be expressed.

Examples:

- a Company Certificate must have exactly one legal identifier;
- a VAT attestation must have one VAT identifier;
- a Posted Worker Notification must include an employer, posted worker and assignment period;
- an ESG attestation must include at least one certification scope and one evidence reference;
- a Power of Attorney must include principal, authorised actor, scope, validity and possible restrictions.

### 3.4 Serialisation and implementation layer

This layer contains JSON-LD contexts, JSON Schemas, SD-JWT claim definitions, XML schemas, OpenAPI specifications and other technical artefacts.

These artefacts should not redefine the meaning of the data. They should bind physical data fields to the semantic definitions in the EBWV.

---

## 4. Relationship to EU Core Vocabularies and W3C ORG

The EBWV should use the EU Core Vocabularies as a foundation whenever this does not create logical inconsistency.

The **Core Business Vocabulary** is a simplified, reusable and extensible data model that captures fundamental characteristics of legal entities, including legal name, activity and address. It is focused on formally registered legal entities and does not cover all possible economic operators, such as sole traders or other non-registered agents able to do business.

The **Core Person Vocabulary** should be used as the conceptual foundation for natural-person attributes such as given name, family name, birth date, citizenship and residency.

The **Core Location Vocabulary** should be used as the foundation for addresses and geographic locations.

The **Core Public Organisation Vocabulary** is relevant for public-sector bodies and public organisations.

The **W3C Organization Ontology** is useful for organisational structures, organisational units and sites. However, it should not be the root model for `ebwv:EconomicOperator`, because an economic operator in the Business Wallet context can also be a natural person, sole trader, self-employed person, group of persons or temporary association.

The EBWV should therefore use this hierarchy of foundations:

1. EU Core Vocabularies where logically suitable;
2. W3C ORG where organisational hierarchy and sites are needed;
3. EBWV-specific classes where Business Wallet concepts are broader or more specific than the existing vocabularies;
4. SKOS concept schemes for controlled classifications and business terminology.

---

## 5. Current uploaded EBWV: useful elements and needed corrections

The uploaded EBWV already contains several useful reusable concepts:

- `ebwv:ElectronicAttestationOfAttributes`;
- `ebwv:EuropeanBusinessWalletOwner`;
- `ebwv:EconomicOperator`;
- `ebwv:LegalEntity`;
- `ebwv:LegalPerson`;
- `ebwv:NaturalPerson`;
- `ebwv:Person`;
- `ebwv:Address`;
- `ebwv:Location`;
- `ebwv:Site`;
- `ebwv:ContactPoint`;
- `ebwv:LegalEntityIdentifier`;
- `ebwv:MonetaryAmount`;
- `ebwv:PeriodOfTime`;
- `ebwv:ScopeOfAuthorization`;
- `ebwv:BankAccount`;
- `ebwv:PaymentTerms`;
- `ebwv:A1Certificate`;
- `ebwv:PostedWorkerNotification`;
- `ebwv:SocialSecurityContribution`.

It also contains important properties such as:

- `ebwv:identifier`;
- `ebwv:legalIdentifier`;
- `ebwv:legalName`;
- `ebwv:registeredAddress`;
- `ebwv:givenName`;
- `ebwv:familyName`;
- `ebwv:dateOfBirth`;
- `ebwv:domicile`;
- `ebwv:residency`;
- `ebwv:thoroughfare`;
- `ebwv:locatorDesignator`;
- `ebwv:postCode`;
- `ebwv:postName`;
- `ebwv:hasSite`;
- `ebwv:scopeOfAuthorization`;
- `ebwv:startDate`;
- `ebwv:endDate`;
- `ebwv:amount`;
- `ebwv:currency`.

However, the uploaded vocabulary also needs correction and extension.

### 5.1 NaturalPerson should not be a subclass of LegalEntity

The uploaded vocabulary currently models `ebwv:NaturalPerson` as a subclass of `ebwv:LegalEntity`. This is not recommended. A natural person can be a legal subject and can act as an economic operator, but should not be treated as a legal entity in the Core Business Vocabulary sense.

Recommended correction:

```turtle
ebwv:NaturalPerson
    rdfs:subClassOf ebwv:Person .
```

For a sole trader:

```turtle
ebwv:SoleTrader
    rdfs:subClassOf ebwv:NaturalPerson ;
    rdfs:subClassOf ebwv:EconomicOperator .
```

### 5.2 LegalPerson should not simply be a subclass of LegalEntity without clarification

`LegalPerson` and `LegalEntity` overlap in many legal and business contexts, but they are not always used identically. A legal person is a non-human subject recognised by law; a legal entity in the EU Core Business Vocabulary context is normally a formally registered trading body appearing in a business register.

Recommended model:

```turtle
ebwv:LegalEntity
    rdfs:subClassOf ebwv:EconomicOperator .

ebwv:LegalPerson
    rdfs:subClassOf ebwv:EconomicOperator .
```

If EBWV needs both concepts, define their scope explicitly.

### 5.3 EconomicOperator must remain broader than LegalEntity

`ebwv:EconomicOperator` should be the central EBWV business-process subject. It must not be treated as equivalent to `LegalEntity` or `org:Organization`.

It covers:

- legal entities;
- companies;
- sole traders;
- self-employed persons;
- natural persons acting commercially or professionally;
- groups of persons;
- temporary associations of undertakings;
- organisations acting economically;
- possibly public-sector bodies when acting in an economic capacity.

### 5.4 hasSite is too narrow in the uploaded vocabulary

The uploaded vocabulary currently uses `ebwv:hasSite` with the domain `SocialSecurityContribution`. This is too narrow for a generic EBWV property.

Recommended correction:

```turtle
ebwv:hasSite
    rdfs:domain ebwv:BusinessWalletOwner ;
    rdfs:range ebwv:Site .
```

If needed, a social-security-specific subproperty can be added.

### 5.5 Missing high-value classes

The vocabulary should add at least:

- `ebwv:BusinessWalletOwner` or rename/clarify `ebwv:EuropeanBusinessWalletOwner`;
- `ebwv:PublicSectorBody`;
- `ebwv:UnionEntity`;
- `ebwv:SoleTrader`;
- `ebwv:SelfEmployedPerson`;
- `ebwv:GroupOfPersons`;
- `ebwv:TemporaryAssociation`;
- `ebwv:Organisation`;
- `ebwv:OrganisationalUnit`;
- `ebwv:Branch`;
- `ebwv:Establishment`;
- `ebwv:LocalUnit`;
- `ebwv:Evidence`;
- `ebwv:CertificationScope`;
- `ebwv:EconomicOperatorRole`.

---

## 6. Proposed complete class model

The proposed complete EBWV class model should be built around the entities that participate in Business Wallet processes and the data products that attest facts about them.

### 6.1 High-level structure

```text
ebwv:BusinessWalletOwner
├─ ebwv:EconomicOperator
├─ ebwv:PublicSectorBody
└─ ebwv:UnionEntity

Business-process actors and identity forms:
├─ ebwv:Person
│  └─ ebwv:NaturalPerson
│     ├─ ebwv:SoleTrader
│     └─ ebwv:SelfEmployedPerson
├─ ebwv:LegalPerson
├─ ebwv:LegalEntity
├─ ebwv:Organisation
│  ├─ ebwv:OrganisationalUnit
│  ├─ ebwv:Branch
│  └─ ebwv:Establishment
│     └─ ebwv:LocalUnit
├─ ebwv:GroupOfPersons
│  └─ ebwv:TemporaryAssociation
└─ ebwv:EconomicOperatorRole

Reusable supporting objects:
├─ ebwv:Identifier
├─ ebwv:Address
├─ ebwv:Location
├─ ebwv:Site
├─ ebwv:ContactPoint
├─ ebwv:PeriodOfTime
├─ ebwv:MonetaryAmount
├─ ebwv:Evidence
├─ ebwv:CertificationScope
└─ ebwv:ScopeOfAuthorization

Attestation/data-product classes:
├─ ebwv:ElectronicAttestationOfAttributes
├─ ebwv:CompanyCertificate
├─ ebwv:VATRegistrationUnit
├─ ebwv:A1Certificate
├─ ebwv:PostedWorkerNotification
├─ ebwv:SocialSecurityContribution
├─ ebwv:SustainabilityAttestation
├─ ebwv:BankAccountAttestation
├─ ebwv:TaxResidencyCertificate
├─ ebwv:PowerOfAttorney
└─ ebwv:TradeDocumentAttestation
```

This model is not meant to force a rigid single-inheritance taxonomy. OWL allows multiple typing. For example, an individual can be typed both as `ebwv:SoleTrader` and `ebwv:EconomicOperator`.

---

## 7. Core subject model for Business Wallets

The Business Wallet ecosystem requires a clear subject model.

### 7.1 BusinessWalletOwner

`ebwv:BusinessWalletOwner` is the superclass for entities that own or have the right to use a European Business Wallet.

Recommended definition:

> A Business Wallet owner is an entity that owns or has the right to use a European Business Wallet, including economic operators, public-sector bodies and Union entities where applicable.

Recommended class:

```turtle
ebwv:BusinessWalletOwner
    a owl:Class ;
    rdfs:label "Business Wallet owner"@en ;
    rdfs:comment "An entity that owns or has the right to use a European Business Wallet, including economic operators, public-sector bodies and Union entities where applicable."@en .
```

If the vocabulary keeps the existing `ebwv:EuropeanBusinessWalletOwner`, it should either be renamed to `ebwv:BusinessWalletOwner` for simplicity or defined as an equivalent class.

```turtle
ebwv:EuropeanBusinessWalletOwner owl:equivalentClass ebwv:BusinessWalletOwner .
```

### 7.2 Attestation subject

Business Wallet credentials should have a clear domain-level subject relation, distinct from the W3C VC structural relation.

Recommended property:

```turtle
ebwv:attestationSubject
    a owl:ObjectProperty ;
    rdfs:domain ebwv:ElectronicAttestationOfAttributes ;
    rdfs:range ebwv:BusinessWalletOwner ;
    rdfs:label "attestation subject"@en ;
    rdfs:comment "Links an electronic attestation of attributes to the Business Wallet owner, economic operator, public-sector body, Union entity or other subject whose attributes are attested."@en .
```

Specialised subproperties:

```turtle
ebwv:economicOperator
    rdfs:subPropertyOf ebwv:attestationSubject ;
    rdfs:range ebwv:EconomicOperator .

ebwv:publicSectorBody
    rdfs:subPropertyOf ebwv:attestationSubject ;
    rdfs:range ebwv:PublicSectorBody .

ebwv:unionEntity
    rdfs:subPropertyOf ebwv:attestationSubject ;
    rdfs:range ebwv:UnionEntity .
```

---

## 8. Economic operator model

### 8.1 Why EconomicOperator is central

The EBWV should be **EconomicOperator-centred**, because most wallet-enabled business processes concern an entity acting in a trade, business, craft or professional capacity.

An economic operator may be:

- a company;
- a registered legal entity;
- a legal person;
- a sole trader;
- a self-employed professional;
- a natural person acting commercially or professionally;
- a group of persons;
- a temporary association of undertakings;
- an organisation acting economically;
- a public-sector body acting in a market or procurement-related economic capacity.

The class must therefore be broader than `LegalEntity`.

### 8.2 Recommended definition

```turtle
ebwv:EconomicOperator
    a owl:Class ;
    rdfs:subClassOf ebwv:BusinessWalletOwner ;
    rdfs:label "Economic operator"@en ;
    rdfs:comment "An entity acting in a commercial or professional capacity for purposes related to trade, business, craft or profession. Economic operators may include natural persons, sole traders, self-employed persons, legal persons, legal entities, organisations, groups of persons and temporary associations of undertakings."@en .
```

### 8.3 EconomicOperator is not an organisation superclass

Do not assert:

```turtle
ebwv:EconomicOperator rdfs:subClassOf org:Organization .
```

That would incorrectly imply that all economic operators are organisations. Sole traders and self-employed persons are natural persons acting economically, not organisations.

### 8.4 Economic operator type

Because economic operators can have very different legal and organisational forms, use a SKOS-controlled property:

```turtle
ebwv:economicOperatorType
    a owl:ObjectProperty ;
    rdfs:domain ebwv:EconomicOperator ;
    rdfs:range skos:Concept ;
    rdfs:label "economic operator type"@en ;
    rdfs:comment "Indicates the type or legal-organisational form of an economic operator, such as company, sole trader, self-employed person, partnership, public undertaking, consortium or temporary association."@en .
```

---

## 9. Legal entity, legal person, natural person and sole trader

### 9.1 LegalEntity

`ebwv:LegalEntity` should align with the EU Core Business Vocabulary understanding of a formally registered legal entity. It is one kind of economic operator, but not the full scope of economic operators.

Recommended definition:

```turtle
ebwv:LegalEntity
    a owl:Class ;
    rdfs:subClassOf ebwv:EconomicOperator ;
    rdfs:label "Legal entity"@en ;
    rdfs:comment "A formally registered or legally recognised entity that can bear rights and obligations and is identifiable in a business register, legal register or equivalent administrative context. A legal entity is one possible type of economic operator, but economic operators may also include natural persons, sole traders, groups of persons and temporary associations."@en .
```

### 9.2 LegalPerson

`ebwv:LegalPerson` should be used when the vocabulary needs the legal-theoretical distinction between human and non-human legal subjects.

```turtle
ebwv:LegalPerson
    a owl:Class ;
    rdfs:subClassOf ebwv:EconomicOperator ;
    rdfs:label "Legal person"@en ;
    rdfs:comment "A non-human legal subject recognised by law as capable of bearing rights and obligations, such as a company, association, foundation or other legally recognised body."@en .
```

In practical EBWV data products, `LegalEntity` will often be the more useful class.

### 9.3 Person and NaturalPerson

`ebwv:Person` is the general person class. `ebwv:NaturalPerson` is a human individual.

```turtle
ebwv:Person
    a owl:Class ;
    rdfs:label "Person"@en ;
    rdfs:comment "A human person described in a business, legal, administrative, employment, social security, representative or wallet-related context."@en .
```

```turtle
ebwv:NaturalPerson
    a owl:Class ;
    rdfs:subClassOf ebwv:Person ;
    rdfs:label "Natural person"@en ;
    rdfs:comment "A human individual. A natural person may act personally, as a representative, as a worker, or in a commercial or professional capacity as an economic operator."@en .
```

### 9.4 SoleTrader

```turtle
ebwv:SoleTrader
    a owl:Class ;
    rdfs:subClassOf ebwv:NaturalPerson ;
    rdfs:subClassOf ebwv:EconomicOperator ;
    rdfs:label "Sole trader"@en ;
    rdfs:comment "A natural person who conducts economic activity in their own name and capacity as a trade, business, craft or profession, without the sole-trader activity being a separate legal entity."@en .
```

### 9.5 SelfEmployedPerson

```turtle
ebwv:SelfEmployedPerson
    a owl:Class ;
    rdfs:subClassOf ebwv:NaturalPerson ;
    rdfs:subClassOf ebwv:EconomicOperator ;
    rdfs:label "Self-employed person"@en ;
    rdfs:comment "A natural person who carries out economic or professional activity on their own account and may act as an economic operator in wallet-based business or administrative processes."@en .
```

### 9.6 GroupOfPersons and TemporaryAssociation

```turtle
ebwv:GroupOfPersons
    a owl:Class ;
    rdfs:subClassOf ebwv:EconomicOperator ;
    rdfs:label "Group of persons"@en ;
    rdfs:comment "A group consisting of natural persons, legal persons or both, acting together in a commercial, professional, administrative or legal context without necessarily constituting a separate legal entity."@en .
```

```turtle
ebwv:TemporaryAssociation
    a owl:Class ;
    rdfs:subClassOf ebwv:GroupOfPersons ;
    rdfs:label "Temporary association"@en ;
    rdfs:comment "A temporary grouping or association of undertakings or other persons acting together as an economic operator for a specific contract, procedure, project, transaction or other business purpose, without necessarily constituting a separate legal entity."@en .
```

---

## 10. Public-sector and Union entities

### 10.1 PublicSectorBody

`ebwv:PublicSectorBody` is needed because Business Wallet owners are not limited to economic operators. Public bodies may own or use Business Wallets for public-sector interactions.

```turtle
ebwv:PublicSectorBody
    a owl:Class ;
    rdfs:subClassOf ebwv:BusinessWalletOwner ;
    rdfs:label "Public sector body"@en ;
    rdfs:comment "A public authority, body, office, agency or other public-sector organisation that may own or use a European Business Wallet in public-sector, administrative or cross-border interactions."@en .
```

### 10.2 UnionEntity

`ebwv:UnionEntity` covers EU institutions, bodies, offices and agencies where they participate as wallet owners or wallet-relevant public entities.

```turtle
ebwv:UnionEntity
    a owl:Class ;
    rdfs:subClassOf ebwv:BusinessWalletOwner ;
    rdfs:label "Union entity"@en ;
    rdfs:comment "An institution, body, office, agency or other entity of the European Union that may own or use a European Business Wallet or participate in wallet-based business and administrative processes."@en .
```

### 10.3 Public body acting economically

A public-sector body should not automatically be a subclass of `EconomicOperator`. Instead, it can be typed as both when the process context requires it:

```turtle
:municipalUtility
    a ebwv:PublicSectorBody, ebwv:EconomicOperator .
```

Alternatively, a role model can be used:

```turtle
:municipalAuthority ebwv:actsInCapacity ebwvt:economicOperator .
```

---

## 11. Organisation, organisational unit, branch, establishment and local unit

These classes support organisational structure. They are important for many business processes but should not be confused with the broader `EconomicOperator` concept.

### 11.1 Organisation

```turtle
ebwv:Organisation
    a owl:Class ;
    rdfs:label "Organisation"@en ;
    rdfs:subClassOf org:Organization ;
    rdfs:comment "A structured collective actor with a common purpose, governance, membership, administrative structure or operational identity. An organisation may or may not be a legal entity and may act as an economic operator where it acts in a commercial or professional capacity."@en .
```

### 11.2 OrganisationalUnit

```turtle
ebwv:OrganisationalUnit
    a owl:Class ;
    rdfs:subClassOf ebwv:Organisation ;
    rdfs:subClassOf org:OrganizationalUnit ;
    rdfs:label "Organisational unit"@en ;
    rdfs:comment "An internal subdivision of an organisation, legal entity, public-sector body or economic operator, created for management, operational, reporting or administrative purposes and normally lacking separate legal personality."@en .
```

### 11.3 Branch

A branch is not merely a site. It is a dependent business or organisational presence, often registered or recognised in a jurisdiction.

```turtle
ebwv:Branch
    a owl:Class ;
    rdfs:subClassOf ebwv:Organisation ;
    rdfs:label "Branch"@en ;
    rdfs:comment "A dependent business or organisational presence of a legal entity or other economic operator, often registered or recognised in a jurisdiction, without necessarily being a separate legal entity."@en .
```

### 11.4 Establishment

An establishment is an economic or operational unit through which activity is carried out.

```turtle
ebwv:Establishment
    a owl:Class ;
    rdfs:subClassOf ebwv:Organisation ;
    rdfs:label "Establishment"@en ;
    rdfs:comment "A business, operational or economic unit through which an economic operator carries out activity, usually at or from a specific site, and which may be relevant for statistical, tax, employment, regulatory, certification or reporting purposes."@en .
```

### 11.5 LocalUnit

```turtle
ebwv:LocalUnit
    a owl:Class ;
    rdfs:subClassOf ebwv:Establishment ;
    rdfs:label "Local unit"@en ;
    rdfs:comment "A geographically identified establishment or part of an economic operator used for statistical, register, employment, tax or reporting purposes."@en .
```

### 11.6 Recommended relationships

```turtle
ebwv:hasUnit
    rdfs:domain ebwv:Organisation ;
    rdfs:range ebwv:OrganisationalUnit .

ebwv:unitOf
    rdfs:domain ebwv:OrganisationalUnit ;
    rdfs:range ebwv:Organisation .

ebwv:branchOf
    rdfs:domain ebwv:Branch ;
    rdfs:range ebwv:EconomicOperator .

ebwv:establishmentOf
    rdfs:domain ebwv:Establishment ;
    rdfs:range ebwv:EconomicOperator .
```

---

## 12. Sites, addresses and locations

### 12.1 Location

`ebwv:Location` should align with the Core Location Vocabulary idea of a location described by an address, geographic name or geometry.

```turtle
ebwv:Location
    a owl:Class ;
    rdfs:label "Location"@en ;
    rdfs:comment "An identifiable geographic or administrative location, which may be represented by an address, geographic name, geographic identifier or geometry."@en .
```

### 12.2 Address

`ebwv:Address` is a structured address.

```turtle
ebwv:Address
    a owl:Class ;
    rdfs:label "Address"@en ;
    rdfs:comment "A structured representation of a location using address components such as thoroughfare, locator designator, post code, post name and administrative unit."@en .
```

Core address properties should include:

- `ebwv:fullAddress`;
- `ebwv:thoroughfare`;
- `ebwv:locatorDesignator`;
- `ebwv:locatorName`;
- `ebwv:postCode`;
- `ebwv:postName`;
- `ebwv:poBox`;
- `ebwv:adminUnitL1`;
- `ebwv:adminUnitL2`;
- `ebwv:addressArea`.

### 12.3 Site

A `Site` should be a place or premise where an actor has presence or where activity is carried out.

```turtle
ebwv:Site
    a owl:Class ;
    rdfs:subClassOf org:Site ;
    rdfs:label "Site"@en ;
    rdfs:comment "A physical, virtual or otherwise identifiable place, premise or location where a Business Wallet owner, economic operator, public-sector body, Union entity, organisational unit, activity or business process has presence or is carried out."@en .
```

`Site` should not be used as a synonym for branch, establishment, local unit or organisational unit. Those are organisational or economic units. A site is the place.

### 12.4 Site properties

```turtle
ebwv:hasSite
    a owl:ObjectProperty ;
    rdfs:domain ebwv:BusinessWalletOwner ;
    rdfs:range ebwv:Site ;
    rdfs:label "has site"@en ;
    rdfs:comment "Links a Business Wallet owner, including an economic operator, public-sector body or Union entity, to a site where it has a registered, operational, administrative, contact or other relevant presence."@en .
```

```turtle
ebwv:siteAddress
    a owl:ObjectProperty ;
    rdfs:domain ebwv:Site ;
    rdfs:range ebwv:Address ;
    rdfs:label "site address"@en ;
    rdfs:comment "Links a site to its address."@en .
```

Specialised site properties may include:

- `ebwv:registeredSite`;
- `ebwv:primarySite`;
- `ebwv:operatingSite`;
- `ebwv:productionSite`;
- `ebwv:workSite`;
- `ebwv:certifiedSite`;
- `ebwv:coveredSite`.

A SKOS `siteType` concept scheme may be better than many subclasses.

---

## 13. Identifiers and the corrected use of legalIdentifier

### 13.1 Identifier

The uploaded vocabulary has `ebwv:identifier`, but a complete EBWV should use a structured identifier class, preferably aligned with `adms:Identifier`.

```turtle
ebwv:Identifier
    a owl:Class ;
    rdfs:subClassOf adms:Identifier ;
    rdfs:label "Identifier"@en ;
    rdfs:comment "A structured reference used to identify an entity, role, site, attestation, evidence object or other business-relevant resource, including its value, scheme, issuing authority and jurisdiction where relevant."@en .
```

Recommended properties for identifiers:

- `ebwv:identifierValue`;
- `ebwv:identifierScheme`;
- `ebwv:issuingAuthority`;
- `ebwv:jurisdiction`;
- `ebwv:issuedDate`;
- `ebwv:validFrom`;
- `ebwv:validUntil`.

### 13.2 legalIdentifier

Use `legalIdentifier`, not `registeredIdentifier`.

The EU Core Business Vocabulary uses **legal identifier** for the unambiguous structured reference assigned to the legal entity by the legal authority that registered it.

Recommended property:

```turtle
ebwv:legalIdentifier
    a owl:ObjectProperty ;
    rdfs:subPropertyOf ebwv:identifier ;
    rdfs:domain ebwv:LegalEntity ;
    rdfs:range ebwv:Identifier ;
    rdfs:label "legal identifier"@en ;
    rdfs:comment "The unambiguous structured reference assigned to a legal entity by the legal authority or register that registered it."@en .
```

### 13.3 businessIdentifier

`businessIdentifier` is broader and can apply to all economic operators.

```turtle
ebwv:businessIdentifier
    a owl:ObjectProperty ;
    rdfs:subPropertyOf ebwv:identifier ;
    rdfs:domain ebwv:EconomicOperator ;
    rdfs:range ebwv:Identifier ;
    rdfs:label "business identifier"@en ;
    rdfs:comment "An identifier used to identify an economic operator in business, tax, customs, trade, statistical, regulatory or administrative processes."@en .
```

Specialised subproperties:

```turtle
ebwv:vatIdentifier rdfs:subPropertyOf ebwv:businessIdentifier .
ebwv:eoriIdentifier rdfs:subPropertyOf ebwv:businessIdentifier .
ebwv:taxIdentifier rdfs:subPropertyOf ebwv:businessIdentifier .
ebwv:leiIdentifier rdfs:subPropertyOf ebwv:legalIdentifier .
ebwv:euidIdentifier rdfs:subPropertyOf ebwv:legalIdentifier .
```

Use `legalIdentifier` only where the identifier is specifically the legally assigned registration identifier of a legal entity. Use `businessIdentifier` for broader economic-operator identifiers, especially where the subject may be a sole trader or self-employed person.

---

## 14. Names and naming properties

Different subject types require different name properties.

### 14.1 legalName

Use for legal entities.

```turtle
ebwv:legalName
    a owl:DatatypeProperty ;
    rdfs:domain ebwv:LegalEntity ;
    rdfs:range rdf:langString ;
    rdfs:label "legal name"@en ;
    rdfs:comment "The name under which a legal entity is registered or formally recognised."@en .
```

### 14.2 officialName

Use for public-sector bodies, Union entities, natural-person economic operators and other official administrative names.

```turtle
ebwv:officialName
    a owl:DatatypeProperty ;
    rdfs:domain ebwv:BusinessWalletOwner ;
    rdfs:range rdf:langString ;
    rdfs:label "official name"@en ;
    rdfs:comment "The official name of a Business Wallet owner or other subject in an administrative, legal or authoritative context."@en .
```

### 14.3 tradingName

Use for trade names, business names and market-facing names.

```turtle
ebwv:tradingName
    a owl:DatatypeProperty ;
    rdfs:domain ebwv:EconomicOperator ;
    rdfs:range rdf:langString ;
    rdfs:label "trading name"@en ;
    rdfs:comment "A name used by an economic operator in trade, commerce, professional activity or market-facing communication, which may differ from its legal or official name."@en .
```

### 14.4 Person names

The uploaded properties `givenName`, `familyName`, `birthName`, `patronymicName`, `matronymicName` and `fullName` are useful. Their domain should be `ebwv:Person` or `ebwv:NaturalPerson`, not an anonymous restriction.

---

## 15. Attestations, credentials and attestation subjects

### 15.1 ElectronicAttestationOfAttributes

The uploaded vocabulary already contains `ebwv:ElectronicAttestationOfAttributes` as a subclass of `cred:VerifiableCredential`. This is useful for W3C VC use.

However, the semantic class should also support non-VC formats. The class should be defined as the business-level concept, not only as a VC technical object.

Recommended definition:

```turtle
ebwv:ElectronicAttestationOfAttributes
    a owl:Class ;
    rdfs:subClassOf cred:VerifiableCredential ;
    rdfs:label "Electronic attestation of attributes"@en ;
    rdfs:comment "A digitally represented assertion or set of assertions about a subject, issued by an issuer and intended to be exchanged, verified or relied upon in a business, legal or administrative process. In W3C VC implementations it may be represented as a Verifiable Credential, while the same semantics may also be used in other technical formats."@en .
```

If the vocabulary wants to remain strictly format-neutral, define a superclass:

```turtle
ebwv:Attestation
    a owl:Class ;
    rdfs:label "Attestation"@en ;
    rdfs:comment "A statement or data product issued by an authority or trusted party that confirms one or more attributes or facts about a subject."@en .

ebwv:ElectronicAttestationOfAttributes
    rdfs:subClassOf ebwv:Attestation .
```

### 15.2 Attestation legal category

The uploaded `ebwv:AttestationLegalCategory` is useful, but it should preferably be a SKOS concept scheme rather than a pure OWL class unless there are individuals/classes with reasoning needs.

Possible categories:

- EAA;
- QEAA;
- Pub-EAA;
- sectoral attestation;
- self-declaration;
- registry extract;
- public certificate;
- private certificate;
- third-party assurance statement.

### 15.3 Attestation subject and issuer

Recommended core properties:

```turtle
ebwv:attestationSubject
    rdfs:domain ebwv:Attestation ;
    rdfs:range ebwv:BusinessWalletOwner .
```

```turtle
ebwv:issuer
    rdfs:domain ebwv:Attestation ;
    rdfs:range ebwv:BusinessWalletOwner .
```

```turtle
ebwv:issuedDate
    rdfs:domain ebwv:Attestation ;
    rdfs:range xsd:dateTime .
```

```turtle
ebwv:validFrom
    rdfs:domain ebwv:Attestation ;
    rdfs:range xsd:date .
```

```turtle
ebwv:validUntil
    rdfs:domain ebwv:Attestation ;
    rdfs:range xsd:date .
```

---

## 16. Evidence and verifiable supporting material

Evidence is central for Business Wallet processes. Many credentials do not merely state facts; they should also point to supporting evidence, such as a registry record, certificate, audit report, dataset, file, signed document or authoritative API response.

### 16.1 Evidence class

```turtle
ebwv:Evidence
    a owl:Class ;
    rdfs:label "Evidence"@en ;
    rdfs:comment "A structured reference to material supporting an attestation, such as a registry extract, audit report, certification document, assurance statement, dataset, file, API response or other source used to substantiate the attested facts."@en .
```

### 16.2 Evidence properties

```turtle
ebwv:hasEvidence
    a owl:ObjectProperty ;
    rdfs:domain ebwv:Attestation ;
    rdfs:range ebwv:Evidence ;
    rdfs:label "has evidence"@en ;
    rdfs:comment "Links an attestation to supporting evidence."@en .
```

```turtle
ebwv:evidenceIdentifier
    a owl:ObjectProperty ;
    rdfs:domain ebwv:Evidence ;
    rdfs:range ebwv:Identifier ;
    rdfs:label "evidence identifier"@en .
```

```turtle
ebwv:evidenceType
    a owl:ObjectProperty ;
    rdfs:domain ebwv:Evidence ;
    rdfs:range skos:Concept ;
    rdfs:label "evidence type"@en .
```

```turtle
ebwv:digestMultibase
    a owl:DatatypeProperty ;
    rdfs:domain ebwv:Evidence ;
    rdfs:range xsd:string ;
    rdfs:label "digest multibase"@en ;
    rdfs:comment "A multibase-encoded digest of the evidence object."@en .
```

```turtle
ebwv:evidenceLocation
    a owl:DatatypeProperty ;
    rdfs:domain ebwv:Evidence ;
    rdfs:range xsd:anyURI ;
    rdfs:label "evidence location"@en .
```

```turtle
ebwv:evidenceData
    a owl:DatatypeProperty ;
    rdfs:domain ebwv:Evidence ;
    rdfs:range xsd:base64Binary ;
    rdfs:label "evidence data"@en ;
    rdfs:comment "Embedded evidence data encoded as base64. In many implementations, an external evidence location plus digest is preferable to embedding the full evidence object."@en .
```

---

## 17. Periods, dates, validity and temporal coverage

The uploaded `ebwv:PeriodOfTime`, `ebwv:startDate` and `ebwv:endDate` are useful. The vocabulary should distinguish between:

- the period during which an attestation is valid;
- the period covered by the attested facts;
- the date of issuance;
- the date of registration;
- the date of last update;
- the date of expiry or renewal.

Recommended properties:

```turtle
ebwv:periodOfTime
    rdfs:range ebwv:PeriodOfTime .

ebwv:validityPeriod
    rdfs:domain ebwv:Attestation ;
    rdfs:range ebwv:PeriodOfTime .

ebwv:coveredPeriod
    rdfs:domain ebwv:Attestation ;
    rdfs:range ebwv:PeriodOfTime .

ebwv:startDate
    rdfs:domain ebwv:PeriodOfTime ;
    rdfs:range xsd:date .

ebwv:endDate
    rdfs:domain ebwv:PeriodOfTime ;
    rdfs:range xsd:date .

ebwv:validFrom
    rdfs:domain ebwv:Attestation ;
    rdfs:range xsd:date .

ebwv:validUntil
    rdfs:domain ebwv:Attestation ;
    rdfs:range xsd:date .
```

Use `validFrom` and `validUntil` for direct attestation validity. Use `PeriodOfTime` where the period is itself a reusable object with additional semantics.

---

## 18. Monetary amount and quantitative business values

The uploaded `ebwv:MonetaryAmount` is valuable and should be retained.

Recommended definition:

```turtle
ebwv:MonetaryAmount
    a owl:Class ;
    rdfs:label "Monetary amount"@en ;
    rdfs:comment "A structured representation of a sum of money, consisting at minimum of a numeric amount and the currency in which that amount is expressed."@en .
```

Recommended properties:

```turtle
ebwv:amount
    rdfs:domain ebwv:MonetaryAmount ;
    rdfs:range xsd:decimal .

ebwv:currency
    rdfs:domain ebwv:MonetaryAmount ;
    rdfs:range skos:Concept .
```

Use `xsd:decimal`, not `xsd:float`, for monetary values where precision matters.

---

## 19. Role, mandate, authorisation and representation concepts

EBWV should support authorisation and representation as a generic capability, not just as a document type.

### 19.1 ScopeOfAuthorization

The uploaded `ebwv:ScopeOfAuthorization` is useful. It should be part of a broader authorisation model.

```turtle
ebwv:ScopeOfAuthorization
    a owl:Class ;
    rdfs:label "Scope of authorization"@en ;
    rdfs:comment "A structured description of the actions, transactions, services, data, procedures, roles or business processes that an authorised actor is permitted to perform on behalf of another actor."@en .
```

### 19.2 PowerOfAttorney / Authorisation

Recommended classes:

```turtle
ebwv:Authorisation
    a owl:Class ;
    rdfs:subClassOf ebwv:Attestation ;
    rdfs:label "Authorisation"@en ;
    rdfs:comment "An attestation or statement that grants, confirms or records the authority of an actor to act, access data, submit information or perform actions in a defined capacity or scope."@en .
```

```turtle
ebwv:PowerOfAttorney
    a owl:Class ;
    rdfs:subClassOf ebwv:Authorisation ;
    rdfs:label "Power of attorney"@en ;
    rdfs:comment "An authorisation by which a principal grants another actor authority to act on the principal's behalf within a defined scope, period and set of restrictions."@en .
```

### 19.3 Core properties

```turtle
ebwv:principal
    rdfs:domain ebwv:Authorisation ;
    rdfs:range ebwv:BusinessWalletOwner .

ebwv:authorisedActor
    rdfs:domain ebwv:Authorisation ;
    rdfs:range ebwv:BusinessWalletOwner .

ebwv:scopeOfAuthorization
    rdfs:domain ebwv:Authorisation ;
    rdfs:range ebwv:ScopeOfAuthorization .

ebwv:restriction
    rdfs:domain ebwv:ScopeOfAuthorization ;
    rdfs:range rdf:langString .

ebwv:actsOnBehalfOf
    rdfs:domain ebwv:BusinessWalletOwner ;
    rdfs:range ebwv:BusinessWalletOwner .
```

---

## 20. Sector-specific attestation modules

The EBWV core should remain generic. Sector-specific classes should be modular extensions or clearly marked as application-domain classes.

### 20.1 Company certificate

The uploaded `ebwv:Company` currently relates to Directive 2025/25 use. For attestation design, use a separate attestation class:

```turtle
ebwv:CompanyCertificate
    rdfs:subClassOf ebwv:ElectronicAttestationOfAttributes .
```

The subject would be:

```turtle
ebwv:economicOperator → ebwv:LegalEntity .
```

### 20.2 VAT registration

`ebwv:VATRegistrationUnit` is useful but should be integrated with the broader economic-operator identifier model.

### 20.3 A1 certificate

`ebwv:A1Certificate` should be kept as a social-security-specific credential subject or attestation-specific subject node. It should use generic properties where possible:

- `ebwv:person`;
- `ebwv:employer`;
- `ebwv:placeOfWork`;
- `ebwv:coveredPeriod`;
- `ebwv:stateOfInsurance`.

### 20.4 Posted Worker Notification

`ebwv:PostedWorkerNotification` should reuse:

- `ebwv:employer`;
- `ebwv:postedWorker`;
- `ebwv:hostEntity`;
- `ebwv:assignment`;
- `ebwv:placeOfWork`;
- `ebwv:duration`.

### 20.5 Social Security Contribution

The uploaded class is useful. Recommended correction: the property should be `contributor`, not `contributer`.

```turtle
ebwv:contributor
    rdfs:domain ebwv:SocialSecurityContribution ;
    rdfs:range ebwv:EconomicOperator .
```

### 20.6 SustainabilityAttestation

This should not drive the core vocabulary, but it is a useful module.

```turtle
ebwv:SustainabilityAttestation
    rdfs:subClassOf ebwv:ElectronicAttestationOfAttributes .
```

It should use generic classes:

- `ebwv:EconomicOperator`;
- `ebwv:CertificationScope`;
- `ebwv:Site`;
- `ebwv:Evidence`;
- `ebwv:PeriodOfTime`.

---

## 21. Recommended core object properties

The following object properties form the semantic backbone of EBWV.

| Property | Domain | Range | Purpose |
|---|---|---|---|
| `ebwv:attestationSubject` | `ebwv:Attestation` | `ebwv:BusinessWalletOwner` | Generic subject of an attestation. |
| `ebwv:economicOperator` | `ebwv:Attestation` | `ebwv:EconomicOperator` | Economic operator subject. |
| `ebwv:issuer` | `ebwv:Attestation` | `ebwv:BusinessWalletOwner` | Issuer of the attestation. |
| `ebwv:identifier` | broad | `ebwv:Identifier` | Generic identifier relation. |
| `ebwv:legalIdentifier` | `ebwv:LegalEntity` | `ebwv:Identifier` | Legal identifier of a legal entity. |
| `ebwv:businessIdentifier` | `ebwv:EconomicOperator` | `ebwv:Identifier` | Business identifier of an economic operator. |
| `ebwv:registeredAddress` | `ebwv:BusinessWalletOwner` | `ebwv:Address` | Officially registered address. |
| `ebwv:contactPoint` | broad | `ebwv:ContactPoint` | Contact information. |
| `ebwv:hasSite` | `ebwv:BusinessWalletOwner` | `ebwv:Site` | Site relation. |
| `ebwv:siteAddress` | `ebwv:Site` | `ebwv:Address` | Address of a site. |
| `ebwv:hasUnit` | `ebwv:Organisation` | `ebwv:OrganisationalUnit` | Organisation-unit relation. |
| `ebwv:branchOf` | `ebwv:Branch` | `ebwv:EconomicOperator` | Parent economic operator of a branch. |
| `ebwv:establishmentOf` | `ebwv:Establishment` | `ebwv:EconomicOperator` | Parent economic operator of an establishment. |
| `ebwv:hasEvidence` | `ebwv:Attestation` | `ebwv:Evidence` | Supporting evidence. |
| `ebwv:validityPeriod` | `ebwv:Attestation` | `ebwv:PeriodOfTime` | Validity period. |
| `ebwv:coveredPeriod` | `ebwv:Attestation` | `ebwv:PeriodOfTime` | Period covered by facts. |
| `ebwv:scopeOfAuthorization` | `ebwv:Authorisation` | `ebwv:ScopeOfAuthorization` | Scope of an authorisation. |
| `ebwv:principal` | `ebwv:Authorisation` | `ebwv:BusinessWalletOwner` | Granting party. |
| `ebwv:authorisedActor` | `ebwv:Authorisation` | `ebwv:BusinessWalletOwner` | Actor receiving authority. |
| `ebwv:coveredSite` | `ebwv:CertificationScope` | `ebwv:Site` | Site included in a scope. |
| `ebwv:coveredOrganisation` | `ebwv:CertificationScope` | `ebwv:Organisation` | Organisation included in a scope. |

---

## 22. Recommended core datatype properties

| Property | Domain | Range | Purpose |
|---|---|---|---|
| `ebwv:legalName` | `ebwv:LegalEntity` | `rdf:langString` | Registered legal name. |
| `ebwv:officialName` | `ebwv:BusinessWalletOwner` | `rdf:langString` | Official name. |
| `ebwv:tradingName` | `ebwv:EconomicOperator` | `rdf:langString` | Trade or business name. |
| `ebwv:identifierValue` | `ebwv:Identifier` | `xsd:string` | Identifier value. |
| `ebwv:givenName` | `ebwv:Person` | `xsd:string` | Given name. |
| `ebwv:familyName` | `ebwv:Person` | `xsd:string` | Family name. |
| `ebwv:dateOfBirth` | `ebwv:Person` | `xsd:date` | Date of birth. |
| `ebwv:fullAddress` | `ebwv:Address` | `rdf:langString` | Full address text. |
| `ebwv:thoroughfare` | `ebwv:Address` | `xsd:string` | Street or thoroughfare. |
| `ebwv:locatorDesignator` | `ebwv:Address` | `xsd:string` | Building/house number. |
| `ebwv:postCode` | `ebwv:Address` | `xsd:string` | Postal code. |
| `ebwv:postName` | `ebwv:Address` | `xsd:string` | Postal area/city. |
| `ebwv:startDate` | `ebwv:PeriodOfTime` | `xsd:date` | Start date. |
| `ebwv:endDate` | `ebwv:PeriodOfTime` | `xsd:date` | End date. |
| `ebwv:validFrom` | `ebwv:Attestation` | `xsd:date` | First valid date. |
| `ebwv:validUntil` | `ebwv:Attestation` | `xsd:date` | Last valid date. |
| `ebwv:amount` | `ebwv:MonetaryAmount` | `xsd:decimal` | Numeric monetary amount. |
| `ebwv:digestMultibase` | `ebwv:Evidence` | `xsd:string` | Multibase digest. |
| `ebwv:evidenceData` | `ebwv:Evidence` | `xsd:base64Binary` | Embedded evidence data. |

Use `rdf:langString` for human-readable labels/descriptions that may require multilingual values. Use `xsd:string` for identifiers, codes and non-language-dependent textual values.

---

## 23. Recommended SKOS concept schemes

The EBWV should not model every controlled value as an OWL class. Use SKOS concept schemes for controlled values.

Recommended schemes:

| Concept scheme | Example concepts |
|---|---|
| Economic operator type | company, sole trader, self-employed person, partnership, temporary association, public undertaking |
| Legal form | limited liability company, public limited company, partnership, cooperative, foundation |
| Identifier type | legal identifier, VAT identifier, EORI identifier, LEI, EUID, tax identifier |
| Attestation legal category | EAA, QEAA, Pub-EAA, registry extract, self-declaration |
| Evidence type | registry extract, audit report, certification document, assurance statement, API response, dataset |
| Site type | registered site, primary site, operating site, production site, warehouse, office, work site |
| Authorisation scope type | signing, filing, tax submission, customs declaration, procurement participation, data access |
| Restriction type | monetary limit, time limit, joint action requirement, jurisdictional restriction, procedure restriction |
| Currency | EUR, SEK, NOK, DKK, USD, etc. |
| Country code | ISO country codes or EU-maintained country concept scheme |
| NACE activity | NACE Rev. 2.1 concepts |

---

## 24. Use in W3C Verifiable Credentials and JSON-LD @context

The EBWV should primarily serve W3C VC production through JSON-LD contexts, while remaining format-neutral.

### 24.1 JSON-LD context pattern

A JSON-LD context should map short JSON property names to EBWV IRIs.

Example:

```json
{
  "@context": {
    "ebwv": "https://w3id.org/ebwv#",
    "EconomicOperator": "ebwv:EconomicOperator",
    "LegalEntity": "ebwv:LegalEntity",
    "legalName": "ebwv:legalName",
    "legalIdentifier": {
      "@id": "ebwv:legalIdentifier",
      "@type": "@id"
    },
    "registeredAddress": {
      "@id": "ebwv:registeredAddress",
      "@type": "@id"
    },
    "businessIdentifier": {
      "@id": "ebwv:businessIdentifier",
      "@type": "@id"
    }
  }
}
```

### 24.2 Credential subject pattern

The `credentialSubject` should contain the data product subject. For most business attestations, this will be an economic operator.

```json
{
  "credentialSubject": {
    "id": "did:example:company123",
    "type": ["EconomicOperator", "LegalEntity"],
    "legalName": "Nordic Timber Oy",
    "legalIdentifier": {
      "id": "urn:identifier:fi:business-id:1234567-8",
      "identifierValue": "1234567-8",
      "identifierScheme": "Finnish Business ID"
    },
    "registeredAddress": {
      "type": "Address",
      "thoroughfare": "Tehtaankatu",
      "locatorDesignator": "12",
      "postCode": "00150",
      "postName": "Helsinki",
      "adminUnitL1": "FI"
    }
  }
}
```

### 24.3 Context does not replace the ontology

The JSON-LD context is a binding artefact. It does not define the vocabulary. The OWL vocabulary defines the terms; the context maps JSON keys to those terms.

---

## 25. Use in SD-JWT, JSON Schema, XML, APIs and other formats

The EBWV should also work for non-JSON-LD formats.

### 25.1 SD-JWT

In SD-JWT, the data may not carry RDF semantics inline. The schema, rulebook or metadata should therefore point to the semantic specification.

Possible pattern:

```json
{
  "legal_identifier": {
    "type": "object",
    "description": "The legal identifier of the legal entity.",
    "$comment": "Semantic reference: https://w3id.org/ebwv#legalIdentifier"
  }
}
```

A better profile-level approach is to add a rulebook-level semantic reference:

```json
{
  "semanticDataSpecification": "https://w3id.org/ebwv/specifications/company-certificate/1.0"
}
```

### 25.2 JSON Schema

JSON Schema should validate technical constraints. It should not become the semantic source of truth. Use `$comment`, `$id`, `$defs`, documentation tables or profile metadata to bind schema fields to EBWV properties.

### 25.3 XML

XML schemas can use annotations to reference EBWV IRIs.

```xml
<xs:element name="LegalIdentifier" type="xs:string">
  <xs:annotation>
    <xs:documentation>Semantic reference: https://w3id.org/ebwv#legalIdentifier</xs:documentation>
  </xs:annotation>
</xs:element>
```

### 25.4 APIs and databases

OpenAPI schemas, relational tables and message definitions should include mapping documentation from physical field names to EBWV terms.

Example:

| API field | EBWV property | Meaning |
|---|---|---|
| `legal_identifier` | `ebwv:legalIdentifier` | Legal identifier of the legal entity |
| `business_id` | `ebwv:businessIdentifier` | Business identifier of an economic operator |
| `registered_address` | `ebwv:registeredAddress` | Official registered address |

---

## 26. SHACL application profiles and validation

OWL should define generic reusable terms. SHACL should define data-product-specific constraints.

Example: Legal entity shape for a company certificate.

```turtle
ex:CompanyCertificateSubjectShape
    a sh:NodeShape ;
    sh:targetClass ebwv:LegalEntity ;
    sh:property [
        sh:path ebwv:legalIdentifier ;
        sh:minCount 1 ;
        sh:maxCount 1 ;
        sh:class ebwv:Identifier ;
    ] ;
    sh:property [
        sh:path ebwv:legalName ;
        sh:minCount 1 ;
        sh:datatype rdf:langString ;
    ] ;
    sh:property [
        sh:path ebwv:registeredAddress ;
        sh:minCount 1 ;
        sh:class ebwv:Address ;
    ] .
```

Example: sole trader profile.

```turtle
ex:SoleTraderSubjectShape
    a sh:NodeShape ;
    sh:targetClass ebwv:SoleTrader ;
    sh:property [
        sh:path ebwv:businessIdentifier ;
        sh:minCount 1 ;
        sh:class ebwv:Identifier ;
    ] ;
    sh:property [
        sh:path ebwv:officialName ;
        sh:minCount 1 ;
    ] ;
    sh:property [
        sh:path ebwv:tradingName ;
        sh:maxCount 1 ;
    ] .
```

---

## 27. Naming and modelling conventions

### 27.1 Class names

Use singular PascalCase:

- `EconomicOperator`;
- `LegalEntity`;
- `PublicSectorBody`;
- `BusinessWalletOwner`;
- `SoleTrader`;
- `TemporaryAssociation`;
- `ElectronicAttestationOfAttributes`.

### 27.2 Object properties

Use lower camelCase and descriptive names:

- `legalIdentifier`;
- `businessIdentifier`;
- `registeredAddress`;
- `hasSite`;
- `siteAddress`;
- `attestationSubject`;
- `hasEvidence`.

Avoid vague names such as `type` where a more precise property is needed. Prefer `economicOperatorType`, `evidenceType`, `siteType`, `attestationType`.

### 27.3 Domains and ranges

Keep OWL domains and ranges broad where the property is intended for reuse. Put strict cardinalities, mandatory use and datatype restrictions in SHACL.

### 27.4 Multilingual labels and definitions

Use:

- `rdfs:label` for technical labels;
- `skos:prefLabel` in terminology concepts;
- `rdfs:comment` for OWL definitions/descriptions;
- `skos:definition` and `skos:scopeNote` for SKOS concepts.

### 27.5 Datatypes

Use:

- `rdf:langString` for language-tagged names and descriptions;
- `xsd:string` for code-like strings;
- `xsd:date` for dates;
- `xsd:dateTime` for issuance timestamps;
- `xsd:decimal` for monetary amounts;
- `xsd:boolean` for true/false flags;
- `xsd:anyURI` for URI references.

---

## 28. Recommended implementation roadmap

### Phase 1: Correct and stabilise the core

- Correct `NaturalPerson` so it is not a subclass of `LegalEntity`.
- Add or clarify `BusinessWalletOwner`.
- Keep `EconomicOperator` central and broad.
- Add `PublicSectorBody` and `UnionEntity`.
- Correct `hasSite` domain.
- Add `Identifier` and clarify `legalIdentifier`.
- Add `officialName`, `tradingName`, `businessIdentifier`.

### Phase 2: Add organisational and location model

- Add `Organisation`, `OrganisationalUnit`, `Branch`, `Establishment`, `LocalUnit`.
- Align with W3C ORG where appropriate.
- Add `siteAddress`, `siteType`, `hasSite`, `locatedAt`.

### Phase 3: Add attestation and evidence model

- Add `Attestation` superclass if desired.
- Clarify `ElectronicAttestationOfAttributes`.
- Add `attestationSubject`, `issuer`, `validFrom`, `validUntil`, `coveredPeriod`.
- Add `Evidence`, `hasEvidence`, `evidenceType`, `digestMultibase`, `evidenceLocation`.

### Phase 4: Add authorisation and role model

- Add `Authorisation`, `PowerOfAttorney`, `EconomicOperatorRole`.
- Define `principal`, `authorisedActor`, `scopeOfAuthorization`, `actsOnBehalfOf`.

### Phase 5: Build application profiles

- Company Certificate profile.
- VAT profile.
- EORI profile.
- Tax Residency profile.
- Posted Worker Notification profile.
- A1 Certificate profile.
- Social Security Contribution profile.
- ESG/Sustainability Attestation profile.
- Power of Attorney profile.
- Trade-document profile.

### Phase 6: Publish serialisation artefacts

- OWL Turtle file.
- SKOS terminology Turtle file.
- HTML documentation.
- JSON-LD context files.
- SHACL shapes.
- JSON Schema/SD-JWT mappings.
- Example credentials.
- Human-readable documentation.

---

## Appendix A: proposed core class list

| Class | Status | Rationale |
|---|---|---|
| `ebwv:BusinessWalletOwner` | Add / clarify | Superclass for wallet-owning entities. |
| `ebwv:EconomicOperator` | Existing, central | Core EBWV business-process subject. |
| `ebwv:PublicSectorBody` | Add | Wallet owner category not necessarily economic operator. |
| `ebwv:UnionEntity` | Add | EU institution/body/agency wallet owner or participant. |
| `ebwv:Person` | Existing | General person concept. |
| `ebwv:NaturalPerson` | Existing, correct hierarchy | Human individual, not subclass of LegalEntity. |
| `ebwv:SoleTrader` | Add | Natural person acting as economic operator. |
| `ebwv:SelfEmployedPerson` | Add | Natural person acting professionally/economically on own account. |
| `ebwv:LegalPerson` | Existing, clarify | Non-human legal subject. |
| `ebwv:LegalEntity` | Existing, clarify | Core Business-compatible registered legal entity. |
| `ebwv:Organisation` | Add | Structured collective actor, aligned with ORG. |
| `ebwv:OrganisationalUnit` | Add | Internal unit of an organisation. |
| `ebwv:Branch` | Add | Dependent recognised business presence. |
| `ebwv:Establishment` | Add | Operational/economic unit. |
| `ebwv:LocalUnit` | Add | Geographically identified establishment or statistical unit. |
| `ebwv:GroupOfPersons` | Add | Group acting together. |
| `ebwv:TemporaryAssociation` | Add | Temporary association of undertakings/persons. |
| `ebwv:Identifier` | Add | Structured identifier, preferably ADMS-aligned. |
| `ebwv:Address` | Existing | Core Location-compatible address. |
| `ebwv:Location` | Existing | Generic location. |
| `ebwv:Site` | Existing, clarify | Place/premise where activity or presence exists. |
| `ebwv:ContactPoint` | Existing | Contact details. |
| `ebwv:PeriodOfTime` | Existing | Period object. |
| `ebwv:MonetaryAmount` | Existing | Amount + currency. |
| `ebwv:Attestation` | Add | Format-neutral attestation superclass. |
| `ebwv:ElectronicAttestationOfAttributes` | Existing | Electronic attestation, VC-compatible. |
| `ebwv:Evidence` | Add | Supporting evidence object. |
| `ebwv:CertificationScope` | Add | Scope covered by a certificate/attestation. |
| `ebwv:ScopeOfAuthorization` | Existing | Authorisation scope. |
| `ebwv:Authorisation` | Add | Generic authorisation attestation. |
| `ebwv:PowerOfAttorney` | Add | Specific authorisation granting authority. |

---

## Appendix B: proposed core property list

| Property | Status | Main use |
|---|---|---|
| `ebwv:attestationSubject` | Add | Subject of attestation. |
| `ebwv:economicOperator` | Add | Economic operator subject. |
| `ebwv:issuer` | Add | Issuer of attestation. |
| `ebwv:identifier` | Existing, clarify | Generic identifier relation. |
| `ebwv:legalIdentifier` | Existing, prefer | Legal identifier of legal entity. |
| `ebwv:businessIdentifier` | Add | Identifier for broader economic operator. |
| `ebwv:vatIdentifier` | Add | VAT identifier. |
| `ebwv:eoriIdentifier` | Add | EORI identifier. |
| `ebwv:taxIdentifier` | Add | Tax identifier. |
| `ebwv:leiIdentifier` | Add | LEI identifier. |
| `ebwv:euidIdentifier` | Add | EUID identifier. |
| `ebwv:legalName` | Existing | Legal entity name. |
| `ebwv:officialName` | Add | Official name of wallet owner. |
| `ebwv:tradingName` | Add | Trade/business name. |
| `ebwv:registeredAddress` | Existing | Registered address. |
| `ebwv:correspondenceAddress` | Existing | Correspondence address. |
| `ebwv:siteAddress` | Add | Address of site. |
| `ebwv:hasSite` | Existing, broaden | Wallet owner to site. |
| `ebwv:hasUnit` | Add | Organisation to organisational unit. |
| `ebwv:branchOf` | Add | Branch to parent economic operator. |
| `ebwv:establishmentOf` | Add | Establishment to parent economic operator. |
| `ebwv:hasEvidence` | Add | Attestation to evidence. |
| `ebwv:evidenceType` | Add | Evidence type. |
| `ebwv:digestMultibase` | Add | Evidence digest. |
| `ebwv:evidenceLocation` | Add | Evidence URI/location. |
| `ebwv:validFrom` | Add | Validity start. |
| `ebwv:validUntil` | Add | Validity end. |
| `ebwv:coveredPeriod` | Add | Period covered by attested facts. |
| `ebwv:startDate` | Existing | Start of PeriodOfTime. |
| `ebwv:endDate` | Existing | End of PeriodOfTime. |
| `ebwv:principal` | Add | Authorisation grantor. |
| `ebwv:authorisedActor` | Add | Authorised actor. |
| `ebwv:scopeOfAuthorization` | Existing | Scope of authority. |
| `ebwv:restriction` | Add | Restriction text/concept. |
| `ebwv:amount` | Existing, change datatype | Monetary amount value. |
| `ebwv:currency` | Existing | Currency concept. |

---

## Appendix C: example W3C VC credentialSubject using EBWV semantics

```json
{
  "@context": [
    "https://www.w3.org/ns/credentials/v2",
    "https://w3id.org/ebwv/contexts/core/v1"
  ],
  "type": ["VerifiableCredential", "ElectronicAttestationOfAttributes", "CompanyCertificate"],
  "issuer": "did:example:finnish-business-register",
  "validFrom": "2026-06-12T00:00:00Z",
  "credentialSubject": {
    "id": "did:example:operator:1234567-8",
    "type": ["EconomicOperator", "LegalEntity"],
    "legalName": "Nordic Timber Oy",
    "economicOperatorType": "company",
    "legalIdentifier": {
      "type": "Identifier",
      "identifierValue": "1234567-8",
      "identifierScheme": "Finnish Business ID",
      "issuingAuthority": "Finnish Patent and Registration Office"
    },
    "businessIdentifier": [
      {
        "type": "Identifier",
        "identifierValue": "FI12345678",
        "identifierScheme": "VAT identifier"
      }
    ],
    "registeredAddress": {
      "type": "Address",
      "thoroughfare": "Tehtaankatu",
      "locatorDesignator": "12",
      "postCode": "00150",
      "postName": "Helsinki",
      "adminUnitL1": "FI"
    },
    "hasSite": [
      {
        "type": "Site",
        "siteType": "production site",
        "siteAddress": {
          "type": "Address",
          "thoroughfare": "Sahatie",
          "locatorDesignator": "4",
          "postCode": "57200",
          "postName": "Savonlinna",
          "adminUnitL1": "FI"
        }
      }
    ]
  }
}
```

The JSON key names are implementation-level conveniences. Their semantic meaning should be bound to EBWV IRIs through the JSON-LD context.

---

## Appendix D: references

- European Commission, **European Business Wallets**, Shaping Europe’s digital future, 19 November 2025.  
  https://digital-strategy.ec.europa.eu/en/policies/business-wallets

- European Commission proposal COM(2025) 838, **European Business Wallet owner** definition.  
  https://ipex.eu/IPEXL-WEB/download/file/082d29089a99d8c5019a9d3306950373

- SEMIC / Interoperable Europe, **Core Business Vocabulary**.  
  https://interoperable-europe.ec.europa.eu/collection/semic-support-centre/solution/core-vocabularies/core-business-vocabulary

- SEMIC, **Core Business Vocabulary release 2.00**, including the note that the vocabulary focuses on formally registered legal entities and excludes sole traders and other business-capable agents.  
  https://semiceu.github.io/Core-Business-Vocabulary/releases/2.00/

- SEMIC, **Core Business Vocabulary release 2.2.0**, including `legalIdentifier`.  
  https://semiceu.github.io/Core-Business-Vocabulary/releases/2.2.0/

- Interoperable Europe, **Core Vocabularies** overview, including Core Business, Core Location, Core Person and CPSV.  
  https://interoperable-europe.ec.europa.eu/collection/semic-support-centre/core-vocabularies

- W3C, **The Organization Ontology**.  
  https://www.w3.org/TR/vocab-org/

- W3C, **Verifiable Credentials Data Model 2.0**.  
  https://www.w3.org/TR/vc-data-model-2.0/

- W3C, **JSON-LD 1.1**.  
  https://www.w3.org/TR/json-ld11/

- W3C, **SHACL — Shapes Constraint Language**.  
  https://www.w3.org/TR/shacl/

---

## Closing note

The EBWV should be developed as a reusable semantic foundation for Business Wallet data products, not as a vocabulary for one attestation type or one technical credential format. The central modelling choice is to make `ebwv:EconomicOperator` the primary business-process subject while keeping it broader than `ebwv:LegalEntity` and broader than `org:Organization`. This allows the same vocabulary to describe companies, sole traders, self-employed professionals, temporary associations, public-sector wallet users, Union entities and other actors participating in European Business Wallet processes.

The recommended approach is:

```text
SKOS terminology defines business concepts and controlled values.
OWL defines reusable classes and properties.
SHACL defines profile-specific constraints.
JSON-LD contexts, JSON Schemas, SD-JWT schemas, XML schemas and APIs bind physical data to the same semantic vocabulary.
```

That architecture allows the EBWV to support W3C Verifiable Credentials while also remaining useful for every other technical implementation format where the same business facts need to be exchanged, validated and understood.
