# vaadin-excel-exporter for Vaadin 7

vaadin-excel-exporter is a utility add-on for Vaadin 7. It is a single point solution providing consistent and configurable support for 
exporting data of Vaadin Table, Tree Table and Grid in Excel format.

## Why do we need this addon?

1) Exporting the screen data is a frequent task required in most of the applications. There are several 
helper technologies such as POI which help us in bringing the Export feature in place but a developer 
needs to take it up everytime he wants to implement.
2) Vaadin providing so many components for representation of data such as Table, Tree Table and Grid, 
which again raises a need for a utility that is easy and configurable to be used with varied component 
wihtout the pain of writing the logic to generate the Excel.
3) A consistent excel format throughout the application which would enhance user experience.
4) Can be integrated with any Vaadin application with just few lines of configuration code.

## What dependencies are required?
JDK 1.7 or above
POI 3.9
POI-OOXML 3.9
Apache Commons IO library (http://commons.apache.org/io/) v2.2
net.karneim.pojobuilder 3.4.0 and above

## Online demo

Try the add-on demo at URL

## Download release

Official releases of this add-on are available at Vaadin Directory. For Maven instructions, download and reviews, go to http://vaadin.com/addon/vaadin-excel-exporter

## Building and running demo

git clone https://github.com/bonprix/vaadin-excel-exporter
mvn clean install
cd demo
mvn jetty:run

To see the demo, navigate to http://localhost:8080/

## Development with Eclipse IDE

For further development of this add-on, the following tool-chain is recommended:
- Eclipse IDE
- m2e wtp plug-in (install it from Eclipse Marketplace)
- Vaadin Eclipse plug-in (install it from Eclipse Marketplace)
- JRebel Eclipse plug-in (install it from Eclipse Marketplace)
- Chrome browser

### Importing project

Choose File > Import... > Existing Maven Projects

Note that Eclipse may give "Plugin execution not covered by lifecycle configuration" errors for pom.xml. Use "Permanently mark goal resources in pom.xml as ignored in Eclipse build" quick-fix to mark these errors as permanently ignored in your project. Do not worry, the project still works fine. 

### Debugging server-side

If you have not already compiled the widgetset, do it now by running vaadin:install Maven target for vaadin-excel-exporter-root project.

If you have a JRebel license, it makes on the fly code changes faster. Just add JRebel nature to your vaadin-excel-exporter-demo project by clicking project with right mouse button and choosing JRebel > Add JRebel Nature

To debug project and make code modifications on the fly in the server-side, right-click the vaadin-excel-exporter-demo project and choose Debug As > Debug on Server. Navigate to http://localhost:8080/vaadin-excel-exporter-demo/ to see the application.

### Debugging client-side

Debugging client side code in the vaadin-excel-exporter-demo project:
  - run "mvn vaadin:run-codeserver" on a separate console while the application is running
  - activate Super Dev Mode in the debug window of the application or by adding ?superdevmode to the URL
  - You can access Java-sources and set breakpoints inside Chrome if you enable source maps from inspector settings.
 
## Release notes


### Version 1.0.0-SNAPSHOT
- Initial release

## Roadmap

Upcoming releases:
- Adding a total row at the bottom for components with optional granular configuration of the rows
- Specifying a row header which can allow horizontal data as well.
- Reflecting some column generator logic as well in the Excel. For Example suffixes cm, kg, $ etc...
- Export of selected records wherever applicable
- Optional Footer Section
- Legend for better understanding

## Issue tracking

The issues for this add-on are tracked on its github.com page. All bug reports and feature requests are appreciated. 

## Contributions

Contributions are welcome, but there are no guarantees that they are accepted as such. Process for contributing is the following:
- Fork this project
- Create an issue to this project about the contribution (bug or feature) if there is no such issue about it already. Try to keep the scope minimal.
- Develop and test the fix or functionality carefully. Only include minimum amount of code needed to fix the issue.
- Refer to the fixed issue in commit
- Send a pull request for the original project
- Comment on the original issue that you have implemented a fix for it

## License & Author

Add-on is distributed under Apache License 2.0. For license terms, see LICENSE.txt.

vaadin-excel-exporter is written by Kartik Suba @ Direction Software Solutions, India.
For Client: Bonprix Handelsgesellschaft mbH

# Developer Guide

## Getting started

Here is a simple example on how to try out the add-on component:

/* Configuring Components */
ExportExcelComponentConfiguration componentConfig1 = new ExportExcelComponentConfigurationBuilder().withTable(this.tableWithBeanItemContainer) //Your Table or component goes here
                                                                                                           .withVisibleProperties(this.tableWithBeanItemContainer.getVisibleColumns())
                                                                                                           .withColumnHeaderKeys(this.tableWithBeanItemContainer.getColumnHeaders()) 
                                                                                                           .build();
/* Configuring Sheets */
ArrayList<ExportExcelComponentConfiguration> componentList1 = new ArrayList<ExportExcelComponentConfiguration>();
componentList1.add(componentConfig1);

ExportExcelSheetConfiguration sheetConfig1 = new ExportExcelSheetConfigurationBuilder().withReportTitle("Excel Report")
                                                                                               .withSheetName("Excel Report")
                                                                                               .withComponentConfigs(componentList1)
                                                                                               .withIsHeaderSectionRequired(Boolean.TRUE)
                                                                                               .build();
                                             
/* Configuring Excel */
ArrayList<ExportExcelSheetConfiguration> sheetList = new ArrayList<ExportExcelSheetConfiguration>();
sheetList.add(sheetConfig1);

ExportExcelConfiguration config1 = new ExportExcelConfigurationBuilder().withGeneratedBy("Kartik Suba")
                                                                                .withSheetConfigs(sheetList)
                                                                                .build();

ExportToExcelUtility<DataModel> exportToExcelUtility = new ExportToExcelUtility<DataModel>(this.tableWithBeanItemContainer.getUI(), config1, DataModel.class);
exportToExcelUtility.setSourceUI(UI.getCurrent());
exportToExcelUtility.setResultantExportType(ExportType.XLSX);
exportToExcelUtility.export();

For a more comprehensive example, see src/test/java/org/vaadin/template/demo/DemoUI.java

## Features

It is highly configurable and provides configuration at three levels namely File Level, Sheet Level, Component Level.

### For each file you can configure
- List of Sheets
- Export File Name
- Generated By
- Export Type (xlsx or xls)

### For each sheet you can configure
- Sheet Name
- Report Title Row
- Generated By Row
- List of Components inside the sheet
- FieldGroup - for showing filters selected on the screen for the selected data
- Additional Header Info - HashMap for custom data (Key Value Pair)
- Date Format
- Number Formats
- XSSFCellStyle for each content section of the screen

Note that the Number formats by itself manages the thousand separator and decimal point 
for different locales. For Eg: For English the decimal point is (.) and thousand seperator is (,), but for 
German locale it is the reverse. But the code handles it by its own.

### For each component you can configure
- Component - Tree table or Grid or Table
- It supports all kinds of containers - Indexed, BeanItem, Hierarchical
- Visible properties
- Properties requiring date formatting
- Properties requiring Float formatting
- Properties requiring Integer formatting
- Column Header Texts
- Component Header and Content Styles
- Column Freeze and Header Freeze feature

However, if none of these are specified, it would generate the Excel with default values and styles.