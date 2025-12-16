# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is the **FundsXML Schema** repository - an XSD 1.1 schema defining the pan-European standard for fund data exchange (version 4.2.10). FundsXML enables communication between asset managers, custodian banks, distributors, regulators, and data vendors.

## Schema Architecture

**Two schema versions exist:**
- `FundsXML4.xsd` (root) — Flattened/pre-compiled schema (~2.5MB) for production validation
- `include_files/FundsXML4.xsd` — Modular development schema with includes

**Core modules in `include_files/`:**
- `FundsXML4_Core.xsd` — Base types, identifiers (ISIN, LEI, ISO codes)
- `FundsXML4_FundStaticData.xsd` — Fund master data (legal structure, domicile, benchmarks)
- `FundsXML4_FundDynamicData.xsd` — Time-varying data (NAV, portfolios)
- `FundsXML4_ShareClassData.xsd` — Share class prices, distributions, fees
- `FundsXML4_PortfolioData.xsd` — Position-level holdings
- `FundsXML4_AssetMasterData.xsd` — Security reference data
- `FundsXML4_TransactionData.xsd` — Trade transactions

**Regulatory modules:**
- `FundsXML4_RegulatoryReporting_EMT.xsd` — MiFID II target market & costs (V4-V4.3)
- `FundsXML4_RegulatoryReporting_EET.xsd` — SFDR ESG data (V1-V1.1.3)
- `FundsXML4_RegulatoryReporting_PRIIPS.xsd` — PRIIPs KID scenarios
- `FundsXML4_RegulatoryReporting_SolvencyII.xsd` — TPT look-through (V6-V7)
- `FundsXML4_RegulatoryReporting_KIID.xsd` — UCITS KIID
- `FundsXML4_RegulatoryReporting_EMIR.xsd` — Derivatives reporting
- `FundsXML4_RegulatoryReporting_EFT.xsd` — Distribution feedback

**Country extensions:**
- `FundsXML4_CountrySpecificData_AT.xsd` — Austria (OeNB, PKG2012)
- `FundsXML4_CountrySpecificData_DE.xsd` — Germany (real estate reporting)
- Also: DK, FR, LU, NL

## Key Constraints

**XSD 1.1 Required:** Standard validators like `xmllint` do not work. Use:
```bash
# Saxon-EE
java -cp saxon-ee.jar com.saxonica.Validate -xsd:FundsXML4.xsd document.xml

# Altova XMLSpy
XMLSpy.exe -validate document.xml -schema FundsXML4.xsd
```

**Cross-references:**
- `Asset.UniqueID` (xs:ID) ← `Position.UniqueID`, `Transaction.AssetUniqueID`, `Earning.AssetUniqueID` (xs:IDREF)
- `FundStaticData/Benchmarks/Benchmark/BenchmarkID` (xs:key) ← `FundDynamicData/Benchmarks/Benchmark/BenchmarkID` (xs:keyref)

## Document Structure

```
FundsXML4
├── ControlData              ← Required: UniqueDocumentID, ContentDate, DataSupplier
├── Funds/Fund[]             ← SingleFundFlag determines structure
│   ├── FundStaticData
│   ├── FundDynamicData
│   └── SingleFund/ShareClasses[] OR Subfunds[]
├── AssetMasterData/Asset[]  ← Referenced by positions via UniqueID
├── RegulatoryReportings     ← EMT, EET, PRIIPS, SolvencyII, etc.
└── CountrySpecificData      ← AT, DE extensions
```

## Common Type Patterns

- `AmountType` — Multi-currency with `ccy` attribute, optional `isFundCcy`/`isShareClassCcy` flags
- `IdentifiersType` — ISIN, LEI, Bloomberg, CUSIP, SEDOL, WKN, OtherID
- `Text16Type` through `Text1000Type` — Length-constrained strings
- `PercentageType` — Decimal max 100

## Working with this Repository

When modifying XSD files:
1. Edit files in `include_files/` for development
2. The flattened `FundsXML4.xsd` at root is the production artifact
3. Maintain bilingual documentation (xml:lang="en" and xml:lang="de")
4. Version is declared in schema `version` attribute (currently 4.2.10)
