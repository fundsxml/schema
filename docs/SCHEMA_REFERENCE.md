<h1 align="center">Schema Reference</h1>

<p align="center">
  <strong>FundsXML 4.2.10 Type Catalog</strong>
</p>

<p align="center">
  <a href="#simple-types">Simple Types</a> ·
  <a href="#complex-types">Complex Types</a> ·
  <a href="#enumerations">Enumerations</a> ·
  <a href="#constraints">Constraints</a>
</p>

---

## Simple Types

### Text Types

| Type | Max Length | Usage |
|------|------------|-------|
| `Text16Type` | 16 | Short codes, MIC |
| `Text32Type` | 32 | Tickers |
| `Text64Type` | 64 | Short names |
| `Text128Type` | 128 | Document IDs |
| `Text256Type` | 256 | Standard names |
| `Text500Type` | 500 | Long names |
| `Text1000Type` | 1000 | Descriptions |

### Identifier Types

| Type | Format | Standard | Example |
|------|--------|----------|---------|
| `ISINType` | 12 chars | ISO 6166 | `AT0000A1Z882` |
| `LEICodeType` | 20 chars | ISO 17442 | `PQOH26KWDF7CG10L6792` |
| `ISOCountryCodeType` | 2 chars | ISO 3166-1 | `AT`, `DE` |
| `ISOCurrencyCodeType` | 3 chars | ISO 4217 | `EUR`, `USD` |
| `MICCodeType` | 4 chars | ISO 10383 | `XETR`, `XLON` |
| `BicCodeType` | 8-11 chars | ISO 9362 | `GIBAATWWXXX` |

### Numeric Types

| Type | Description |
|------|-------------|
| `PercentageType` | Decimal, max 100 |
| `positiveDecimals` | Non-negative decimal |

---

## Complex Types

### IdentifiersType

Container for security/entity identifiers:

```xml
<Identifiers>
   <ISIN>AT0000A1Z882</ISIN>
   <LEI>PQOH26KWDF7CG10L6792</LEI>
   <Bloomberg>
      <Ticker>ESPASEL</Ticker>
      <Exchange>AV</Exchange>
      <BBGID>BBG001234567</BBGID>
   </Bloomberg>
   <GermanWKN>A1Z882</GermanWKN>
   <CUSIP>123456789</CUSIP>
   <SEDOL>1234567</SEDOL>
   <SwissValorenCode>12345678</SwissValorenCode>
   <OtherID type="INTERNAL">FUND001</OtherID>
</Identifiers>
```

### AmountType

Multi-currency container:

```xml
<TotalValue>
   <Amount ccy="EUR">1000000.00</Amount>
   <Amount ccy="USD">1100000.00</Amount>
</TotalValue>
```

**FundAmountType** extends with currency context:

| Attribute | Description |
|-----------|-------------|
| `ccy` | Currency code (required) |
| `isFundCcy` | Fund's base currency |
| `isSubfundCcy` | Subfund's currency |
| `isShareClassCcy` | Share class currency |

### CompanyType

| Element | Type | Required |
|---------|------|----------|
| `Identifiers` | IdentifiersType | No |
| `Name` | Text500Type | Yes |
| `LegalName` | Text500Type | No |
| `LegalForm` | Text256Type | No |
| `Address` | AddressType | No |
| `BusinessCountry` | ISOCountryCodeType | No |
| `ParentCompany` | CompanyType | No |
| `Type` | string | No |

### AddressType

| Element | Type |
|---------|------|
| `StreetName` | Text256Type |
| `StreetNumber` | Text32Type |
| `CityName` | Text100Type |
| `CityCode` | Text16Type |
| `Country` | ISOCountryCodeType |
| `Email` | EmailAddressType |
| `Homepage` | Text256Type |

### DataSupplierType

| Element | Type | Required |
|---------|------|----------|
| `SystemCountry` | ISOCountryCodeType | Yes |
| `Short` | Text64Type | Yes |
| `Name` | Text256Type | Yes |
| `Type` | Text64Type | Yes |
| `Contact` | ContactType | No |

### ControlDataType

| Element | Type | Required |
|---------|------|----------|
| `UniqueDocumentID` | Text128Type | Yes |
| `DocumentGenerated` | dateTime | Yes |
| `Version` | string | No |
| `ContentDate` | date | Yes |
| `DataSupplier` | DataSupplierType | Yes |
| `DataOperation` | enum | No |
| `Language` | xs:language | No |
| `ModuleUsage` | list | No |
| `CustomAttributes` | AttributesType | No |

### FundType

| Element | Type | Required |
|---------|------|----------|
| `Identifiers` | IdentifiersType | Yes |
| `Names` | NamesType | Yes |
| `Currency` | ISOCurrencyCodeType | Yes |
| `SingleFundFlag` | boolean | Yes |
| `FundStaticData` | FundStaticDataType | No |
| `FundDynamicData` | FundDynamicDataType | No |
| `SingleFund` | SingleFundType | Choice |
| `Subfunds` | list | Choice |

### ShareClassType

| Element | Type |
|---------|------|
| `Identifiers` | IdentifiersType |
| `Names` | NamesType |
| `Currency` | ISOCurrencyCodeType |
| `ShareClassType` | ShareClassTypeType |
| `InceptionDate` | date |
| `RegistrationCountries` | list |
| `Prices` | list |
| `TotalAssetValues` | list |
| `Distributions` | list |
| `Fees` | list |
| `PerformanceFigures` | list |

### PositionType

| Element | Type |
|---------|------|
| `UniqueID` | xs:IDREF |
| `Identifiers` | IdentifiersType |
| `Currency` | ISOCurrencyCodeType |
| `TotalValue` | FundAmountType |
| `TotalPercentage` | PercentageType |
| `AveragePurchasePrice` | AmountType |
| `FXRates` | FXRatesType |
| `PriceDate` | date |
| `[Asset Type]` | choice |

### TransactionType

| Element | Type |
|---------|------|
| `TransactionID` | string |
| `CancellationFlag` | boolean |
| `AssetUniqueID` | xs:IDREF |
| `TransactionKind` | string |
| `ClosingDate` | date |
| `NominalOrUnitsOrContracts` | decimal |
| `MarketValue` | AmountType |
| `SettlementAmount` | AmountType |
| `Expenses` | list |

---

## Enumerations

### DataOperation

| Value | Description |
|-------|-------------|
| `INITIAL` | First-time delivery |
| `AMEND` | Update/correction |
| `DELETE` | Removal |

### ListedLegalStructure

**UCITS:**
`UCITS` · `UCITS-SICAV` · `UCITS-SICAF` · `UCITS-CONTRACTUAL`

**AIF:**
`AIF` · `AIF-HEDGE FUND` · `AIF-PRIVATE EQUITY FUND` · `AIF-VENTURE CAPITAL FUND` · `AIF-REAL ESTATE FUND` · `AIF-REIT` · `AIF-INFRASTRUCTURE FUND` · `AIF-COMMODITY FUND` · `AIF-ELTIF` · `AIF-EUVECA` · `AIF-EUSEF`

**Other:**
`SPV`

### OpenClosedEnded

| Value | Description |
|-------|-------------|
| `OPEN` | Open-ended |
| `CLOSED` | Closed-ended |

### ClosedType

| Value | Description |
|-------|-------------|
| `HARD` | No new investors |
| `SOFT` | Existing only |

### SFDRProductType

| Value | Article | Description |
|-------|---------|-------------|
| `0` | — | Not applicable |
| `6` | Art. 6 | No sustainability claim |
| `8` | Art. 8 | ESG characteristics |
| `9` | Art. 9 | Sustainable objective |

### BenchmarkType

`Market Index` · `Blended` · `Custom` · `Absolute Value` · `Peer Groups` · `Factor Based` · `Returns Based` · `ETF` · `Other`

### BenchmarkUsage

`OFFICIAL` · `INTERNAL` · `PERFORMANCE ATTRIBUTION` · `PRIIP KID` · `UCITS KIID`

### PriceNature

| Value | Description |
|-------|-------------|
| `OFFICIAL` | Official NAV |
| `ESTIMATED` | Estimated |
| `TECHNICAL` | Technical/accounting |

### RegistrationStatus

`Registered` · `De-registered`

### EarningKind

`Coupon` · `Dividend` · `Fund distribution` · `Interest deposit` · `Interest swap` · `Other`

### Classification Groups

`AMF` · `BLOOMBERG` · `BVI` · `EFAMA` · `ESMA` · `FATCA` · `FIX` · `IMA` · `LIPPER` · `MIFID` · `MORNINGSTAR` · `VOEIG` · `WM` · `GERMAN CBCL` · `CIC` · `CFI`

### Pricing Sources

`BARCLAYS` · `BLOOMBERG` · `BROKERS` · `FACTSET` · `JPMORGAN` · `LPC` · `MARKIT` · `MERRILL` · `NTRS` · `REUTERS`

---

## Constraints

### Benchmark Reference

Static benchmarks define IDs, dynamic benchmarks reference them:

```xml
<!-- Static: defines key -->
<FundStaticData>
   <Benchmarks>
      <Benchmark>
         <BenchmarkID>BM001</BenchmarkID>  <!-- xs:key -->
      </Benchmark>
   </Benchmarks>
</FundStaticData>

<!-- Dynamic: references key -->
<FundDynamicData>
   <Benchmarks>
      <Benchmark>
         <BenchmarkID>BM001</BenchmarkID>  <!-- xs:keyref -->
      </Benchmark>
   </Benchmarks>
</FundDynamicData>
```

### Asset Reference

Positions link to AssetMasterData via IDREF:

| Location | Type |
|----------|------|
| `Asset/UniqueID` | xs:ID (definition) |
| `Position/UniqueID` | xs:IDREF (reference) |
| `Transaction/AssetUniqueID` | xs:IDREF (reference) |
| `Earning/AssetUniqueID` | xs:IDREF (reference) |

---

## Schema Statistics

| Metric | Count |
|--------|-------|
| Complex Types | ~1,000+ |
| Simple Types | ~150+ |
| Global Elements | 1 |
| Total Lines | ~40,844 |

---

## File Reference

| Component | File |
|-----------|------|
| Core types | `FundsXML4_Core.xsd` |
| Fund static | `FundsXML4_FundStaticData.xsd` |
| Fund dynamic | `FundsXML4_FundDynamicData.xsd` |
| Share class | `FundsXML4_ShareClassData.xsd` |
| Portfolio | `FundsXML4_PortfolioData.xsd` |
| Assets | `FundsXML4_AssetMasterData.xsd` |
| Transactions | `FundsXML4_TransactionData.xsd` |
