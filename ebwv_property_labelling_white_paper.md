# White Paper: OWL Property Labelling in EBWV
## Executive summary
In OWL/RDF, a property is a predicate in a subject–predicate–object statement. For object properties, the preferred label should therefore name the relationship, not merely repeat the range class. A property such as `hasIdentifier` is usually clearer than `identifier` when the object is an `Identifier` node. Noun-like labels remain suitable for datatype properties, such as `familyName`, `identifierValue`, or `postCode`.
The uploaded EBWV Turtle vocabulary contains 93 properties. The review shows two linked issues: many object-like properties are labelled as bare nouns, and several properties declared as datatype properties have class or code-list ranges. The recommended clean-up is therefore not a cosmetic `has` prefix exercise only; it should be aligned with OWL property type, range semantics, and SHACL usage.
## Recommendation
Use the following convention:

1. Use `hasX` for object properties where the subject carries, owns, is described by, or is associated with an object node or controlled concept.
2. Use more precise verbs where the relation has a clear business meaning, e.g. `identifiesLegalEntity`, `isIssuedBy`, `represents`, `isRepresentedBy`.
3. Keep noun-like names for datatype properties whose values are literals, e.g. `familyName`, `legalName`, `postCode`, `startDate`.
4. Use inverse `isXOf` labels only for inverse properties, not as the primary direction unless that direction is semantically natural.
## Core example
For `Person → Identifier`, the better object property is:

```ttl
ebwv:hasIdentifier
    a owl:ObjectProperty ;
    rdfs:label "has identifier"@en ;
    rdfs:domain ebwv:Person ;
    rdfs:range ebwv:Identifier .
```

The corresponding literal value should be placed inside the identifier object:

```ttl
ebwv:identifierValue
    a owl:DatatypeProperty ;
    rdfs:label "identifier value"@en ;
    rdfs:domain ebwv:Identifier ;
    rdfs:range xsd:string .
```
## Why this matters
The naming convention improves graph readability, SHACL validation, and VC/JSON-LD interpretation. In a verifiable credential context, the distinction is especially important: `hasIdentifier` points to an identifier object that may contain value, scheme, issuer, jurisdiction, validity, and status; `identifierValue` points only to a literal string.
## Annex: Proposed EBWV property changes
| Current property | Current label | Current range | Proposed property label/local name | Comment |
|---|---|---|---|---|
| `ebwv:a1` | a1 certificate | `xsd:anyUri` | `keep or rename to hasA1CertificateReference` | Datatype property: if value is URI reference to the A1 certificate, label should be "A1 certificate reference". If modelled as a certificate object, use object property hasA1Certificate. |
| `ebwv:accountType` | account type | `ebwv:AccountType` | `hasAccountType` | Range is ebwv:AccountType; model as object property if AccountType is a code-list concept/class. |
| `ebwv:activity` | main activity of company | `ebwv:Nace21` | `hasMainEconomicActivity` | Range is ebwv:Nace21; use object property to NACE concept. |
| `ebwv:adminUnitL1` | administrative unit level 1 | `ebwv:CountryCode` | `hasAdministrativeUnitLevel1` | Range is ebwv:CountryCode in current file; verify whether this should be Country/AdministrativeUnit rather than CountryCode. |
| `ebwv:administrativeRepresentative` | administrative representative | `ebwv:Person` | `hasAdministrativeRepresentative` | Object relation to Person. |
| `ebwv:applicableJurisdiction` | applicable jurisdiction | `ebwv:CountryCode` | `hasApplicableJurisdiction` | Range is ebwv:CountryCode; object-like coded jurisdiction. |
| `ebwv:assignment` | assignment | `ebwv:WorkAssignment` | `hasAssignment` | Object relation to WorkAssignment. |
| `ebwv:attestationLegalCategory` | attestation legal category | `ebwv:AttestationLegalCategory \| _:bnode` | `hasAttestationLegalCategory` | Object relation to category concept/class. |
| `ebwv:citizenship` | citizenship | `ebwv:CountryCode` | `hasCitizenship` | Range is CountryCode; object-like relation to citizenship country/code. |
| `ebwv:contactPoint` | contact point | `ebwv:ContactPoint` | `hasContactPoint` | Object relation to ContactPoint. |
| `ebwv:correspondenceAddress` | correspondence Address | `ebwv:Address` | `hasCorrespondenceAddress` | Object relation to Address. |
| `ebwv:countryOfBirth` | country of birth | `ebwv:Location` | `hasCountryOfBirth` | Object relation to Location; consider Country/CountryCode depending on intended granularity. |
| `ebwv:countryOfDeath` | country of death | `ebwv:Location` | `hasCountryOfDeath` | Object relation to Location; consider Country/CountryCode depending on intended granularity. |
| `ebwv:currency` | currency | `ebwv:Currency` | `hasCurrency` | Range is Currency; model as object property to currency code/concept. |
| `ebwv:deliveryLocation` | delivery location | `ebwv:Location \| ebwv:Address` | `hasDeliveryLocation` | Object relation to Location/Address. |
| `ebwv:domicile` | domicile | `ebwv:Address` | `hasDomicile` | Object relation to Address; "domicile" is acceptable but less predicate-like. |
| `ebwv:duration` | duration | `ebwv:PeriodOfTime` | `hasDuration` | Object relation to PeriodOfTime. |
| `ebwv:employer` | employer | `ebwv:EconomicOperator` | `hasEmployer` | For PWN this is acceptable; inverse could be isEmployerInNotification or employs. |
| `ebwv:hasEmail` | email | `xsd:string` | `emailAddress` | Datatype property should usually be noun-like; "has" is less useful with string range. |
| `ebwv:hasTelephone` | telephone | `xsd:string` | `telephoneNumber` | Datatype property should usually be noun-like; "has" is less useful with string range. |
| `ebwv:hostEntity` | host entity | `ebwv:EconomicOperator` | `hasHostEntity` | Object relation to EconomicOperator. |
| `ebwv:iban` | IBAN | `ebwv:Iban` | `hasIBAN` | Current range is ebwv:Iban; model as object property if IBAN is structured. Use ibanValue for literal string. |
| `ebwv:identifier` | identifier | `ebwv:Bic \| ebwv:ClearingNumber \| ebwv:Eori \| ebwv:Excise \| ebwv:Iban \| ebwv:Lei \| ebwv:Tin \| ebwv:VatId \| ebwv:PeppolId` | `hasIdentifier` | Range is identifier classes; object property should name the relation. Use identifierValue inside the Identifier object. |
| `ebwv:jurisdiction` | jurisdiction | `ebwv:CountryCode` | `hasJurisdiction` | Range is CountryCode; object-like jurisdiction relation. |
| `ebwv:legalEntity` | legal entity as registered at lei issuer | `ebwv:LegalEntity` | `identifiesLegalEntity` | More precise than hasLegalEntity for LEI context; relates LegalEntityIdentifier to LegalEntity. |
| `ebwv:legalIdentifier` | legal identifier | `ebwv:PublicBodyId \| ebwv:Euid` | `hasLegalIdentifier` | Range is PublicBodyId/EUID; object property if these are identifier classes. |
| `ebwv:legalRepresentative` | legal representative | `ebwv:LegalRepresentative` | `hasLegalRepresentative` | Object relation to LegalRepresentative. |
| `ebwv:leiLastUpdate` | lei last update date | `xsd:string` | `leiLastUpdateDate` | Minor consistency: label says date; range currently xsd:string, consider xsd:dateTime or xsd:date. |
| `ebwv:leiNextRenewal` | lei next renewal | `xsd:string` | `leiNextRenewalDate` | Minor consistency: label says renewal date; range currently xsd:string, consider xsd:date. |
| `ebwv:leiRegistration` | lei registration data | `ebwv:LeiRegistration` | `hasLEIRegistration` | Object relation to LeiRegistration. |
| `ebwv:leiRegistrationDate` | lei registration date | `xsd:string` | `leiRegistrationDate` | Keep name; consider xsd:date instead of xsd:string. |
| `ebwv:leiRegistrationStatus` | lei registration status | `xsd:string` | `leiRegistrationStatus` | Keep if literal/code string; if status concept, use hasLEIRegistrationStatus. |
| `ebwv:liabilityOrContribution` | liability or contribution of the limited partner | `ebwv:Capital` | `hasLiabilityOrContribution` | Object relation to Capital. |
| `ebwv:locatorDesignator` | locator designator | `xsd:string \| xsd:integer` | `locatorDesignator` | Keep as datatype; range includes string/integer, but xsd:string may be safer for leading zeros and alphanumeric values. |
| `ebwv:managingLeiIssuer` | managing lei issuer | `xsd:string` | `managingLEIIssuerIdentifier or hasManagingLEIIssuer` | If literal LEI issuer code, keep as datatype; if issuer entity, use object property. |
| `ebwv:owner` | owner | `ebwv:LegalPerson \| ebwv:EconomicOperator \| ebwv:NaturalPerson \| ebwv:Person` | `hasOwner` | Object relation from BankAccount to owner party. |
| `ebwv:partner` | partner | `ebwv:GeneralPartner \| ebwv:LimitedPartner \| ebwv:StatutoryPartner` | `hasPartner` | Object relation to partner role. |
| `ebwv:placeOfBirth` | place of birth | `ebwv:Location` | `hasPlaceOfBirth` | Object relation to Location. |
| `ebwv:placeOfDeath` | place of death | `ebwv:Location` | `hasPlaceOfDeath` | Object relation to Location. |
| `ebwv:placeOfWork` | place of work | `ebwv:CountryCode \| ebwv:EconomicOperator \| ebwv:Vessel` | `hasPlaceOfWork` | Object-like relation; current property lacks explicit OWL type. |
| `ebwv:postedWorker` | posted worker | `ebwv:Person` | `hasPostedWorker` | Object relation from notification to Person; consider certificateSubject/notificationSubject if used generally. |
| `ebwv:provider` | provide | `ebwv:LegalPerson \| ebwv:EconomicOperator` | `hasProvider` | Current label "provide" should be corrected; relates BankAccount to provider. |
| `ebwv:registeredAddress` | registered address | `ebwv:Address` | `hasRegisteredAddress` | Object relation to Address. |
| `ebwv:residency` | residency | `ebwv:CountryCode` | `hasResidency` | Range is CountryCode; object-like relation. If literal country code, use residencyCountryCode. |
| `ebwv:scopeOfAuthorization` | scope of authorization (alone or jointly) | `ebwv:ScopeOfAuthorization \| _:bnode` | `hasScopeOfAuthorization` | Object relation to ScopeOfAuthorization. |
| `ebwv:socialRepresentative` | social representative | `ebwv:Person` | `hasSocialRepresentative` | Object relation to Person. |
| `ebwv:stateOfInsurance` | state of insurance | `ebwv:CountryCode` | `hasStateOfInsurance` | Range is CountryCode; object-like relation. |
| `ebwv:subscribedCapital` | subscribed capital | `ebwv:Capital` | `hasSubscribedCapital` | Object relation to Capital. |
| `ebwv:supplier` | supplier | `xsd:anyURI` | `supplierIdentifier or hasSupplier` | Current datatype URI; if supplier is an entity, model as object property hasSupplier. |
| `ebwv:temporaryAddress` | temporary address | `ebwv:Address` | `hasTemporaryAddress` | Object relation to Address. |
| `ebwv:terms` | terms | `ebwv:Incoterms \| _:bnode` | `hasTerms` | Object relation to Incoterms/terms concept; consider hasIncoterms for narrower meaning. |
| `ebwv:typeOfEmployment` | type of employment | `ebwv:TypeOfEmployment \| _:bnode` | `hasTypeOfEmployment` | Object relation to TypeOfEmployment concept/class. |
