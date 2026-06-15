# Mapping: Finnish National Citizenship Certificate SHACL → EBWV

Source file: `OOTS Evidence, citizenship certificate.ttl`  
Source application profile version: `0.0.2`  
Generated semantic data specification: `https://w3id.org/ebwv/specifications/oots-citizenship-certificate/1.0`

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

