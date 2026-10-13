# Changelog

All notable changes to the FundsXML schema are documented in this file.
New schema nodes are also marked in the XSD with `<xs:appinfo source="@since X.Y.Z"/>`.

## [4.2.12] - Unreleased

### Added

Additional predefined classification systems for companies (`FundsXML4_Core.xsd`). New values of `CompanyListedClassificationType`, usable in `CompanyType/OtherClassification/@ListedType` (issuer, counterparty, management company, custodian, ...):

- `ÖNACE 2025` – Austrian version of NACE Rev. 2.1
- `ISIC Rev 4`, `ISIC Rev 5` – UN International Standard Industrial Classification
- `NAICS` – North American Industry Classification System
- `GICS`, `ICB`, `BICS`, `TRBC` – commercial industry classifications (MSCI / S&P, FTSE Russell, Bloomberg, LSEG)
- `ESA 2010` – institutional sector code according to ESA 2010, e.g. `S.11`, `S.122`
- `ISO 20275 ELF` – Entity Legal Form code as maintained by GLEIF

### Changed

- `CompanyType/OtherClassification`:
  - Documentation corrected: the element holds classifications, not identifiers (text had been copied from `OtherID`)
  - `ListedType="SIC"` is now documented as the US Standard Industrial Classification 1987; UK SIC 2007 is to be delivered via `FreeType`
  - Attribute `FreeType` is now of type `Text64Type` (was untyped). **Potentially restricting:** free type names longer than 64 characters are no longer valid

### Example

```xml
<Issuer>
  <Identifiers>
    <LEI>529900T8BM49AURSDO55</LEI>
  </Identifiers>
  <Name>Test AG</Name>
  <OtherClassification ListedType="GICS">45102010</OtherClassification>
  <OtherClassification ListedType="ESA 2010">S.11</OtherClassification>
  <OtherClassification ListedType="ISO 20275 ELF">AXSB</OtherClassification>
  <OtherClassification FreeType="UK SIC 2007">62012</OtherClassification>
</Issuer>
```
