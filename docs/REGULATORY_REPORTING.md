<h1 align="center">Regulatory Reporting</h1>

<p align="center">
  <strong>FundsXML 4.2.10 Regulatory Templates</strong>
</p>

<p align="center">
  <a href="#emt">EMT</a> ·
  <a href="#eet">EET</a> ·
  <a href="#priips">PRIIPS</a> ·
  <a href="#solvency-ii">Solvency II</a> ·
  <a href="#kiid">KIID</a> ·
  <a href="#emir">EMIR</a> ·
  <a href="#eft">EFT</a>
</p>

---

## Overview

FundsXML provides native support for European regulatory reporting requirements:

| Template | Regulation | Versions | Purpose |
|----------|------------|----------|---------|
| **EMT** | MiFID II | V4 – V4.3 | Target market & costs |
| **EET** | SFDR | V1 – V1.1.3 | ESG & sustainability |
| **PRIIPS** | PRIIPs | V2 – V2.0 | Key Information Document |
| **TPT** | Solvency II | V5 – V7 | Insurance look-through |
| **KIID** | UCITS | V1 | Key Investor Information |
| **EMIR** | EMIR | V1 | Derivatives reporting |
| **EFT** | MiFID II/IDD | V1 | Distribution feedback |

All templates are contained within:

```xml
<RegulatoryReportings>
   <EMT>...</EMT>
   <EET>...</EET>
   <PRIIPS>...</PRIIPS>
   <SolvencyII>...</SolvencyII>
   <KIID>...</KIID>
   <EMIR>...</EMIR>
   <EFT>...</EFT>
</RegulatoryReportings>
```

---

## EMT

**European MiFID Template** — MiFID II product governance and cost disclosure.

### Versions

| Version | Type |
|---------|------|
| V4 | `EMT_V4_FinancialInstrumentType` |
| V4.1 | `EMT_V41_FinancialInstrumentType` |
| V4.2 | `EMT_V42_FinancialInstrumentType` |
| V4.3 | `EMT_V43_FinancialInstrumentType` |

### Data Sections

**General Information**
- Financial instrument identification (ISIN)
- Product type and classification
- Currency

**Target Market**
- Investor type (Retail, Professional, ECP)
- Knowledge & Experience
- Risk tolerance (1-7)
- Time horizon
- Distribution strategy

**Costs Ex-Ante**
- Entry charges
- Exit charges
- Ongoing charges
- Performance fees
- Transaction costs

**Costs Ex-Post**
- Historical cost percentages

### Example

```xml
<EMT>
   <EMTReport>
      <FundOrShareClassIdentifiers>
         <ISIN>AT0000A1Z882</ISIN>
      </FundOrShareClassIdentifiers>
      <EMTData>
         <EMT_V43>
            <DataSetInformation>
               <Version>V4.3</Version>
            </DataSetInformation>
            <TargetMarket>
               <RetailInvestor>Y</RetailInvestor>
               <ProfessionalInvestor>Y</ProfessionalInvestor>
            </TargetMarket>
         </EMT_V43>
      </EMTData>
   </EMTReport>
</EMT>
```

---

## EET

**European ESG Template** — SFDR sustainability disclosure.

### Versions

| Version | Type | Updates |
|---------|------|---------|
| V1 | `EETReportType` | Initial |
| V1.1 | `EETReportType1.1` | SFDR RTS |
| V1.1.1 | `EETReportType1.1.1` | Gas/nuclear |
| V1.1.2 | `EETReportType1.1.2` | Updates |
| V1.1.3 | `EETReportType1.1.3` | ESMA naming |

### SFDR Classification

| Value | Article | Description |
|-------|---------|-------------|
| `6` | Art. 6 | No sustainability claim |
| `8` | Art. 8 | ESG characteristics |
| `9` | Art. 9 | Sustainable objective |

### Data Sections

**Product Classification**
- SFDR product type
- Pre-contractual disclosure flag
- Periodic reporting flag

**ESG Focus**
- Main ESG focus area
- ESG labels and certifications
- Minimum Art. 8/9 allocation

**PAI Indicators**
- GHG emissions
- Carbon footprint
- Fossil fuel exposure
- UNGC violations
- Controversial weapons

**EU Taxonomy**
- Taxonomy-aligned percentage
- Economic activities
- DNSH assessment

### Example

```xml
<EET>
   <EETReport>
      <FundOrShareClassIdentifiers>
         <ISIN>AT0000A1Z882</ISIN>
      </FundOrShareClassIdentifiers>
      <EETData>
         <EET_V1_1_3>
            <SFDR_ProductType>8</SFDR_ProductType>
            <SFDR_PreContractual>Y</SFDR_PreContractual>
            <TaxonomyAligned>
               <Percentage>25.5</Percentage>
            </TaxonomyAligned>
         </EET_V1_1_3>
      </EETData>
   </EETReport>
</EET>
```

---

## PRIIPS

**PRIIPs Key Information Document** — Performance scenarios and costs.

### Versions

| Version | Type |
|---------|------|
| V2 | `PRIIPSType` |
| V2.0 | `PRIIPSType_V20` |

### Performance Scenarios

| Scenario | Description |
|----------|-------------|
| Stress | Extreme adverse conditions |
| Unfavorable | Poor market conditions |
| Moderate | Average conditions |
| Favorable | Good market conditions |

Each scenario includes:
- 1-year returns
- Half RHP returns
- RHP returns

### Risk Indicator

| Value | Risk Level |
|-------|------------|
| 1 | Lowest |
| 2-3 | Low to medium |
| 4 | Medium |
| 5-6 | Medium to high |
| 7 | Highest |

### Cost Elements

- Entry charges
- Exit costs at RHP
- Total costs
- Reduction in Yield (RIY)

### Example

```xml
<PRIIPS>
   <PRIIPSReport>
      <FundOrShareClassIdentifiers>
         <ISIN>AT0000A1Z882</ISIN>
      </FundOrShareClassIdentifiers>
      <PRIIPSData>
         <ReturnScenarios>
            <ReturnStress>
               <OneYear>-35.2</OneYear>
            </ReturnStress>
            <ReturnModerate>
               <OneYear>5.2</OneYear>
            </ReturnModerate>
         </ReturnScenarios>
         <SRRIValue>4</SRRIValue>
         <Costs>
            <RIY_RHP>1.85</RIY_RHP>
         </Costs>
      </PRIIPSData>
   </PRIIPSReport>
</PRIIPS>
```

---

## Solvency II

**Tripartite Template** — Insurance look-through reporting.

### Versions

| Version | Type |
|---------|------|
| V6 | `TripartiteTemplateSolvencyIIType_V6` |
| V7 | `TripartiteTemplateSolvencyIIType_V7` |

### Portfolio Information

| Field | Description |
|-------|-------------|
| TPTVersion | Template version |
| PortfolioID | ISIN or internal ID |
| PortfolioCurrency | ISO 4217 |
| TotalNetAssets | NAV |
| ValuationDate | NAV date |
| CashPercentage | Liquid assets % |
| ModifiedDuration | Duration metric |

### Position Details

- Security codification (ISIN, CUSIP, SEDOL, etc.)
- Issuer information
- NACE code
- Country ISO
- SCR contributions

### Codification Systems

| Code | System |
|------|--------|
| 1 | ISIN |
| 2 | CUSIP |
| 3 | SEDOL |
| 4 | WKN |
| 5 | Bloomberg |
| 6 | BBGID |
| 7 | Reuters RIC |
| 8 | FIGI |
| 10 | LEI |
| 99 | Internal |

### Example

```xml
<SolvencyII>
   <TripartiteTemplate>
      <FundOrShareClassIdentifiers>
         <ISIN>AT0000A1Z882</ISIN>
      </FundOrShareClassIdentifiers>
      <SolvencyIIData>
         <V7>
            <PortfolioInformation>
               <TPTVersion>V7</TPTVersion>
               <PortfolioID>AT0000A1Z882</PortfolioID>
               <TotalNetAssets>250000000</TotalNetAssets>
               <ValuationDate>2025-06-30</ValuationDate>
            </PortfolioInformation>
         </V7>
      </SolvencyIIData>
   </TripartiteTemplate>
</SolvencyII>
```

---

## KIID

**UCITS Key Investor Information Document** — EU Regulation 583/2010.

### Data Sections

**Identification**
- Share class name and ISIN
- Fund manager
- Umbrella/subfund structure

**Objectives & Policy**
- Strategy description
- Asset categories
- Distribution policy
- Benchmark

**Risk & Return**
- Synthetic Risk Indicator (1-7)
- Additional risks

**Charges**
- Entry/exit charges
- Ongoing charges
- Performance fees

**Past Performance**
- 10-year historical returns
- Benchmark comparison

### Example

```xml
<KIID>
   <KIIDReport>
      <FundOrShareClassIdentifiers>
         <ISIN>AT0000A1Z882</ISIN>
      </FundOrShareClassIdentifiers>
      <KIIDData>
         <ShareClassName>ERSTE Responsible Stock Global R01</ShareClassName>
         <RiskReturnProfile>
            <SyntheticRiskIndicator>
               <IndicatorValue>5</IndicatorValue>
            </SyntheticRiskIndicator>
         </RiskReturnProfile>
         <Charges>
            <EntryCharges>5.00</EntryCharges>
            <OngoingCharges>1.75</OngoingCharges>
         </Charges>
      </KIIDData>
   </KIIDReport>
</KIID>
```

---

## EMIR

**Derivatives Trade Reporting** — European Market Infrastructure Regulation.

### Report Sections

**Counterparty Data**
- Counterparty ID (LEI)
- Trading capacity
- Collateralization

**Contract Type**
- Product taxonomy
- Underlying asset
- Currencies

**Transaction Details**
- Trade ID
- Execution venue
- Price/rate
- Notional amount

**Clearing**
- Clearing obligation
- CCP identification
- Intragroup flag

**Asset-Specific**
- Interest rates (fixed/floating)
- FX (currencies, rates)
- Options (type, strike)
- Commodities (delivery)

### Example

```xml
<EMIR>
   <EMIRReport>
      <CounterpartyData>
         <CounterpartyID>PQOH26KWDF7CG10L6792</CounterpartyID>
         <CounterpartySide>B</CounterpartySide>
      </CounterpartyData>
      <TransactionDetails>
         <TradeID>TRADE_2025_001</TradeID>
         <ExecutionVenue>XEUR</ExecutionVenue>
         <NotionalAmount>10000000</NotionalAmount>
      </TransactionDetails>
   </EMIRReport>
</EMIR>
```

---

## EFT

**European Feedback Template** — Distribution deviation reporting.

### Perspectives

| Value | Description |
|-------|-------------|
| `M` | Manufacturer |
| `D` | Distributor |
| `B` | Both |

### Data Sections

**Report Information**
- Reporting period
- Reference target market

**Entity Information**
- Submitter details
- Position in distribution chain

**Deviation Reports**
- Sales outside target market
- Knowledge/experience mismatches
- Distribution strategy widening

### Example

```xml
<EFT>
   <EFTReport>
      <FundOrShareClassIdentifiers>
         <ISIN>AT0000A1Z882</ISIN>
      </FundOrShareClassIdentifiers>
      <EFTData>
         <Version>V1</Version>
         <PeriodStartDate>2025-01-01</PeriodStartDate>
         <PeriodEndDate>2025-06-30</PeriodEndDate>
         <ReferenceTargetMarket>M</ReferenceTargetMarket>
         <FinancialInstrumentInformation>
            <TotalNumberOfTransactions>1250</TotalNumberOfTransactions>
         </FinancialInstrumentInformation>
      </EFTData>
   </EFTReport>
</EFT>
```

---

## File Reference

| Module | Schema File |
|--------|-------------|
| EMT | `FundsXML4_RegulatoryReporting_EMT.xsd` |
| EET | `FundsXML4_RegulatoryReporting_EET.xsd` |
| PRIIPS | `FundsXML4_RegulatoryReporting_PRIIPS.xsd` |
| Solvency II | `FundsXML4_RegulatoryReporting_SolvencyII.xsd` |
| KIID | `FundsXML4_RegulatoryReporting_KIID.xsd` |
| EMIR | `FundsXML4_RegulatoryReporting_EMIR.xsd` |
| EFT | `FundsXML4_RegulatoryReporting_EFT.xsd` |
