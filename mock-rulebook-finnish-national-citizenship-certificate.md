# Mock Attestation / Evidence Rulebook: Finnish National Citizenship Certificate

**Status:** Draft example / mock-up  
**Source application profile:** `OOTS Evidence, citizenship certificate`, version `0.0.2`  
**Source format:** SHACL Shapes in Turtle  
**Reusable semantic data specification:** `https://w3id.org/ebwv/specifications/oots-citizenship-certificate/1.0`  
**Reusable JSON Schema:** `https://w3id.org/ebwv/schemas/oots-citizenship-certificate/1.0/citizenship-certificate.schema.json`  
**Semantic vocabulary:** `https://w3id.org/ebwv/`

## 1. Purpose and scope

This mock rulebook shows how an SDG OOTS evidence definition for a Finnish national citizenship certificate can be documented in the same style as EUDI/EBW attestation rulebooks. The core assumption is that the **semantic content layer** should be reusable across initiatives:

- In SDG OOTS, the content is exchanged as evidence requested through OOTS procedures.
- In EUDI/EBW, the same content may be issued as an Electronic Attestation of Attributes or as a Verifiable Credential payload.
- The JSON, XML, SD-JWT, mDoc or W3C VC envelope may vary, but the semantic references should remain stable.

## 2. Attributes

### 2.1 Attribute overview

The uploaded SHACL application profile defines three main nodes:

1. `CitizenshipCertificate` — the evidence/certificate data product.
2. `person` — the natural person that the evidence is about.
3. `Citizenship` — a citizenship entry associated with the person.

The attribute-level mapping to EBWV is as follows.

| Source SHACL property | Label | Source path | Cardinality / type | JSON Schema path | EBWV semantic reference | Fit | Comment |
|---|---|---|---|---|---|---|---|
| `identifier` | Identifier | `https://iri.suomi.fi/model/oots-evi-population/0.0.6/identifier` | 0..1 / Literal | `evidenceIdentifier` | `https://w3id.org/ebwv#identifier` | near | The source property is a literal certificate/evidence identifier. EBWV identifier is usable, but a specialised evidenceIdentifier / attestationIdentifier would be clearer if available. |
| `isAbout` | is about | `https://iri.suomi.fi/model/oots-evi-population/0.0.6/isAbout` | 0..n / class: person | `credentialSubject` | `https://w3id.org/ebwv#credentialSubject` | near/proposed | The source property links the certificate to the person concerned. In a W3C VC this is naturally represented by credentialSubject; for SDG evidence a generic isAbout/evidenceSubject property is also appropriate. |
| `givenName` | Given name | `https://iri.suomi.fi/model/oots-evi-population/0.0.6/givenName` | 0..n / string | `credentialSubject.givenName` | `https://w3id.org/ebwv#givenName` | exact | Direct person attribute mapping. |
| `familyName` | Family name | `https://iri.suomi.fi/model/oots-evi-population/0.0.6/familyName` | 0..1 / string | `credentialSubject.familyName` | `https://w3id.org/ebwv#familyName` | exact | Direct person attribute mapping. |
| `dateOfBirth` | Date of birth | `https://iri.suomi.fi/model/oots-evi-population/0.0.6/dateOfBirth` | 0..1 / date | `credentialSubject.dateOfBirth` | `https://w3id.org/ebwv#dateOfBirth` | exact | Direct person attribute mapping with xsd:date / JSON Schema date. |
| `personCitizenship` | citizenship | `https://iri.suomi.fi/model/oots-evi-population/0.0.6/personCitizenship` | 0..n / class: Citizenship | `credentialSubject.citizenships` | `https://w3id.org/ebwv#citizenship` | near | The source has an object link to a Citizenship node. EBWV should support a person-to-citizenship relationship, preferably to a Citizenship object or jurisdiction/code value. |
| `citizensp` | Citizenship | `https://iri.suomi.fi/model/regperson/0.0.6/citizenship` | 1..1 / string | `credentialSubject.citizenships[].citizenship` | `https://w3id.org/ebwv#citizenship` | exact/near | The source uses regperson:citizenship as a string. For stronger semantics, use a controlled country/jurisdiction code. |
| `dateOfCitizenshipAcquisition` | Date of citizenship acquisition | `https://iri.suomi.fi/model/oots-evi-population/0.0.6/dateOfCitizenshipAcquisition` | 0..1 / date | `credentialSubject.citizenships[].dateOfCitizenshipAcquisition` | `https://w3id.org/ebwv#dateOfCitizenshipAcquisition` | proposed extension | Not assumed to exist in EBWV. Recommended addition because acquisition date is a stable citizenship evidence attribute. |
| `citizenship_` | Citizenship | `https://iri.suomi.fi/model/oots-evi-population/0.0.6/citizenship_` | 1..1 / string | `credentialSubject.citizenships[].citizenship` | `https://w3id.org/ebwv#citizenship` | duplicate/legacy | Duplicate-looking property shape in the uploaded model. Prefer the regperson:citizenship path used by citizensp where possible. |


### 2.2 Attribute descriptions

#### evidenceIdentifier

Identifier of the certificate/evidence instance. In the uploaded SHACL profile this is the `identifier` property of the `CitizenshipCertificate` node. In JSON Schema it is represented as `evidenceIdentifier` and linked to `https://w3id.org/ebwv#identifier`.

#### credentialSubject / isAbout

The uploaded SHACL profile uses an `isAbout` object property from `CitizenshipCertificate` to a person node. In a W3C VC/EUDI attestation profile, this maps naturally to the `credentialSubject` object. In a pure SDG evidence exchange, it can also be represented as an `evidenceSubject` or `isAbout` relation, but the same EBWV semantic relationship should be used.

#### givenName, familyName, dateOfBirth

These are direct person attributes. The proposed semantic references are:

- `givenName` → `https://w3id.org/ebwv#givenName`
- `familyName` → `https://w3id.org/ebwv#familyName`
- `dateOfBirth` → `https://w3id.org/ebwv#dateOfBirth`

#### citizenships

The uploaded SHACL profile models citizenship through a person-to-citizenship object relation and then a citizenship literal on the `Citizenship` node. In the JSON structure this is represented as an array of `citizenships`, because a person may hold more than one citizenship. The value should preferably be a controlled country or jurisdiction code, for example `FI`.

#### dateOfCitizenshipAcquisition

The uploaded SHACL profile includes an optional date of citizenship acquisition. This is a useful attribute for both SDG evidence and EUDI/EBW attestations. If EBWV does not yet contain this property, it should be added as a reusable person/citizenship-related property.

## 3. Semantic reference pattern

At schema level:

```json
{
  "semanticDataSpecification": "https://w3id.org/ebwv/specifications/oots-citizenship-certificate/1.0",
  "x-semanticVocabulary": "https://w3id.org/ebwv/"
}
```

At attribute level:

```json
{
  "givenName": {
    "type": "string",
    "x-semanticReference": "https://w3id.org/ebwv#givenName"
  }
}
```

The same pattern can be inserted in:

- SDG OOTS evidence definitions;
- EUDI attestation schemas;
- EBW attestation schemas;
- W3C VC JSON-LD contexts;
- SD-JWT JSON Schemas, using project-specific extension fields such as `x-semanticReference`.

## 4. JSON Schema

The reusable JSON Schema is provided as a separate file:

`citizenship-certificate.schema.json`

Core schema-level annotations:

```json
{
  "$id": "https://w3id.org/ebwv/schemas/oots-citizenship-certificate/1.0/citizenship-certificate.schema.json",
  "semanticDataSpecification": "https://w3id.org/ebwv/specifications/oots-citizenship-certificate/1.0",
  "x-semanticDataSpecification": "https://w3id.org/ebwv/specifications/oots-citizenship-certificate/1.0",
  "x-semanticVocabulary": "https://w3id.org/ebwv/",
  "x-sourceApplicationProfile": "https://iri.suomi.fi/model/oots-evi-citizenship-ap/0.0.2/"
}
```

## 5. SDG OOTS evidence publication example

```json
{
  "evidenceTypeIdentifier": "urn:oots:evidence-type:fi:national-citizenship-certificate:1.0",
  "name": {
    "en": "Finnish National Citizenship Certificate",
    "fi": "Kansalaisuustodistus"
  },
  "semanticDataSpecification": "https://w3id.org/ebwv/specifications/oots-citizenship-certificate/1.0",
  "schema": {
    "mediaType": "application/schema+json",
    "accessURL": "https://w3id.org/ebwv/schemas/oots-citizenship-certificate/1.0/citizenship-certificate.schema.json"
  }
}
```

## 6. EUDI/EBW attestation publication example

```json
{
  "attestationType": "FinnishNationalCitizenshipCertificate",
  "vct": "https://w3id.org/ebwv/vct/oots-citizenship-certificate/1.0",
  "semanticDataSpecification": "https://w3id.org/ebwv/specifications/oots-citizenship-certificate/1.0",
  "schema": {
    "mediaType": "application/schema+json",
    "accessURL": "https://w3id.org/ebwv/schemas/oots-citizenship-certificate/1.0/citizenship-certificate.schema.json"
  }
}
```

## 7. Implementation notes

1. `semanticDataSpecification` should identify the whole semantic data product.
2. `x-semanticReference` should identify the individual class or property in EBWV.
3. If the physical schema is SDG/OOTS XML, EUDI SD-JWT, mDoc, JSON Schema or W3C VC JSON-LD, the semantic references should remain the same.
4. The uploaded profile uses `citizensp` and `citizenship_` as separate property shapes with similar labels. The recommended implementation should rationalise this into one payload attribute, `citizenships[].citizenship`, while preserving source mappings in the Turtle mapping file.
5. The acquisition date should be maintained as a first-class semantic attribute because it is part of the source model and is relevant in evidence interpretation.

## 8. Files in this package

- `citizenship-certificate-semantic-references.ttl` — Turtle mapping file and semantic data specification declaration.
- `citizenship-certificate.schema.json` — reusable JSON Schema for SDG and EUDI/EBW use.
- `citizenship-certificate.context.jsonld` — minimal JSON-LD context for W3C VC use.
- `example-sdg-oots-evidence.json` — mock SDG OOTS publication and payload.
- `example-eudi-ebw-attestation-vc.json` — mock W3C VC/EUDI/EBW attestation payload.
