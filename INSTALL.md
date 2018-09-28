ODS+ PerfKPI Dashboard
=======

Welcome to the PerfKPI dashboard replication guide. 
The main purpose is to explain how to replicate this dashboard easily, based on the generic example and its generic data source.

You'll then first find the portal customization code, the pages code, and all the assets.
To replicate the same application on your own portal, you'll need:
 * to add the portal customization code to your portal (fully or partially)
 * to upload the assets and define dataset themes
 * to create the pages
 * to create the associated datasets

Please find all the links and resources below.

Direct links to the demo are available at the end of this guide. 


## Get the code and assets

You can simply download the assets, portal header and stylesheet, and apply them to your portal.

**NB**: if your portal already have a customization, do not simply replace the stylesheet and header, you'll probably need to merge these files. Advanced HTML/CSS knowledge might be required. Ask your OpenDataSoft CSM contact for more information and help.



### Portal look and feel

* [Header menu (HTML)](./resources/web/perfkpi-header.html)
* [Portal stylesheet (CSS)](./resources/web/perfkpi.css)

### Pages

* [PerfKPI Home page (HTML)](./resources/web/home.html)
* [PerfKPI Home page (CSS)](./resources/web/home.css)
* [PerfKPI Generic page](./resources/web/generic.html)

The PerfKPI Dashboard pages contains links to the different pages, you'll probably need to change the URL and references to adapt your page to your use case.

### Assets

* [PerfKPI Assets (zip)](./resources/assets/perfkpi-assets.zip)

The assets archive contains pictograms, logos, and background images. Do not hesitate to replace them with your own.
It may also require some CSS changes if the dimensions of your assets vary from these ones.

Pictograms must be uploaded as dataset themes.
The list of used themes on the home pages:
* Energy
* Environment
* Citizen
* Transport

And for the header 
* ODS Village

Feel free to replace the header with your logo obviously, but also to use your own pictogram for SmartCity theme.
You can also use ours!  

## Get the data

The generic page is designed to work with the data schema described below. Another schema will require modifications to the page code. 
If the column names are not the same, simply search and replace the identifier of the column in the page.
If the data is more complex, or the datavisualization is more advanced, you can also replace the charts by removing the default one and pasting the code of your own visualization. 

* [Generic data example](./resources/data/generic.csv)


#### Data Schema :

- **Date** 
  - _id_: date
  - _type_: calendar date yyyy/mm/dd
  - _purpose_: contains the date of the metric
  - _sample value_: 2018/01/11
- **Value**
  - _id_: value
  - _type_: numerical
  - _purpose_: contains the metric, any type of value as long as it's a numerical value
  - _note_: must not contains units
  - _sample value_: 17'001.99
- **Category**
  - _id_: category
  - _type_: text
  - _purpose_: describe or define a metric category
  - _note_: this column is used in the timeserie, splitting the value visualization.
  this column is optional or all metric can have the same category but therefore is useless.
  - _sample value_: Expense, Budget, Income
  

### Next steps

The next steps will then be to:

1. Get your data and adapt it to the right schema
1. Create your dataset and publish it on your OpenDataSoft platform
    1. be sure that *Date* column is the correct date type (it enables timeserie analysis)
    1. set *Category* as a facet (it enables filtering and analysis on this field)
    1. be sure that *Value* is an Integer or a Decimal (it allows SUM, SVG, or any other mathematical operation)
1. Create a page and copy/paste the generic page code into it
    1. Adapt the page code if the column identifiers of the dataset don't match
1. Change the page's editorial content
    1. Title
    1. Section/bloc title
    1. Navigation links and labels
1. Save, and have a first look!

If a datavisualization or a KPI is not working (wrong value OR no value displayed), you can inspect the code of your page with your browser's debugging tools.
You'll see the error in the console or network tab. Most of the time, you'll have an API query failing because of wrong parameters; double clicking on the URL will open the API call in a new tab, and you'll then be able to see the API return message. 

Most common errors are due to wrong column format, if there is no facet, field not sortable etc...

For more advanced analysis or help, do not hesitate to contact [OpenDataSoft support](mailto:support@opendatasoft.com) for assistance.


Thank you, and do not hesitate to share your reuse of this application on twitter (@opendatasoft) or email; We will be very pleased to see your work and see that this application was useful for you!

### Access the live demo !

* [PerfKPI Dashboard](https://perfkpi-odsplus.opendatasoft.com/)
* [Generic KPI](https://perfkpi-odsplus.opendatasoft.com/pages/generic/)
* [Generic data source](https://perfkpi-odsplus.opendatasoft.com/explore/dataset/kpi-generic/table/)


### Page templates

To go beyond you can also replace or add other blocks and sections to integrate more KPIs and data to your dashboard.
To do so, you can go to the [generic template page](https://perfkpi-odsplus.opendatasoft.com/pages/template/) that propose several variations of sections that can be reused.

The steps are:

1. Get the code of the section you would like by looking into the [page code here](./resources/web/templates.html)
1. Add it to your existing KPI page, bellow the existing sections
1. replace the placeholders comments by your own text and data visualization
1. Save and see the result

However this is a more complex manipulation, do not hesitate to contact your designated Customer Success Manager to assist you.
 