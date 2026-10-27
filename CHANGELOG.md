# Changelog

All notable changes to the FundsXML schema are documented in this file.
New schema nodes are also marked in the XSD with `<xs:appinfo source="@since X.Y.Z"/>`.

## [4.2.12] - Unreleased

### Added

Initial and maintenance margin for futures. All new elements are optional, so existing documents remain valid.

Position level (`FundsXML4_PortfolioData.xsd`), type `FundAmountType`:

- `Position/Future/InitialMargin` – initial margin deposited for the futures position as of the valuation date
- `Position/Future/MaintenanceMargin` – minimum margin level for the position; falling below it triggers a margin call

Asset master data (`FundsXML4_AssetMasterData.xsd`), type `AmountType`:

- `Asset/AssetDetails/Future/InitialMarginPerContract` – initial margin requirement per contract as set by the exchange or clearing house
- `Asset/AssetDetails/Future/MaintenanceMarginPerContract` – maintenance margin requirement per contract as set by the exchange or clearing house

In `Position/Future` the new elements are placed between `BookRate` and `VariationMarginDaily`; in the asset master data they follow `InitialMarginMethod`.

Additional predefined classification systems for companies (`FundsXML4_Core.xsd`). New values of `CompanyListedClassificationType`, usable in `CompanyType/OtherClassification/@ListedType` (issuer, counterparty, management company, custodian, ...):

- `ÖNACE 2025` – Austrian version of NACE Rev. 2.1
- `ISIC Rev 4`, `ISIC Rev 5` – UN International Standard Industrial Classification
- `NAICS` – North American Industry Classification System
- `GICS`, `ICB`, `BICS`, `TRBC` – commercial industry classifications (MSCI / S&P, FTSE Russell, Bloomberg, LSEG)
- `ESA 2010` – institutional sector code according to ESA 2010, e.g. `S.11`, `S.122`
- `ISO 20275 ELF` – Entity Legal Form code as maintained by GLEIF

Nominal value per piece for bonds and commercial papers listed in pieces (`FundsXML4_AssetMasterData.xsd`), type `AmountType`, optional:

- `Asset/AssetDetails/Bond/NominalValuePerPiece` – nominal (face) value of one piece of the bond
- `Asset/AssetDetails/CommercialPaper/NominalValuePerPiece` – nominal (face) value of one piece of the CP/CD

To be delivered if `ListingUnit` is `P` (pieces). `Position/Bond/Nominal` resp. `Position/CommercialPaper/Nominal` then holds the number of pieces, and the total nominal of the position is the number of pieces multiplied by the nominal value per piece. In both types the new element follows `ListingUnit`.

Credit, securitisation and convertible bond characteristics in the asset master and portfolio data. They are regular core elements without TPT codes. Their values are at least as granular as the Solvency II Tripartite Template, so TPT V7 and V8 can be generated from them; the mapping is documented in `docs/REGULATORY_REPORTING.md`. All new elements are optional.

Asset master data (`FundsXML4_AssetMasterData.xsd`):

- `Asset/AssetDetails/Bond/SolvencyIISecuritisationCategory` and `Asset/AssetDetails/CommercialPaper/SolvencyIISecuritisationCategory` – securitisation category under Delegated Regulation (EU) 2015/35, new type `SolvencyIISecuritisationCategoryType` with the values `NotAssessed`, `STSSenior`, `STSNonSenior`, `Resecuritisation`, `NonSTSSenior`, `NonSTSNonSenior`, `NonSTSOther`, `TransitionalArt178a1`, `TransitionalArt178a3`, `TransitionalArt178a4`, `GuaranteedEIBEIF`, `NotASecuritisation`
- `Asset/AssetDetails/Bond/ExpectedMaturityDate` – expected maturity taking prepayments and option exercise into account; follows `MaturityDate`
- `Asset/AssetDetails/Bond/Seniority` – ranking in insolvency or resolution: `SeniorSecured`, `SeniorUnsecured`, `SeniorNonPreferred`, `Subordinated`, `Tier2`, `AdditionalTier1`
- `Asset/AssetDetails/Bond/BailIn` – treatment under a resolution regime: `NotAssessed`, `Statutory`, `Contractual`, `Excluded`, `NotApplicable`
- `Asset/AssetDetails/Bond/PaymentInKind` – payment-in-kind feature with `Type` (`None`, `Full`, `Partial`, `Toggle`), `Settlement` (`Capitalised`, `AdditionalSecurities`) and `Rate`

In `BondType` the category, `Seniority`, `BailIn` and `PaymentInKind` follow `Tranche`; in `CommercialPaperType` the category follows `IsAssetBacked`.

Company type (`FundsXML4_Core.xsd`), new value of `CompanyType/Type`:

- `Financial vehicle corporations engaged in securitisation transactions` – ESA 2010 sector S.125

Risk codes (`FundsXML4_PortfolioData.xsd`), new values of `RiskCode/ListedCode`:

- `CONVEXITY`, `CREDIT SENSITIVITY`, `CREDIT SENSITIVITY TO EXPECTED MATURITY`, `MODIFIED DURATION TO EXPECTED MATURITY`, `Z-SPREAD`

Convertible bonds:

- `Asset/AssetDetails/Bond/ConvertibleBond/UnderlyingUniqueID` (`FundsXML4_AssetMasterData.xsd`, `xs:IDREF`) – reference to the asset into which the bond can be converted, giving access to the master data of the underlying (name, currency, issuer). Follows `Underlying`, which keeps holding the identifiers only
- `Position/Bond/BondFloor` and `Position/Bond/OptionPremium` (`FundsXML4_PortfolioData.xsd`, `FundAmountType`) – bond floor and premium of the embedded conversion option. Both follow `CollateralValue`

Solvency II TPT V8 (`FundsXML4_RegulatoryReporting_SolvencyII.xsd`):

- `RegulatoryReportings/IndirectReporting/TripartiteTemplateSolvencyII_V8` (type `TripartiteTemplateSolvencyIIType_V8`) – Tripartite Template version 8.0 according to the FinDatEx consultation draft of 2026-07-07. The final version is not published yet, so the structure may still change. Against V7, 20 datapoints are removed, issuer identification accepts non-LEI codes, the securitisation category `e` is split into `e1`, `e2` and `e3`, and the datapoints 150 and 151 are new
- `SIIPositionType_V8`: `DerivativeOrConvertible/OptionCharacteristics/Convertible` with `BondFloorConvertibleInstrumentOnly` (TPT 127) and `OptionPremiumConvertibleInstrumentOnly` (TPT 128), both `xs:decimal` and optional – bond floor and option premium of convertible instruments, as `Convertible/BondFloor` and `Convertible/OptionPremium` in the earlier versions (`SIIPositionType`, `_V6`, `_V7`). Follows `ExerciseType`

### Fixed

- TPT V7 and V8, `Redemption/PIK` (TPT 146): contradicting facets `minInclusive=1` / `maxInclusive=2` removed. The values `0` (no PIK), `3` and `4` of the enumeration were rejected before
- TPT V6, V7 `LookThroughIdentifier` and V8 `OriginalPortfolioIdentification` (TPT 95): type changed from `ISINType` to `xs:string`, as the datapoint allows "ISIN or CUSIP or any other identification"
- `TripartiteTemplateSolvencyII_V8`: `@since` corrected to 4.2.12, and the non-English documentation named version 7.0 and its date
- TPT `DerivativeOrConvertible/OptionCharacteristics/ConversionRatio` (V4, V6, V7) and `ConversionFactor` (V8), TPT 62: now optional. The datapoint applies to convertible bonds (CIC 22) only, but the element was mandatory, so an option with `CallPutType`, `StrikePrice` or `ExerciseType` could not be delivered without a dummy value
- `ControlData/Version`: enumeration value `4.2.12` added, so that documents using the elements introduced in this version can declare it

### Changed

- Documentation only, no structural change:
  - EET fields 80020 to 80050 (four template versions) and 20170: the English documentation contained Spanish text; English wording restored, and the Spanish text of 20170 moved to its own slot
  - `AssetMasterData`: the French, Dutch, Czech, Hungarian, Danish, Polish and Swedish documentation of `AssetDetails/Loan`, `AssetDetails/Right`, `Ratings/Rating`, `RealEstateCompany`, `Yields`, `Coupon/Type`, `Coupon/PaymentFrequency`, `Coupon/BaseIndex`, `Redemption/Frequency`, `ConvertibleBond/ConversionRatio` and `ConvertibleBond/ContractSize` repeated the Spanish or a placeholder text and is now translated; a few Spanish and Italian texts of these elements were corrected as well
  - Missing documentation languages completed: every annotation with an English text now carries all eleven languages (1,973 texts added in `RegulatoryReporting_SolvencyII`, `PortfolioData`, `FundStaticData`, `RegulatoryReporting_EET`, `ShareClassData`, `AssetMasterData`, `RegulatoryReporting_EMT`, `AssetMgmtCompDynData` and `CountrySpecificData_DE`). 164 of them were taken over from identical English texts elsewhere in the schema, the others are new machine translations that have not been reviewed by native speakers
  - Mixed-language documentation corrected: 1,425 texts that contained English, Spanish, Dutch or German fragments in another language slot, or the untranslated English text, were rewritten in the slot's language (all modules, mainly `RegulatoryReporting_EET`, `_EMT`, `_SolvencyII`, `_PRIIPS`, `AssetMasterData` and `TransactionData`); 18 of them are English slots that contained German, French or Spanish text. Machine-corrected, not reviewed by native speakers
  - Datapoint identifiers restored in 547 translated texts of EET, EFT and PRIIPS (e.g. `10000_Hersteller_Name` back to `10000_Manufacturer_Name`); the identifier had been translated along with the description
  - TPT V7 `Securitisation/Securitised` (TPT 56): range corrected to a–j and the meaning of the codes added (English and German)
  - TPT V8, wording of the post-consultation template: `IssuerIdentificationCode` (47), `IdentificationOfTheGroup` (50), `IssuerIdentificationCodeOfTheUnderlyingAsset` (81) and `IdentificationOfTheGroupOfTheUnderlyingAsset` (84) are alphanumeric with up to 20 characters; `GroupingCodeForMultipleLegInstruments` (16) is alphanumeric with up to 255 characters, its text had been copied from the contract size datapoint
  - TPT V8 `Securitisation`: codification is up to two characters (`e1`, `e2`, `e3`); `TPTVersion` names V8.0 as consultation draft instead of a date placeholder
  - `AssetTypeType`: `SC` is the share class (formerly fund), not "Structured Credit"
  - Spanish text in the English and German documentation replaced for `AssetDetails/Loan`, `AssetDetails/Right`, `Ratings/Rating`, `RealEstateCompany`, `Yields`, and in `Bond` and `CommercialPaper` for `Coupon/Type`, `Coupon/PaymentFrequency`, `Coupon/BaseIndex` and (Bond) `Redemption/Frequency`
  - `Bond/Tranche` described as the number of the issue tranche; Dutch text of `Tranche` and `Securitized` was Spanish

- Documentation of `ConvertibleBond/ConversionPrice` and `ConversionRatio` made precise in English and German (price per share resp. number of shares per bond); the English text of `ConvertibleBond/ContractSize` was German. No structural change

- German documentation of `ListingUnit` in `BondType` and `CommercialPaperType` corrected: the value for pieces is `P` (text said `S`). No structural change

- German documentation of `Commodity/ListingUnit` corrected: the values are `P` (pieces) and `U` (units); the text described `S` and `N`. No structural change

- `CompanyType/OtherClassification`:
  - Documentation corrected: the element holds classifications, not identifiers (text had been copied from `OtherID`)
  - `ListedType="SIC"` is now documented as the US Standard Industrial Classification 1987; UK SIC 2007 is to be delivered via `FreeType`
  - Attribute `FreeType` is now of type `Text64Type` (was untyped). **Potentially restricting:** free type names longer than 64 characters are no longer valid

- Documentation of `Position/Future/VariationMarginDaily` and `Position/Future/VariationMarginOverall` made precise (no structural change):
  - `VariationMarginDaily` – variation margin of the valuation date, i.e. the daily mark-to-market settlement since the previous valuation date
  - `VariationMarginOverall` – cumulative variation margin since the position was opened
  - Sign convention for both: positive = received by the fund, negative = paid
  - Missing French and Dutch documentation added

### Example

```xml
<Position>
  <UniqueID>ID_FUT_1</UniqueID>
  ...
  <Future>
    <Contracts>10</Contracts>
    <InitialMargin>
      <Amount ccy="EUR" isFundCcy="true">32000.00</Amount>
    </InitialMargin>
    <MaintenanceMargin>
      <Amount ccy="EUR" isFundCcy="true">29000.00</Amount>
    </MaintenanceMargin>
    <VariationMarginDaily>
      <Amount ccy="EUR" isFundCcy="true">-1250.00</Amount>
    </VariationMarginDaily>
    <VariationMarginOverall>
      <Amount ccy="EUR" isFundCcy="true">4800.00</Amount>
    </VariationMarginOverall>
  </Future>
</Position>
```

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

```xml
<Asset>
  <UniqueID>ID_BOND_1</UniqueID>
  ...
  <AssetDetails>
    <Bond>
      <ConvertibleFlag>false</ConvertibleFlag>
      <ListingUnit>P</ListingUnit>
      <NominalValuePerPiece>
        <Amount ccy="EUR">1000.00</Amount>
      </NominalValuePerPiece>
      <MaturityDate>2031-06-30</MaturityDate>
    </Bond>
  </AssetDetails>
</Asset>
```
