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

### Changed

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
