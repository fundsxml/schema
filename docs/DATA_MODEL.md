<h1 align="center">Data Model</h1>

<p align="center">
  <strong>FundsXML 4.2.10 Entity Relationships</strong>
</p>

<p align="center">
  <a href="#document-structure">Structure</a> ·
  <a href="#fund-hierarchy">Funds</a> ·
  <a href="#asset-types">Assets</a> ·
  <a href="#portfolio-model">Portfolio</a> ·
  <a href="#relationships">Relationships</a>
</p>

---

## Document Structure

```
FundsXML4
├── ControlData              ← Required
├── Funds
│   └── Fund[]
├── AssetMgmtCompanyDynData
├── AssetMasterData
│   └── Asset[]
├── Documents
├── RegulatoryReportings
├── CountrySpecificData
└── ds:Signature
```

### Data Categories

| Section | Type | Update Frequency |
|---------|------|------------------|
| ControlData | Metadata | Per document |
| FundStaticData | Master data | Low |
| FundDynamicData | Time-series | High |
| AssetMasterData | Reference | On change |
| RegulatoryReportings | Compliance | Per regulation |

---

## Fund Hierarchy

### Single Fund

```
Fund (SingleFundFlag = true)
├── Identifiers (LEI)
├── Names
├── Currency
├── FundStaticData
├── FundDynamicData
└── SingleFund
    └── ShareClasses[]
        └── ShareClass (ISIN)
```

### Umbrella Structure

```
Fund (SingleFundFlag = false)
├── Identifiers (LEI)
├── Names
├── Currency
├── FundStaticData
└── Subfunds[]
    └── Subfund
        ├── SubfundStaticData
        └── ShareClasses[]
            └── ShareClass (ISIN)
```

### Fund Static Data

| Element | Description |
|---------|-------------|
| DomicileCountry | Legal domicile |
| ListedLegalStructure | UCITS, AIF, etc. |
| InceptionDate | Launch date |
| OpenClosedEnded | OPEN / CLOSED |
| Administrator | Fund admin |
| Custodian | Depositary |
| PortfolioManagers | Named managers |
| Benchmarks | Reference indices |
| Classifications | Fund categories |
| SFDRProductType | 0, 6, 8, or 9 |

### Fund Dynamic Data

| Element | Description |
|---------|-------------|
| TotalAssetValues | NAV time-series |
| Portfolios | Holdings per date |
| Benchmarks | Benchmark values |

---

## Asset Types

FundsXML supports 15+ asset classes:

### Equity

| Element | Description |
|---------|-------------|
| Units | Number of shares |
| Price | Current price |
| DividendYield | Yield % |
| IssuerCompany | Issuer details |
| ShareType | Ordinary, Preferred |

### Bond

| Element | Description |
|---------|-------------|
| Nominal | Face value |
| CleanPrice | Price ex-interest |
| DirtyPrice | Price with interest |
| CouponRate | Coupon % |
| MaturityDate | Maturity |
| Rating | Credit rating |

### Derivatives

**Option**
- OptionType (CALL/PUT)
- StrikePrice
- ExpirationDate
- Greeks (Delta, Gamma, Vega, Theta)

**Future**
- ContractSize
- SettlementType
- LastTradingDate

**Swap**
- SwapType
- FixedLeg / FloatingLeg
- NotionalAmount

### Other Types

| Type | Description |
|------|-------------|
| FXForward | Currency forwards |
| Repo | Repurchase agreements |
| FixedTimeDeposit | Term deposits |
| RealEstate | Property holdings |
| REIT | REIT investments |
| Commodity | Commodity positions |
| Crypto | Cryptocurrency |

---

## Portfolio Model

### Position Structure

```
Portfolio
├── NavDate
└── Positions[]
    └── Position
        ├── UniqueID ──────► AssetMasterData
        ├── TotalValue
        ├── TotalPercentage
        └── [Asset Type Data]
```

### Position Elements

| Element | Description |
|---------|-------------|
| UniqueID | Asset reference |
| TotalValue | Market value |
| TotalPercentage | % of portfolio |
| AveragePurchasePrice | Cost basis |
| FXRates | Valuation rates |
| PriceDate | Price date |

### Transaction Structure

```
Transaction
├── TransactionID
├── AssetUniqueID ──────► AssetMasterData
├── TransactionKind
├── Dates (Trade, Settlement, Value)
├── Quantity
├── Prices (Clean, Dirty, Agreed)
├── Values (Market, Settlement)
└── Expenses[]
```

### Earning Structure

```
Earning
├── EarningID
├── AssetUniqueID ──────► AssetMasterData
├── EarningKind (Coupon, Dividend, etc.)
├── Dates
└── Values
```

---

## Share Class Model

```
ShareClass
├── Identifiers (ISIN)
├── Names
├── Currency
├── InceptionDate
├── RegistrationCountries[]
├── Prices[]
│   └── Price
│       ├── NavDate
│       ├── NavPrice
│       └── PriceNature
├── TotalAssetValues[]
├── Distributions[]
├── Fees[]
├── PerformanceFigures
└── Flows[]
```

### Price Time-Series

| Element | Description |
|---------|-------------|
| NavDate | Valuation date |
| NavPrice | NAV per share |
| SubscriptionPrice | Buy price |
| RedemptionPrice | Sell price |
| PriceNature | OFFICIAL / ESTIMATED |

### Distribution

| Element | Description |
|---------|-------------|
| ExDate | Ex-dividend date |
| PaymentDate | Payment date |
| GrossDividendAmount | Gross amount |
| NetDividendAmount | Net amount |

---

## Relationships

### Reference Diagram

```
┌──────────────────────────────────────────────────────┐
│                   AssetMasterData                     │
│  ┌────────────────────────────────────────────────┐  │
│  │ Asset                                          │  │
│  │   UniqueID: "AAPL_001" ◄───────────────┐      │  │
│  │   ISIN: "US0378331005"                 │      │  │
│  │   Name: "Apple Inc."                   │      │  │
│  └────────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────────┘
                                            │
                          xs:IDREF          │
                                            │
┌──────────────────────────────────────────────────────┐
│                      Portfolio                        │
│  ┌────────────────────────────────────────────────┐  │
│  │ Position                                       │  │
│  │   UniqueID: "AAPL_001" ────────────────┘      │  │
│  │   TotalValue: EUR 5,000,000                   │  │
│  │   TotalPercentage: 2.00%                      │  │
│  └────────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────────┘
```

### Key References

| Source | Target | Type |
|--------|--------|------|
| Position.UniqueID | Asset.UniqueID | xs:IDREF |
| Transaction.AssetUniqueID | Asset.UniqueID | xs:IDREF |
| Earning.AssetUniqueID | Asset.UniqueID | xs:IDREF |
| Benchmark (Dynamic) | Benchmark (Static) | xs:keyref |

### Benchmark Constraint

```xml
<!-- Static: defines key -->
<FundStaticData>
   <Benchmarks>
      <Benchmark>
         <BenchmarkID>BM001</BenchmarkID>
      </Benchmark>
   </Benchmarks>
</FundStaticData>

<!-- Dynamic: references key -->
<FundDynamicData>
   <Benchmarks>
      <Benchmark>
         <BenchmarkID>BM001</BenchmarkID>
      </Benchmark>
   </Benchmarks>
</FundDynamicData>
```

---

## Country Extensions

### Austria (AT)

**OeNB Reporting**
- Subscription/Redemption flows
- Distribution data
- Fund reporting type (OFI)

**PKG2012 Reporting**
- Position-level categories
- 3-digit PKG codes
- Summary aggregations

### Germany (DE)

**Real Estate Reporting**
- Rental contract expirations
- Property purchases/sales
- Domestic/foreign allocation

### Extension Structure

```
CountrySpecificData
├── AT
│   ├── OeNB
│   │   ├── Subscription
│   │   ├── Redemption
│   │   └── Distribution
│   └── PKG2012
│       └── Positions[]
└── DE
    └── RealEstateReporting
        ├── RentalContracts
        └── PurchasesAndSales
```

---

## Multi-Currency Pattern

FundsXML supports multiple currencies per amount:

```xml
<TotalValue>
   <Amount ccy="EUR" isFundCcy="true">100000000.00</Amount>
   <Amount ccy="USD">110000000.00</Amount>
   <Amount ccy="GBP">85000000.00</Amount>
</TotalValue>
```

### Currency Flags

| Flag | Description |
|------|-------------|
| isFundCcy | Fund's base currency |
| isSubfundCcy | Subfund's currency |
| isShareClassCcy | Share class currency |

---

## File Reference

| Model | Schema File |
|-------|-------------|
| Core types | `FundsXML4_Core.xsd` |
| Fund static | `FundsXML4_FundStaticData.xsd` |
| Fund dynamic | `FundsXML4_FundDynamicData.xsd` |
| Share class | `FundsXML4_ShareClassData.xsd` |
| Portfolio | `FundsXML4_PortfolioData.xsd` |
| Assets | `FundsXML4_AssetMasterData.xsd` |
| Transactions | `FundsXML4_TransactionData.xsd` |
| Country AT | `FundsXML4_CountrySpecificData_AT.xsd` |
| Country DE | `FundsXML4_CountrySpecificData_DE.xsd` |
