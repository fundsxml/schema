
# FundsXML



## The pan-European format
Aside from its technical advantages and its easy handling, FundsXML is becoming the pan-European format, allowing B2B and B2C activity in the mutual fund sector.  
The development of this standard was pushed by various companies to integrate numerous working processes in the sector, enclosing the foundation of a fund and the system of registration as well as the electronic data exchange, beyond the borders of companies and countries.

If you have questions about FundsXML don't hesitate to contact one of these persons: 

|        | Country | Company | Contact    |
| :----: | :-----: | :------ | :--------- |
| ![Flag of Austria](doc/images/flag_austria.png)| AT      | Erste Asset Management GmbH | [Karl Kauc](mailto:karl.kauc@erste.am.com)       |
| ![Flag of Austria](doc/images/flag_austria.png)| AT | Amundi Austria GmbH | [Peter Raffelsberger](mailto:peter.raffelsberger@amundi.com)  |
| ![Flag of Austria](doc/images/flag_germany.png)| DE | BVI Bundesverband   Investment und Asset Management e. V | [Thomas Koop](mailto:Thomas.Koop@bvi.de) |
| ![Flag of Austria](doc/images/flag_germany.png)| DE | DWS Investment GmbH | [Kai Schrumpf](mailto:kai.schrumpf@dws.de) |
| ![Flag of Austria](doc/images/flag_denmark.png)| DK | FundConnect A/S | [Daniel Heskia](mailto:dh@fundconnect.com) |
| ![Flag of Austria](doc/images/flag_luxembourg.png)| LU | KNEIP | [Fabrice Puziak](mailto:fabrice.puziak@kneip.com) |
| ![Flag of Austria](doc/images/flag_netherlands.png)| NL | Robeco | [Andy Chan](mailto:a.chan@robeco.nl) | 
| ![Flag of Austria](doc/images/flag_france.png)| FR | Efeso | [Pierre Maugery-Pons](mailto:pierre.maugery-pons@efeso.com) |




## History of FundsXML

lorem ipsum...


## FundsXML is a powerful XML standard format for easy exchange of fund related data between
- Asset Managers
- Clients
- Distributors
- Custodians
- Authorities
- Vendors
- Advisors



## XML vs CSV

- Clear file structure and formatting (number, dates, ...)
- Native support for nested loops, optional fields and sections
- Definition of fields and tree structure via XML-Schema
- Built in documentation in multiple languages
- Automatic validation of file structure and field content against XML-Schema

You can find a lot of discussions about XML vs CSV on the internet. E.g. on [Stackoverflow](https://stackoverflow.com/questions/1820129/when-and-why-is-xml-preferable-to-csv).



## FundsXML spectrum of Data
With FundXML it is possible to transport all Fund related Data within a single file.

| Topic | Description | FundsXML | 
| ----- | ----------- | :------: |
| File Metadata | Creation Date, Data Supplier, File operation (delete, update, create), ... | ![fully supported](doc/images/ok.png) | 
| Asset manager Data | Asset Manager data (Address, LEI, ...), Statistic Data (AuM, etc.), Parent Company, Fund Manager, ... | ![fully supported](doc/images/ok.png) | 
| Fund Static Data | Fund name, Identifiers (LEI, Bloomberg, ...), inception date, closing date, Benchmark used, Custodian information, regulatory framework (UCITS, AIF,..), ...  | ![fully supported](doc/images/ok.png) | 
| Fund Dynamic Data | Funds volumes (M2M and additional calculation methods), ...  | ![fully supported](doc/images/ok.png) | 
| Shareclass Information | Identifiers (ISIN, Bloomberg, Sedol,...), Shareclass Volume, shares outstanding, Portfolio data, ... | ![fully supported](doc/images/ok.png) | 
| Segment Data | internal Fund Segments, Portfolio Data, Transactions, Static Data,... | ![fully supported](doc/images/ok.png) | 
| Holdings | Detailed position information | ![fully supported](doc/images/ok.png) |  
| Transactions | Detailed transaction information | ![fully supported](doc/images/ok.png) |  
| Benchmark Data | benchmark values, composition, index constituents, ... | ![fully supported](doc/images/ok.png) | 
| Fund Documents | regulatory documents (KIID, prospectus, anual reports,...) and custom reports (factsheets, customer reporting, ...) in all kind of formats (either as link or direct embedded in the FundsXML file), ... | ![fully supported](doc/images/ok.png) | 
| Asset Master Data | detailed information about assets including all kind of derivatives | ![fully supported](doc/images/ok.png) | 
| Country Specific Data | section for country specific data | ![fully supported](doc/images/ok.png) | 
| Regulatroy Reportings | all kind of regulatory reportings: AIFMD, MIFID, PRIPS, TPT, EMT, ... | ![fully supported](doc/images/ok.png) | 
| Custom Data | flexible to transport additional data between sender and reciever | ![fully supported](doc/images/ok.png) | 




## Documentation
Documentation can be found on the [official FundsXML Website](http://www.fundsxml.org/documentation/) or on Github Pages. 


## Where can I get the latest release?
Latest official relese can be downloaded via the [fundsxml.org](http://www.fundsxml.org/schema-definitions/) website or via [Github](https://github.com/fundsxml/schema/releases).


## Contributing
If you want to become a Associate Member of the FundsXML organisation or active participate in one of the working groups contact the "FundsXML Standards Committee" contact the Secretary [Thomas Koop](mailto:thomas.koop@bvi.de).  
If you have found any bugs or enhance some documentaion - we also accept Pull Requests via GitHub.


## License
The organisation FundsXML.org administrates the XML patterns with the aim to establish these patterns as a unified standard. For this reason each company and single person shall be allowed to download and use these file formats and the documentations and files going with them from the FundsXML website. This is why FundsXML.org decided to put all patterns, files and documentations going with them under the [Mozilla Public Licence (MPL)](https://www.mozilla.org/en-US/MPL/).



## Additional Resources

> 
> * [FundsXML – CSV converter](http://www.xml-tools.net/fundsxml/fundsxml-csv-converter.html)
> * [Online FundsXML Schema Viewer](http://www.xml-tools.net/fundsxml/schemaviewer.html)
> * [Online Portfolio Analysis via FundsXML data](http://www.xml-tools.net/fundsxml/portfolio-analysis.html)
> * [FundsXML-EMT-Converter](https://github.com/karlkauc/FundsXML-EMT-Converter)
> * [Online FundsXML Format and Quality Check](http://www.xml-tools.net/fundsxml/fundsxml4-analysis.html)