# FundsXML
## The pan-European format

FundsXML is an open, free, and internationally recognized XML-based standard for the exchange of data in the fund industry. It was developed to standardize and simplify communication and data transfer between various participants such as asset management companies, custodian banks, distributors, regulatory authorities, and data vendors.


## Main Purpose and Areas of Application

The main purpose of FundsXML is to automate and increase the efficiency of data exchange in the fund business. By providing a uniform structure and clear definitions for data fields, errors are reduced and processes are accelerated.

**Areas of application include the exchange of:**

* **Fund Master Data:** General information such as fund name, identification numbers (ISIN, WKN), fund type, fund manager, etc.

* **Transaction Data:** Information on the buying and selling of securities within the fund.

* **Portfolio and Holdings Data:** A detailed breakdown of the positions held in the fund's assets.

* **Share Class Data:** Information on the various share classes of a fund, including their Net Asset Values (NAV), distributions, and performance.

* **Regulatory Data:** Support for various regulatory requirements such as MiFID II, PRIIPs, Solvency II, and country-specific reporting.



## Structure and Advantages

FundsXML is based on an XML Schema (XSD - XML Schema Definition), which precisely prescribes the hierarchical structure and data types for every piece of information. This offers decisive advantages over less specific formats like CSV.

### Key Advantages:

* **Clear Structure:** The XML schema defines exactly which data fields are expected, whether they are optional, and how they relate to each other. This prevents misunderstandings and facilitates machine processing.

* **Automated Validation:** XML files can be automatically validated against the schema. This ensures that the data structure and field content (e.g., date or number formats) are correct before they are processed further.

* **Support for Complex Data:** Unlike flat structures such as CSV, XML can easily represent nested and complex data structures, which is essential for the detailed representation of fund data.

* **International Standardization:** As a European-wide standard, FundsXML promotes interoperability between different systems and market participants across national borders.

* **Free and Open:** The standard is maintained by the [fundsxml.org](https://urldefense.com/v3/__http:/fundsxml.org__;!!OMGRPR5eiCE28w!uS0SLK1zVXJ05t-WicHYGLGBpQRXkrwE7BANjCEmuESqkzqUlCs9qawSjn9cYXY-AF6kZ-0cUopLSA6geRYA$) initiative and is freely available under an open license (Mozilla Public License).



## Example Data Structure (Simplified)

A FundsXML file has a hierarchical tree structure. Here is a conceptual example of how fund information might be structured:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<FundsXML4 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="https://github.com/fundsxml/schema/releases/download/4.2.9/FundsXML.xsd">
	<ControlData>
		<UniqueDocumentID>ID_001</UniqueDocumentID>
		<DocumentGenerated>2025-07-04T00:00:00Z</DocumentGenerated>
		<ContentDate>2025-06-30</ContentDate>
		<DataSupplier>
			<SystemCountry>AT</SystemCountry>
			<Short>AM</Short>
			<Name>Asset Manager</Name>
			<Type>Asset Manager</Type>
		</DataSupplier>
	</ControlData>
	<Funds>
		<Fund>
			<Identifiers><LEI>PQOH26KWDF7CG10L6792</LEI>
			</Identifiers>
			<Names>
				<OfficialName>Fund ONE</OfficialName>
			</Names>
			<Currency>EUR</Currency>
			<SingleFundFlag>true</SingleFundFlag>
			<FundDynamicData><Portfolios>
				<Portfolio>
					<NavDate>2025-06-30</NavDate>
					<Positions>
						<Position>
							<UniqueID>POS_ID_01</UniqueID>
							<TotalValue>
								<Amount ccy="EUR">100</Amount>
							</TotalValue>
							<Equity>
								<Units>50</Units>
								<Price>
									<Amount ccy="EUR">20</Amount>
								</Price>
							</Equity>
						</Position>
					</Positions>
				</Portfolio>
			</Portfolios>
			</FundDynamicData>
		</Fund>
	</Funds>
	<AssetMasterData>
		<Asset>
			<UniqueID>POS_ID_01</UniqueID>
			<Currency>EUR</Currency>
			<Name>Equity One</Name>
			<AssetType>EQ</AssetType>
		</Asset>
	</AssetMasterData>
</FundsXML4>
```
