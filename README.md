<p align="center">
  <img src="https://www.fundsxml.org/wp-content/uploads/2025/11/logo_FundsXML_179x43-1.png" alt="FundsXML Logo" width="179"/>
</p>

<h1 align="center">FundsXML Schema</h1>

<p align="center">
  <strong>The Pan-European Standard for Fund Data Exchange</strong>
</p>

<p align="center">
  <a href="#features">Features</a> ·
  <a href="#getting-started">Getting Started</a> ·
  <a href="#schema-modules">Schema Modules</a> ·
  <a href="#documentation">Documentation</a> ·
  <a href="https://www.fundsxml.org">Website</a>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/version-4.2.10-598EFF?style=flat-square" alt="Version 4.2.10"/>
  <img src="https://img.shields.io/badge/XSD-1.1-49BCE3?style=flat-square" alt="XSD 1.1"/>
  <img src="https://img.shields.io/badge/license-MIT-14B8A6?style=flat-square" alt="MIT License"/>
</p>

---

## Overview

FundsXML is an open, internationally recognized XML standard for exchanging fund data. It enables seamless communication between asset managers, custodian banks, distributors, regulators, and data vendors across Europe.

**Why FundsXML?**

- **Standardized Structure** — Precisely defined data fields and relationships
- **Automated Validation** — XML schema validation before processing
- **Complex Data Support** — Nested structures for detailed fund representation
- **Regulatory Compliance** — Built-in support for MiFID II, SFDR, PRIIPs, Solvency II
- **Open & Free** — MIT licensed, community-driven development

---

## Features

### Fund Data
Complete lifecycle management for funds, subfunds, and share classes including NAV, distributions, fees, and performance.

### Portfolio Holdings
Position-level data for 15+ asset classes: equities, bonds, derivatives, real estate, and more.

### Regulatory Reporting
Native support for European regulatory templates:

| Template | Regulation | Description |
|----------|------------|-------------|
| **EMT** | MiFID II | Target market & cost disclosure |
| **EET** | SFDR | ESG & sustainability data |
| **PRIIPS** | PRIIPs | Key Information Document data |
| **TPT** | Solvency II | Look-through reporting |
| **KIID** | UCITS | Key Investor Information |

### Country Extensions
Specialized modules for Austria (OeNB, PKG) and Germany (BVI, real estate reporting).

---

## Getting Started

### Schema Files

| File | Purpose |
|------|---------|
| `FundsXML4.xsd` | **Production** — Flattened schema (2.5 MB) for validation |
| `include_files/FundsXML4.xsd` | **Development** — Modular schema with includes |

### Basic Structure

```xml
<?xml version="1.0" encoding="UTF-8"?>
<FundsXML4 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:noNamespaceSchemaLocation="FundsXML4.xsd">

   <ControlData>
      <UniqueDocumentID>DOC_2025_001</UniqueDocumentID>
      <DocumentGenerated>2025-07-04T10:00:00Z</DocumentGenerated>
      <ContentDate>2025-06-30</ContentDate>
      <DataSupplier>
         <SystemCountry>AT</SystemCountry>
         <Short>AM_ONE</Short>
         <Name>Asset Management ONE</Name>
         <Type>Asset Manager</Type>
      </DataSupplier>
   </ControlData>

   <Funds>
      <Fund>
         <Identifiers>
            <LEI>PQOH26KWDF7CG10L6792</LEI>
         </Identifiers>
         <Names>
            <OfficialName>Sample Fund</OfficialName>
         </Names>
         <Currency>EUR</Currency>
         <SingleFundFlag>true</SingleFundFlag>
      </Fund>
   </Funds>

</FundsXML4>
```

### Validation

FundsXML requires an **XSD 1.1** compliant validator:

```bash
# Altova XMLSpy
XMLSpy.exe -validate document.xml -schema FundsXML4.xsd

# Saxon-EE
java -cp saxon-ee.jar com.saxonica.Validate -xsd:FundsXML4.xsd document.xml

# Oxygen XML
oxygen-validate.sh -schema FundsXML4.xsd -instance document.xml
```

> **Note:** Standard tools like `xmllint` do not support XSD 1.1.

---

## Schema Modules

### Core Modules

```
include_files/
├── FundsXML4.xsd                    # Entry point
├── FundsXML4_Core.xsd               # Base types & identifiers
├── FundsXML4_FundStaticData.xsd     # Fund master data
├── FundsXML4_FundDynamicData.xsd    # Time-varying fund data
├── FundsXML4_ShareClassData.xsd     # Share class information
├── FundsXML4_PortfolioData.xsd      # Portfolio holdings
├── FundsXML4_AssetMasterData.xsd    # Security reference data
└── FundsXML4_TransactionData.xsd    # Transaction records
```

### Regulatory Modules

```
├── FundsXML4_RegulatoryReporting_EMT.xsd       # MiFID II
├── FundsXML4_RegulatoryReporting_EET.xsd       # SFDR/ESG
├── FundsXML4_RegulatoryReporting_PRIIPS.xsd    # PRIIPs KID
├── FundsXML4_RegulatoryReporting_SolvencyII.xsd # Insurance
├── FundsXML4_RegulatoryReporting_KIID.xsd      # UCITS
├── FundsXML4_RegulatoryReporting_EMIR.xsd      # Derivatives
└── FundsXML4_RegulatoryReporting_EFT.xsd       # Distribution
```

### Country Extensions

```
├── FundsXML4_CountrySpecificData.xsd      # Wrapper
├── FundsXML4_CountrySpecificData_AT.xsd   # Austria
├── FundsXML4_CountrySpecificData_DE.xsd   # Germany
├── FundsXML4_CountrySpecificData_DK.xsd   # Denmark
├── FundsXML4_CountrySpecificData_FR.xsd   # France
├── FundsXML4_CountrySpecificData_LU.xsd   # Luxembourg
└── FundsXML4_CountrySpecificData_NL.xsd   # Netherlands
```

---

## Document Structure

```
FundsXML4
├── ControlData              ← Required: Document metadata
├── Funds                    ← Fund/subfund/share class data
├── AssetMgmtCompanyDynData  ← Asset manager statistics
├── AssetMasterData          ← Security reference data
├── Documents                ← Attached documents
├── RegulatoryReportings     ← EMT, EET, PRIIPS, etc.
├── CountrySpecificData      ← AT, DE extensions
└── ds:Signature             ← XML digital signature
```

### Key Types

| Type | Description |
|------|-------------|
| `IdentifiersType` | ISIN, LEI, Bloomberg, CUSIP, SEDOL, WKN |
| `AmountType` | Multi-currency amount container |
| `CompanyType` | Company with address and identifiers |
| `FundType` | Complete fund structure |
| `ShareClassType` | Share class with prices and flows |
| `PositionType` | Portfolio position |
| `TransactionType` | Trade transaction |

---

## Asset Classes

FundsXML supports 15+ asset types:

| Category | Types |
|----------|-------|
| **Equity** | Stocks, shares |
| **Fixed Income** | Bonds, convertibles, commercial paper |
| **Derivatives** | Options, futures, swaps, FX forwards |
| **Funds** | Fund shares, ETFs, REITs |
| **Money Market** | Deposits, repos, call money |
| **Alternatives** | Real estate, private equity, commodities, crypto |

---

## Example

Complete example with fund, share classes, portfolio, and asset master data:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<FundsXML4 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:noNamespaceSchemaLocation="FundsXML4.xsd">
   <ControlData>
      <UniqueDocumentID>DOC_2025_001</UniqueDocumentID>
      <DocumentGenerated>2025-07-04T10:00:00Z</DocumentGenerated>
      <Version>4.2.10</Version>
      <ContentDate>2025-06-30</ContentDate>
       <DataSupplier>
           <SystemCountry>AT</SystemCountry>
           <Short>AM_ONE</Short>
           <Name>Asset Management ONE</Name>
           <Type>Asset Manager</Type>
       </DataSupplier>
      <DataOperation>INITIAL</DataOperation>
   </ControlData>

   <Funds>
      <Fund>
         <Identifiers>
            <LEI>PQOH26KWDF7CG10L6792</LEI>
         </Identifiers>
         <Names>
            <OfficialName>Sample Fund</OfficialName>
         </Names>
         <Currency>EUR</Currency>
         <SingleFundFlag>true</SingleFundFlag>

         <FundStaticData>
            <DomicileCountry>AT</DomicileCountry>
            <ListedLegalStructure>UCITS</ListedLegalStructure>
            <InceptionDate>2001-03-15</InceptionDate>
            <OpenClosedEnded>OPEN</OpenClosedEnded>
            <SFDRProductType>8</SFDRProductType>
         </FundStaticData>

         <FundDynamicData>
            <TotalAssetValues>
               <TotalAssetValue>
                  <Date>2025-06-30</Date>
                  <TotalNetAssets>
                     <Amount ccy="EUR">250000000.00</Amount>
                  </TotalNetAssets>
               </TotalAssetValue>
            </TotalAssetValues>
            <Portfolios>
               <Portfolio>
                  <NavDate>2025-06-30</NavDate>
                  <Positions>
                     <Position>
                        <UniqueID>POS_AAPL</UniqueID>
                        <TotalValue>
                           <Amount ccy="EUR">5000000.00</Amount>
                        </TotalValue>
                        <TotalPercentage>2.00</TotalPercentage>
                        <Equity>
                           <Units>25000</Units>
                           <Price>
                              <Amount ccy="USD">200.00</Amount>
                           </Price>
                        </Equity>
                     </Position>
                  </Positions>
               </Portfolio>
            </Portfolios>
         </FundDynamicData>

         <SingleFund>
            <ShareClasses>
               <ShareClass>
                  <Identifiers>
                     <ISIN>AT0000A1Z882</ISIN>
                  </Identifiers>
                  <Names>
                     <OfficialName>Sample Fund - Shareclass A</OfficialName>
                  </Names>
                  <Currency>EUR</Currency>
                  <InceptionDate>2001-03-15</InceptionDate>
                  <Prices>
                     <Price>
                        <NavDate>2025-06-30</NavDate>
                        <PriceCurrency>EUR</PriceCurrency>
                        <PriceNature>OFFICIAL</PriceNature>
                        <NavPrice>185.42</NavPrice>
                     </Price>
                  </Prices>
               </ShareClass>
            </ShareClasses>
         </SingleFund>
      </Fund>
   </Funds>

   <AssetMasterData>
      <Asset>
         <UniqueID>POS_AAPL</UniqueID>
         <Identifiers>
            <ISIN>US0378331005</ISIN>
            <Bloomberg>
               <Ticker>AAPL</Ticker>
               <Exchange>US</Exchange>
            </Bloomberg>
         </Identifiers>
         <Currency>USD</Currency>
         <Name>Apple Inc.</Name>
         <AssetType>EQ</AssetType>
      </Asset>
   </AssetMasterData>
</FundsXML4>
```

---

## Documentation

| Document | Description |
|----------|-------------|
| **[Schema Reference](docs/SCHEMA_REFERENCE.md)** | Complete type catalog and enumerations |
| **[Regulatory Reporting](docs/REGULATORY_REPORTING.md)** | EMT, EET, PRIIPS, Solvency II details |
| **[Data Model](docs/DATA_MODEL.md)** | Entity relationships and hierarchies |

---

## Community

- **Website:** [fundsxml.org](https://www.fundsxml.org)
- **Implementation Guide:** [fundsxml.org/implementation-guide](https://www.fundsxml.org/implementation-guide/)
- **Downloads:** [fundsxml.org/downloads](https://www.fundsxml.org/downloads/)
- **GitHub:** [github.com/fundsxml/schema](https://github.com/fundsxml/schema)

---

## License

FundsXML is released under the [MIT License](LICENSE).

```
MIT License

Copyright (c) 2020 FundsXML

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software.
```
