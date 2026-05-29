# Social Security Contribution Attestation in EBWV

**Plain French URSSAF version and extended cross-border SSC version**  
**Audience:** non-experts, business analysts, semantic modellers, and implementers  
**Vocabulary basis:** latest uploaded EBWV YAML file, `vocabulary (5).yml`  
**Credential model:** W3C Verifiable Credentials with EBWV terms inside `credentialSubject`

---

## 1. Executive summary

A **Social Security Contribution Attestation** is a digital proof that an employer has fulfilled, or is assessed as having fulfilled, certain employment-related obligations. These may include social security declarations, social contribution payments, unemployment insurance contributions, family allowance contributions, or similar Member State obligations.

The French URSSAF certificate is the starting point for this model. It is a national certificate used to show that an employer is up to date with relevant declarations and contribution obligations. In the EU Business Wallet / EBWV context, the same idea can be expressed as a structured, verifiable credential rather than as a PDF or paper document.

The model has two levels:

1. **The Verifiable Credential envelope**, using the W3C VC vocabulary. This contains general credential metadata such as issuer, validity dates, credential identifier, and credential status.
2. **The EBWV credential subject**, using EBWV terms. This contains the actual business and legal facts being attested: liable employer, identifiers, reporting period, workforce, payroll, covered obligations, compliance status, and related scope information.

The key principle is simple:

> Use W3C VC terms for the credential as a digital credential. Use EBWV terms for the substantive business facts inside the credential.

---

## 2. Vocabulary and modelling conventions used in this document

The examples use the following broad conventions:

| Prefix / term family | Meaning |
|---|---|
| `cred:` / W3C VC vocabulary | General Verifiable Credential metadata, such as issuer and validity. |
| `ebwv:` / EBWV | Business vocabulary terms used inside the credential subject. |
| `SocialSecurityContributionAttestation` | EBWV class representing the SSC attestation subject. |
| `EconomicOperator` | EBWV class representing the employer or business operator. |
| `Site` | EBWV class representing a site, establishment, branch, workplace, or comparable operational location. |
| `MonetaryAmount` | EBWV class representing an amount of money with value and currency. |
| `PeriodOfTime` | EBWV class representing a time period with start and end dates. |

The latest EBWV YAML already includes the following SSC-relevant terms:

| EBWV term | Type | Current role |
|---|---|---|
| `SocialSecurityContributionAttestation` | Class | Root class for the SSC credential subject. |
| `liableEmployer` | Property | Links the attestation to the employer whose obligations are certified. |
| `hasSite` | Property | Links the attestation to covered sites or establishments. |
| `reportingPeriod` | Property | Indicates the period to which the reported data relates. |
| `validAsOfDate` | Property | Indicates the date on which the facts or status were assessed. |
| `complianceStatus` | Property | Indicates the assessed compliance status. |
| `coveredObligationType` | Property | Indicates what type of obligations are covered. |
| `averagePeriodWorkforce` | Property | Indicates average workforce for the reporting period. |
| `totalPayroll` | Property | Indicates total payroll or remuneration for the reporting period. |
| `legalBasis` | Property | Indicates the legal basis or legal reference for the attestation. |
| `MonetaryAmount`, `amount`, `currency` | Class/properties | Used for total payroll. |
| `PeriodOfTime`, `startDate`, `endDate` | Class/properties | Used for reporting period. |
| `Site`, `identifier` | Class/property | Used for covered sites and site identifiers. |

Some additional terms have been recommended during the modelling discussion, especially `coverageStatement`, `qualificationNote`, `Siren`, `Siret`, `NationalBusinessRegisterId`, and `NationalEstablishmentId`. Where these are used below, they are marked as recommended extensions if not yet present in the YAML.

---

# Part A — Plain French URSSAF version

## 3. Purpose of the plain French URSSAF version

The plain French version is a compact digital representation of the core content of a French URSSAF-style attestation. It is intended to answer the practical relying-party question:

> Is this employer up to date with the relevant French social security contribution and reporting obligations for the relevant period or date?

This version deliberately keeps the data model simple and close to the French source use case.

---

## 4. Plain French URSSAF attributes

| Attribute | EBWV / VC property | English description | Description française | Example value |
|---|---|---|---|---|
| Credential issuer | `issuer` | The authority or body that issued the credential. | L’autorité ou l’organisme qui a délivré l’attestation. | `did:web:urssaf.example.fr` |
| Credential validity start | `validFrom` | The date and time from which the credential is valid as a digital credential. | La date et l’heure à partir desquelles l’attestation numérique est valable. | `2025-08-01T00:00:00Z` |
| Credential validity end | `validUntil` | The date and time until which the credential may be relied upon. | La date et l’heure jusqu’à laquelle l’attestation peut être utilisée. | `2026-02-01T00:00:00Z` |
| Attestation subject type | `credentialSubject.type` = `SocialSecurityContributionAttestation` | States that the credential subject is an SSC attestation. | Indique que le sujet de l’attestation est une attestation de contributions sociales. | `SocialSecurityContributionAttestation` |
| Liable employer | `liableEmployer` | The employer whose social security obligations are being certified. | L’employeur dont les obligations sociales sont attestées. | `SA ORANGE BUSINESS SERVICES` |
| Legal name | `legalName` | The official legal name of the employer. | La dénomination sociale officielle de l’employeur. | `SA ORANGE BUSINESS SERVICES` |
| Employer identifier | `legalIdentifier` | The legal identifier of the employer. In France, this is typically a SIREN. | L’identifiant légal de l’employeur. En France, il s’agit généralement du SIREN. | `345039416` |
| Covered site or establishment | `hasSite` | A site, establishment, local unit, branch, workplace, or comparable operational location covered by the attestation. | Un site, établissement, unité locale, succursale, lieu de travail ou lieu opérationnel comparable couvert par l’attestation. | Saint-Denis establishment |
| Site identifier | `identifier` on `Site` | The identifier of the site or establishment. In France, this may be a SIRET. | L’identifiant du site ou de l’établissement. En France, il peut s’agir du SIRET. | `34503941600012` |
| Legal basis | `legalBasis` | The legal provision or framework under which the attestation is issued. | La base légale ou réglementaire en vertu de laquelle l’attestation est délivrée. | `Article L243-15 du Code de la sécurité sociale` |
| Covered obligation type | `coveredObligationType` | The type of social security, employment-related contribution, reporting, or payment obligation covered. | Le type d’obligation sociale, déclarative ou de paiement couverte par l’attestation. | `socialSecurityContribution` |
| Compliance status | `complianceStatus` | The assessed status of the employer’s fulfilment of the covered obligations. | Le statut évalué du respect par l’employeur des obligations couvertes. | `upToDate` |
| Valid-as-of date | `validAsOfDate` | The date on which the facts or status were assessed. | La date à laquelle les faits ou le statut ont été évalués. | `2025-07-31` |
| Reporting period | `reportingPeriod` | The period to which the declarations, payroll, workforce, or other reported data relate. | La période à laquelle se rapportent les déclarations, la masse salariale, l’effectif ou les autres données déclarées. | July 2025 |
| Average period workforce | `averagePeriodWorkforce` | The average number of employed persons for the reporting period. | L’effectif moyen de personnes employées pour la période déclarée. | `2365` |
| Total payroll | `totalPayroll` | The total gross payroll, wage sum, or declared remuneration for the reporting period. | La masse salariale totale, le montant total des salaires ou la rémunération déclarée pour la période concernée. | `129350000.00 EUR` |
| Amount | `amount` | The numeric value of the monetary amount. | La valeur numérique du montant monétaire. | `129350000.00` |
| Currency | `currency` | The currency in which the monetary amount is expressed. | La devise dans laquelle le montant monétaire est exprimé. | `EUR` |

---

## 5. How to represent the French “month and year reported”

The French source concept “month and year reported” should not be modelled as a single `xsd:date`, because a month is a period, not one specific day.

In EBWV, the better property is:

```yaml
reportingPeriod -> PeriodOfTime
```

Example for July 2025:

```json
"reportingPeriod": {
  "type": "PeriodOfTime",
  "startDate": "2025-07-01",
  "endDate": "2025-07-31"
}
```

This means that payroll, workforce, declarations, and contribution information relate to the whole month of July 2025.

---

## 6. Plain French URSSAF-style VC example

```json
{
  "@context": [
    "https://www.w3.org/ns/credentials/v2",
    "https://w3id.org/ebwv/v0.1"
  ],
  "id": "urn:uuid:5b01992c-2c5c-4e62-8590-plain-french-urssaf",
  "type": [
    "VerifiableCredential",
    "ElectronicAttestationOfAttributes"
  ],
  "attestationLegalCategory": "Pub-EAA",
  "issuer": "did:web:urssaf.example.fr",
  "validFrom": "2025-08-01T00:00:00Z",
  "validUntil": "2026-02-01T00:00:00Z",
  "credentialSubject": {
    "id": "urn:uuid:subject-plain-urssaf-example",
    "type": "SocialSecurityContributionAttestation",
    "liableEmployer": {
      "type": "EconomicOperator",
      "legalName": "SA ORANGE BUSINESS SERVICES",
      "legalIdentifier": "345039416",
      "registeredAddress": {
        "type": "Address",
        "fullAddress": "1 Place des Droits de l'Homme, 93200 Saint-Denis, France"
      }
    },
    "hasSite": [
      {
        "type": "Site",
        "identifier": "34503941600012",
        "registeredAddress": {
          "type": "Address",
          "fullAddress": "1 Place des Droits de l'Homme, 93200 Saint-Denis, France"
        }
      }
    ],
    "legalBasis": "Article L243-15 of the French Social Security Code",
    "coveredObligationType": [
      "socialSecurityContribution",
      "employmentDeclarationObligation"
    ],
    "complianceStatus": "upToDate",
    "validAsOfDate": "2025-07-31",
    "reportingPeriod": {
      "type": "PeriodOfTime",
      "startDate": "2025-07-01",
      "endDate": "2025-07-31"
    },
    "averagePeriodWorkforce": 2365,
    "totalPayroll": {
      "type": "MonetaryAmount",
      "amount": "129350000.00",
      "currency": "EUR"
    }
  }
}
```

### Non-expert explanation

This credential says:

- URSSAF, or an equivalent issuing authority, issued a digital attestation.
- The employer is `SA ORANGE BUSINESS SERVICES`.
- The relevant employer identifier is `345039416`.
- The attestation covers a reported period, here July 2025.
- The employer is assessed as `upToDate` as of `2025-07-31`.
- The attestation includes an average workforce figure and a total payroll amount for the reported period.
- The digital credential itself has its own validity dates in the VC envelope.

---

# Part B — More extensive generic SSC version for cross-border use

## 7. Why a more extensive version is needed

The French URSSAF version is useful, but a reusable European model must also work for comparable social contribution or employment-related compliance attestations in other Member States.

Other Member States may differ in:

- the legal basis for the attestation;
- the authority responsible for issuing it;
- the employer identifier used;
- whether sites or establishments are individually listed;
- whether the reporting period is monthly, quarterly, annual, or another period;
- which obligations are covered;
- how compliance is assessed;
- whether there are caveats, payment plans, disputes, or other qualifications.

The extensive SSC version therefore keeps the French core but makes the model more flexible.

---

## 8. Extended SSC attributes currently supported by EBWV YAML

The following table focuses on attributes that are currently supported by the latest EBWV YAML.

| Attribute | EBWV property/class | Range / value type | Plain-language explanation | Example |
|---|---|---|---|---|
| SSC attestation root | `SocialSecurityContributionAttestation` | Class | The object that contains the attested SSC facts. | `type: SocialSecurityContributionAttestation` |
| Liable employer | `liableEmployer` | `EconomicOperator` | The employer whose social contribution, reporting, or payment obligations are the subject of the attestation. | Employer object |
| Employer name | `legalName` | string | The legal name of the employer. | `Example Company SA` |
| Employer legal identifier | `legalIdentifier` | identifier datatype | The official identifier of the employer. | EUID, SIREN, VAT ID, national register ID |
| Employer address | `registeredAddress` | `Address` | The registered or official address of the employer. | Address object |
| Covered site | `hasSite` | `Site` | A site, establishment, branch, workplace, or local unit covered by the attestation. | Site object |
| Site identifier | `identifier` | identifier datatype | The identifier of a site or establishment. | SIRET, national establishment ID |
| Legal basis | `legalBasis` | `xsd:string` | The legal provision or framework for the attestation or covered obligation. | National legal reference |
| Covered obligation type | `coveredObligationType` | `xsd:string` | The category of obligation covered. | `socialSecurityContribution` |
| Compliance status | `complianceStatus` | `xsd:string` | The assessed status of the employer regarding the covered obligations. | `upToDate` |
| Valid-as-of date | `validAsOfDate` | `xsd:date` | The date on which the status or facts were assessed. | `2025-07-31` |
| Reporting period | `reportingPeriod` | `PeriodOfTime` | The period to which declarations, payroll, workforce, or other reported data relate. | July 2025, Q2 2025 |
| Start date | `startDate` | `xsd:date` | Start of a `PeriodOfTime`. | `2025-07-01` |
| End date | `endDate` | `xsd:date` | End of a `PeriodOfTime`. | `2025-07-31` |
| Average workforce | `averagePeriodWorkforce` | `xsd:decimal` | Average number of employed persons for the reporting period. | `2365` |
| Total payroll | `totalPayroll` | `MonetaryAmount` | Total payroll, wage sum, or declared remuneration for the reporting period. | Monetary amount object |
| Amount | `amount` | `xsd:decimal` | Numeric amount value. | `129350000.00` |
| Currency | `currency` | `Currency` | Currency code for the amount. | `EUR` |

---

## 9. Recommended but still optional extensions

The following attributes are strongly useful for cross-border SSC attestations, but some may still need to be added to the EBWV YAML depending on the final vocabulary update.

| Recommended attribute | Proposed EBWV property or datatype | Why it is useful |
|---|---|---|
| Coverage statement | `coverageStatement` | Captures scope text such as “valid for all establishments declared with the issuing authority”. |
| Qualification note | `qualificationNote` | Captures caveats, limitations, legal notes, or interpretive conditions. |
| French legal unit identifier | `Siren` | Needed for French employer identification. |
| French establishment identifier | `Siret` | Needed for French site / establishment identification. |
| Generic national business identifier | `NationalBusinessRegisterId` | Needed for Member States that use national register identifiers instead of EUID. |
| Generic national establishment identifier | `NationalEstablishmentId` | Needed for Member State-specific local unit or workplace identifiers. |

Recommended comments:

```yaml
- id: coverageStatement
  label: coverage statement
  comment: Provides a human-readable statement describing the establishments, sites, obligations, regimes, or reporting scope covered by the attestation.
  domain: SocialSecurityContributionAttestation
  range: xsd:string
```

```yaml
- id: qualificationNote
  label: qualification note
  comment: Provides a caveat, limitation, condition, or explanatory note that qualifies the interpretation of the attestation.
  domain: SocialSecurityContributionAttestation
  range: xsd:string
```

---

## 10. Extended cross-border SSC VC example

```json
{
  "@context": [
    "https://www.w3.org/ns/credentials/v2",
    "https://w3id.org/ebwv/v0.1"
  ],
  "id": "urn:uuid:4f1b2d0d-9a52-4c3d-9a6f-generic-ssc-example",
  "type": [
    "VerifiableCredential",
    "ElectronicAttestationOfAttributes"
  ],
  "attestationLegalCategory": "Pub-EAA",
  "issuer": "did:web:social-security-authority.example.eu",
  "validFrom": "2025-08-01T00:00:00Z",
  "validUntil": "2026-02-01T00:00:00Z",
  "credentialStatus": {
    "id": "https://social-security-authority.example.eu/status/credentials/4f1b2d0d",
    "type": "StatusList2021Entry"
  },
  "credentialSubject": {
    "id": "urn:uuid:subject-generic-ssc-example",
    "type": "SocialSecurityContributionAttestation",
    "liableEmployer": {
      "id": "did:web:example-company.eu",
      "type": "EconomicOperator",
      "legalName": "Example Company SA",
      "legalIdentifier": [
        "FRPAR.345039416",
        "345039416",
        "FR12345039416"
      ],
      "registeredAddress": {
        "type": "Address",
        "fullAddress": "1 Place des Droits de l'Homme, 93200 Saint-Denis, France",
        "postCode": "93200",
        "postName": "Saint-Denis",
        "adminUnitL1": "FR"
      }
    },
    "hasSite": [
      {
        "id": "urn:uuid:site-1",
        "type": "Site",
        "identifier": "34503941600012",
        "registeredAddress": {
          "type": "Address",
          "fullAddress": "1 Place des Droits de l'Homme, 93200 Saint-Denis, France",
          "postCode": "93200",
          "postName": "Saint-Denis",
          "adminUnitL1": "FR"
        }
      }
    ],
    "legalBasis": "Article L243-15 of the French Social Security Code, or equivalent national legal basis in the issuing Member State",
    "coveredObligationType": [
      "socialSecurityContribution",
      "familyAllowanceContribution",
      "unemploymentInsuranceContribution",
      "wageGuaranteeFundContribution",
      "employmentDeclarationObligation"
    ],
    "complianceStatus": "upToDate",
    "validAsOfDate": "2025-07-31",
    "reportingPeriod": {
      "type": "PeriodOfTime",
      "startDate": "2025-07-01",
      "endDate": "2025-07-31"
    },
    "averagePeriodWorkforce": 2365,
    "totalPayroll": {
      "type": "MonetaryAmount",
      "amount": "129350000.00",
      "currency": "EUR"
    },
    "coverageStatement": "This attestation covers the liable employer and the establishments declared to the issuing authority for the reporting period. Where obligations are handled centrally, the attestation applies within the scope of that centralised reporting arrangement.",
    "qualificationNote": "The attestation confirms the status recorded by the issuing authority for the covered obligations at the valid-as-of date. It does not replace any separate attestation required for obligations outside the stated scope or outside the issuing jurisdiction."
  }
}
```

---

## 11. How the extended version supports cross-border use

| Cross-border issue | How the extended model helps |
|---|---|
| Different national identifiers | `legalIdentifier` and `identifier` can support EUID, VAT ID, SIREN, SIRET, national business identifiers, or national establishment identifiers. |
| Different contribution regimes | `coveredObligationType` can list the relevant national obligation categories. |
| Different reporting periods | `reportingPeriod` supports months, quarters, years, or nationally defined periods. |
| Multi-site employers | `hasSite` can list covered sites, while `coverageStatement` can explain broader coverage. |
| National legal differences | `legalBasis` can state the national legal reference. |
| Status interpretation | `complianceStatus` gives the assessed status, while `qualificationNote` can explain limitations. |
| Separation from VC metadata | Issuer, credential validity, and credential status remain in W3C VC fields, not in EBWV business properties. |

---

## 12. Separation between VC metadata and EBWV subject data

The following table explains which information belongs where.

| Information need | Use W3C VC property | Use EBWV property | Explanation |
|---|---|---|---|
| Who issued the digital credential? | `issuer` | — | This is metadata about the credential. |
| When does the credential become valid? | `validFrom` | — | This is VC validity metadata. |
| When does the credential expire? | `validUntil` | — | This is VC validity metadata. |
| How can the credential status be checked? | `credentialStatus` | — | This is VC status infrastructure. |
| Who is the employer whose obligations are certified? | — | `liableEmployer` | This is the business subject of the attestation. |
| Which obligations are covered? | — | `coveredObligationType` | This is the substantive scope of the attestation. |
| Is the employer up to date? | — | `complianceStatus` | This is the attested legal/administrative conclusion. |
| What date was the status assessed on? | — | `validAsOfDate` | This is not the same as VC validity. |
| Which month, quarter, or year is reported? | — | `reportingPeriod` | This is the period for the underlying declarations/payroll. |
| What payroll amount is declared? | — | `totalPayroll` | This is an attested business fact. |

---

## 13. Recommended value examples for string-based properties

The current YAML uses `xsd:string` for `complianceStatus` and `coveredObligationType`. This is acceptable as a transitional solution before code lists or SKOS concept schemes are introduced.

### Possible `complianceStatus` values

| Value | Meaning |
|---|---|
| `upToDate` | The employer is assessed as up to date with the covered obligations. |
| `notUpToDate` | The employer is not assessed as up to date. |
| `compliantUnderPaymentPlan` | The employer is treated as compliant because an accepted payment plan exists. |
| `disputedAmount` | The status is affected by a disputed contribution amount. |
| `notApplicable` | The obligation is not applicable to the employer or period. |
| `unknown` | The status cannot be determined from the attestation data. |

### Possible `coveredObligationType` values

| Value | Meaning |
|---|---|
| `socialSecurityContribution` | Social security contribution obligations. |
| `familyAllowanceContribution` | Family allowance-related contributions. |
| `unemploymentInsuranceContribution` | Unemployment insurance contributions. |
| `wageGuaranteeFundContribution` | Wage guarantee fund contributions. |
| `employmentDeclarationObligation` | Obligations to submit employment-related declarations. |
| `agriculturalSocialContribution` | Agricultural-sector social contribution obligations. |
| `selfEmployedSocialContribution` | Self-employed worker contribution obligations. |
| `otherEmploymentRelatedContribution` | Other employment-related contribution obligations. |

In a future semantic foundation, these string values should ideally become governed SKOS concepts.

---

## 14. Recommended EBWV YAML refinements still relevant

The latest YAML already contains most of the necessary SSC structure. The remaining refinements are:

```yaml
# Improve comment
- id: SocialSecurityContributionAttestation
  comment: A social security contribution attestation is an attestation issued by a competent authority or authorised body confirming information about an employer’s fulfilment of obligations related to social security contributions, employment-related declarations, or contribution payments arising from the employment of workers.

# Improve comment
- id: liableEmployer
  comment: Indicates the economic operator whose social security contribution, employment-related reporting, or payment obligations are the subject of the attestation.

# Improve comment
- id: totalPayroll
  comment: Indicates the total gross payroll, wage sum, or declared remuneration amount associated with the liable employer for the reporting period covered by the social security contribution attestation.

# Deprecate or remove
- id: reportingMonth
  deprecated: true
  replaced_by: reportingPeriod

# Add if not yet present
- id: coverageStatement
  label: coverage statement
  comment: Provides a human-readable statement describing the establishments, sites, obligations, regimes, or reporting scope covered by the attestation.
  domain: SocialSecurityContributionAttestation
  range: xsd:string

# Add if not yet present
- id: qualificationNote
  label: qualification note
  comment: Provides a caveat, limitation, condition, or explanatory note that qualifies the interpretation of the attestation.
  domain: SocialSecurityContributionAttestation
  range: xsd:string
```

---

## 15. Final recommended minimal profile

For the first implementable SSC attestation profile, the following attributes are recommended as the practical core:

| Attribute | Recommended status | Reason |
|---|---|---|
| `liableEmployer` | Mandatory | Identifies the employer whose obligations are certified. |
| `legalName` | Mandatory within employer | Needed for human-readable identification. |
| `legalIdentifier` | Mandatory within employer | Needed for reliable machine identification. |
| `legalBasis` | Recommended | Important for national legal traceability. |
| `coveredObligationType` | Mandatory | Identifies what kind of obligations are covered. |
| `complianceStatus` | Mandatory | Expresses the central attested conclusion. |
| `validAsOfDate` | Mandatory | Indicates when the status was assessed. |
| `reportingPeriod` | Mandatory when figures are reported | Needed for workforce and payroll figures. |
| `averagePeriodWorkforce` | Optional / nationally required | Required in the French URSSAF-style profile. |
| `totalPayroll` | Optional / nationally required | Required in the French URSSAF-style profile. |
| `hasSite` | Optional / conditionally required | Needed if sites or establishments are listed or scoped. |
| `coverageStatement` | Recommended | Needed when scope is expressed as text. |
| `qualificationNote` | Optional but useful | Needed for caveats and limitations. |

---

## 16. Final non-expert takeaway

The SSC attestation should not be modelled as just a digital copy of a French paper certificate. It should be modelled as a structured, reusable European business attestation.

The plain French version captures the essential URSSAF-style information: employer, identifier, reporting month, average workforce, payroll, and up-to-date status.

The extended EBWV version adds the structure needed for cross-border and automated use: different national identifiers, different reporting periods, different obligation types, legal basis, site coverage, and explanatory notes.

This makes the attestation suitable for an EU Business Wallet because it can be verified automatically, interpreted consistently, and reused across administrative, procurement, contracting, and compliance processes.
