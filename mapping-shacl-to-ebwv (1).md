# Mapping: Finnish National Citizenship Certificate SHACL → EBWV

Source file: `OOTS Evidence, citizenship certificate.ttl`  
Source application profile version: `0.0.2`  
Generated semantic data specification: `https://w3id.org/ebwv/specifications/oots-citizenship-certificate/1.0`

## Compact mapping table

| Source attribute | Label | Card. / type | EBWV semantic reference | Fit | Comment |
|---|---|---:|---|---|---|
| `identifier` | Identifier | 0..1 / Literal | `ebwv:identifier` | Near | Literal certificate/evidence identifier. A specialised `evidenceIdentifier` or `attestationIdentifier` would be clearer if introduced. |
| `isAbout` | is about | 0..n / `person` | `ebwv:credentialSubject` | Near / proposed | Links the certificate to the person concerned. In a W3C VC this is naturally the `credentialSubject`; for SDG evidence, `isAbout` / `evidenceSubject` is also valid. |
| `givenName` | Given name | 0..n / string | `ebwv:givenName` | Exact | Direct person attribute mapping. |
| `familyName` | Family name | 0..1 / string | `ebwv:familyName` | Exact | Direct person attribute mapping. |
| `dateOfBirth` | Date of birth | 0..1 / date | `ebwv:dateOfBirth` | Exact | Direct mapping using `xsd:date` / JSON Schema `date`. |
| `personCitizenship` | citizenship | 0..n / `Citizenship` | `ebwv:citizenship` | Near | Object link from person to a Citizenship node. EBWV should support a person-to-citizenship relationship, preferably to a Citizenship object or jurisdiction/code value. |
| `citizensp` | Citizenship | 1..1 / string | `ebwv:citizenship` | Exact / near | Source uses `regperson:citizenship` as a string. For stronger interoperability, use a controlled country/jurisdiction code. |
| `dateOfCitizenshipAcquisition` | Date of citizenship acquisition | 0..1 / date | `ebwv:dateOfCitizenshipAcquisition` | Proposed | Recommended EBWV extension, because acquisition date is a stable and reusable citizenship evidence attribute. |
| `citizenship_` | Citizenship | 1..1 / string | `ebwv:citizenship` | Duplicate / legacy | Duplicate-looking property shape in the uploaded model. Prefer the `regperson:citizenship` path used by `citizensp` where possible. |

## Notes

- The table intentionally omits long source-path and JSON-schema-path columns to keep it readable in Markdown and GitHub previews.
- Full URI mappings are preserved in `citizenship-certificate-semantic-references.ttl`.
- The recommended common schema-level reference is:

```json
{
  "semanticDataSpecification": "https://w3id.org/ebwv/specifications/oots-citizenship-certificate/1.0"
}
```

- The recommended attribute-level reference pattern is:

```json
{
  "x-semanticReference": "https://w3id.org/ebwv#givenName"
}
```
