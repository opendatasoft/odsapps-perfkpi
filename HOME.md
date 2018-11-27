ODS Apps PerfKPI - Home page guide
=======

PerfKPI Home page contains four themes / strategies. 

Each theme, as a column, contains two metrics, highlighting the progress, achievement, or evolution of the city strategy.

Different type of metrics are represented:
- Average or sum of the indicator (aggregation)
- Current year/month - Past year/month comparison (evolution)
- Last value of the indicator (last value)

This guide will then propose the documented and commented code of each available metrics.


### Dynamic goals and metrics


Each metric are green or red, crossed or checked, depending on the goal achievement or failure.

This behavior is totally dynamic and rely on a [Goal dataset](https://perfkpi-odsplus.opendatasoft.com/explore/dataset/kpi-goals/) that contains the title, theme, goal and goal unit of each metric.

As the page is generated dynamically from this dataset, the end user can simply modify the Excel spreadsheet or CSV that is sourcing the dataset to rename the metric, or simply change the goal.
Find the [goal.csv here](./resources/data/goals.csv)

*If you want to simplify your code and have hardcoded goals and metric names (the content is directly in the page), you can directly jump to the next section [KPIs](./HOME.md#kpis) and ignore the dynamic goal management*


This mechanism is shared into 2 parts. The first one is the goal data structure loading, based on the goal dataset. The second is the usage of this structure containing all the needed values in the page.

#### Goal loading 

```html
<div class="perfkpi-page" ng-init="myvar = {}; goals = {}">
<!-- LOAD AND BUILD GOALS STRUCTURE
    For each 'Theme', get 2 KPIs (1 and 2), and for each KPI get the value, the unit and the title
    goals['Energy']['1']['value'] -> get the goal value of the first Energy KPI
-->
    <ods-dataset-context context="kpigoals" kpigoals-dataset="kpi-goals">
        <div ods-facet-results="themes" 
             ods-facet-results-context="kpigoals"
             ods-facet-results-facet-name="theme"
             ng-repeat="theme in themes">
            {{ goals[theme.name] = {'1': {'value': 0, 'unit': '', 'title':''},'2': {'value': 0, 'unit': '', 'title':''} } ; "" }}
        </div>
        <div ng-repeat="item in items" ods-results="items" ods-results-context="kpigoals">
            {{ 
            goals[item.fields.theme][item.fields.id].value = item.fields.goal ;
            goals[item.fields.theme][item.fields.id].unit = item.fields.unit ;
            goals[item.fields.theme][item.fields.id].title = item.fields.title ;
            ""
            }}
        </div>
    </ods-dataset-context>
```

`goals` is the data structure that will load all KPI themes, for each theme, KPI names, goal and unit.
Once loaded, the data structure looks like this:

```json
{
    "Citizen services": {
        "1": {
            "title": "of calls answered w/in 1 min",
            "unit": "%",
            "value": 85
        },
        "2": {
            "title": "open datasets",
            "unit": "datasets",
            "value": 1500
        }
    },
    "Energy": {
        "1": {
            "title": "MWh produced by solar PV",
            "value": 50
        },
        "2": {
            "title": "solar PV production",
            "unit": "%",
            "value": 10
        }
    },
    "Environment": {
        "1": {
            "title": "hectares of new green spaces",
            "value": 3
        },
        "2": {
            "title": "of waste recycled",
            "unit": "%",
            "value": 17
        }
    },
    "Transport": {
        "1": {
            "title": "trains on time",
            "unit": "%",
            "value": 90
        },
        "2": {
            "title": "shared electric cars",
            "value": 3000
        }
    }
}
```

#### Goal usage

It will then be easy to get corresponding value when needed.

For example, in the transport column, the second KPI will provide it's information like this :

```angularjs
Title: {{ goals['Transport']['2'].title }}
Goal: {{ goals['Transport']['2'].value }} {{ goals['Transport']['2'].unit }}
``` 

## KPIs 

### a KPI template

All KPIs looks like this template

```html
<a href="<link to the KPI page>"
   class="perfkpi-card"
   
   ng-class="{'perfkpi-card--green perfkpi-card--check-white': agg >= goals[kpitheme]['2'].value, 'perfkpi-card--red perfkpi-card--times-white': agg < goals[kpitheme]['2'].value}"
   
   ods-aggregation-widget>
    
    <p>
        <span class="perfkpi-bold perfkpi-white">
            AGGREGATION VALUE
        </span>
        KPI TITLE
    </p>
    
    <p class="perfkpi-small perfkpi-italic">DATA DATE</p>
    <p class="perfkpi-small perfkpi-italic perfkpi-goal"><i class="fa fa-bullseye" aria-hidden="true"></i> GOAL VALUE - GOAL UNIT
    </p>
    <span class="perfkpi-marker"></span>
</a>
```

#### Template explanation
 
`ng-class` attribute dynamically apply CSS class if the goal is achieved or not, and therefore display the KPI green or red, crossed or checked.
`ods-aggregation-widget` compute the aggregation on the data and get the result into a variable used in the HTML code

#### Parent block and `kpitheme`

Please also note that `kpitheme` variable is used for 'Transport' 'Energy' etc... this variable is set in the parent HTML tag like this:
```html
<!-- Transport -->
<div ... ng-init="kpitheme = 'Transport'">
    <!-- KPIs here -->
</div>
```


### Aggregation KPI - counting, summing or averaging a value

The KPI counts the number of datasets in the city OpenData portal. [The source dataset contains the list of dataset](https://perfkpi-odsplus.opendatasoft.com/explore/dataset/open-data-ods-village/).

The KPI then is a simple counting operation.

```html
<a href="/pages/citizen/#/#open"
   class="perfkpi-card"
   ng-class="{'perfkpi-card--green perfkpi-card--check-white': agg >= goals[kpitheme]['2'].value, 'perfkpi-card--red perfkpi-card--times-white': agg < goals[kpitheme]['2'].value}"
   ods-aggregation="agg"
   ods-aggregation-context="opendata"
   ods-aggregation-function="COUNT"
   ods-aggregation-expression="title">
    <p>
        <span class="perfkpi-bold perfkpi-white">
            {{ agg | number }}
        </span>
        {{ goals[kpitheme]['2'].title }}
    </p>
    <p class="perfkpi-small perfkpi-italic">{{ opendata.dataset.metas.modified | date : 'MMMM yyyy' }}</p>
    <p class="perfkpi-small perfkpi-italic perfkpi-goal"><i class="fa fa-bullseye"
                                                            aria-hidden="true"> </i> {{ goals[kpitheme]['2'].value | number }} {{ goals[kpitheme]['2'].unit }}
    </p>
    <span class="perfkpi-marker"></span>
</a>
```

#### Specificity

`agg` is the result of the counting operation and is then used to display the result.
`opendata.dataset.metas.modified` is the modified date of the dataset.
As there is no date data in the dataset, the dataset modification date is used instead. See other KPI example to see how to use the record date.


### Double aggregation KPI - displaying two values

This KPI is more or less the same that previously, but display two values.
In this case, two aggregation widget are used to get two variables `rate` and `calls`

```html
<!-- KPI 1 - first row -->
                        <div ods-results="lastkpi6"
                             ods-results-context="kpi6lastevo"
                             ods-results-max="1">
                            <!-- Get last date of kpi6lastevo -->
                            {{ lastdate_kpi6lastevo = lastkpi6[0].fields.year_month ; "" }}
                            {{ kpi6lastevo.parameters['refine.year_month'] = (lastdate_kpi6lastevo | moment : 'YYYY-MM-01' | date : 'yyyy/MM') ; "" }}

                            <a href="/pages/citizen/#/#calls"
                               class="perfkpi-card"
                               ng-if="lastkpi6"
                               ng-class="{'perfkpi-card--green perfkpi-card--check-white': rate >= goals[kpitheme]['1'].value, 'perfkpi-card--red perfkpi-card--times-white': rate < goals[kpitheme]['1'].value}"
                               ods-aggregation="rate"
                               ods-aggregation-context="kpi6lastevo"
                               ods-aggregation-function="SUM"
                               ods-aggregation-expression="answered_call_rate">
                                <p ods-aggregation="calls"
                                   ods-aggregation-context="kpi6lastevo"
                                   ods-aggregation-function="SUM"
                                   ods-aggregation-expression="answered_call">
                                    <span class="perfkpi-bold perfkpi-white">{{ rate | number }}%</span>
                                    {{ goals[kpitheme]['1'].title }} (<span
                                                                            class="perfkpi-bold perfkpi-white perfkpi-small">{{ calls | number }}</span> calls)
                                </p>
                                <p class="perfkpi-small perfkpi-italic">{{ lastdate_kpi6lastevo | moment : 'YYYY-MM-01' | date : 'MMMM yyyy' }}</p>
                                <p class="perfkpi-small perfkpi-italic perfkpi-goal"><i class="fa fa-bullseye"
                                                                                        aria-hidden="true"> </i> {{ goals[kpitheme]['1'].value | number }} {{ goals[kpitheme]['1'].unit }} </p>
                                <span class="perfkpi-marker"></span>
                            </a>
                        </div>
```

#### Specificity

```html
<div ods-results="lastkpi6"
     ods-results-context="kpi6lastevo"
     ods-results-max="1">
     <!-- Get last date of kpi6lastevo -->
     {{ lastdate_kpi6lastevo = lastkpi6[0].fields.year_month ; "" }}
     {{ kpi6lastevo.parameters['refine.year_month'] = (lastdate_kpi6lastevo | moment : 'YYYY-MM-01' | date : 'yyyy/MM') ; "" }}
```

`ods-results` get records in a dataset. 
Here, `ods-results-max="1"` means that only 1 record is fetched.
 
As the dataset is sorted, thanks to the initialisation parameter:
```html
<ods-dataset-context context="kpi6lastevo"
                     kpi6lastevo-dataset="kpi-6-citizen-engagement"
                     kpi6lastevo-parameters="{'sort':'year_month'}"
```
It then means that `ods-results` get the last (more recent) value of the dataset.

`lastkpi6` the record list fetched

`lastkpi6[0]` the first item of the list

`lastdate_kpi6lastevo = lastkpi6[0].fields.year_month` the value of the date field

`lastdate_kpi6lastevo | moment : 'YYYY-MM-01' | date : 'yyyy/MM'` the date is parsed, then parsed as a year/month format `yyyy/MM`. This is the expected format for the search API.

Now that we have the last date of the dataset, we can filter the dataset to only focus on this period:

`kpi6lastevo.parameters['refine.year_month'] = (lastdate_kpi6lastevo | moment : 'YYYY-MM-01' | date : 'yyyy/MM')` set the parameters to the right period

Now the dataset is filtered, all aggregation computed on the dataset will focus on this specific period of time.
To finish, it can be pretty printed with a Month Year format :

`lastdate_kpi6lastevo | moment : 'YYYY-MM-01' | date : 'MMMM yyyy'`  


### Data KPI over a range of year

The KPI aggregation is similar, but the date processing is different.
Here, we don't get the last record of the dataset and therefore the most recent.

We use the `ods-facet-results` widget to get the entire range of date of the dataset. The output of the widget is the list of all available years. 
Getting the first and last element of the list gives the boundaries.

These two boundaries are stored in the `year` structure, and then used in the KPI

```html
<!-- KPI 1 - first row -->
<div ods-facet-results="items" 
     ods-facet-results-context="kpi1lastevo" 
     ods-facet-results-facet-name="year"
     ng-init="years = {'start':0, 'end':0}">
    {{ years.start = items[0].name ; 
    years.end = items[items.length-1].name ; "" }}

    <a href="/pages/environment/#/#green-areas"
       class="perfkpi-card"
       ng-init="items"
       ng-class="{'perfkpi-card--green perfkpi-card--check-white': agg >= goals[kpitheme]['1'].value, 'perfkpi-card--red perfkpi-card--times-white': agg < goals[kpitheme]['1'].value}"
       ods-aggregation="agg"
       ods-aggregation-context="kpi1lastevo"
       ods-aggregation-function="SUM"
       ods-aggregation-expression="new_green_areas_ha">
        <p>
            <span class="perfkpi-bold perfkpi-white">
                {{ agg | number:1 }}
            </span>
            {{ goals[kpitheme]['1'].title }}
        </p>
        <p class="perfkpi-italic perfkpi-small">{{ years.start }} to {{ years.end }}</p>
        <p class="perfkpi-small perfkpi-italic perfkpi-goal"><i class="fa fa-bullseye"
                                                                aria-hidden="true"> </i> {{ goals[kpitheme]['1'].value | number }} {{ goals[kpitheme]['1'].unit }}</p>
        <span class="perfkpi-marker"></span>
    </a>
</div>
```



### Year N/N-1 comparison and different syntax KPI

This is a more complex KPI with a different way of computing aggregation beforehand and then using result variables in the KPI blocks

```html
<!-- ENERGY -->
<div class="col-md-3 col-sm-6" ng-if="true" ng-init="kpitheme = 'Energy'">
    <a href="/pages/energy" class="perfkpi-kpi-section-header">
        <ods-theme-picto theme="Energy"></ods-theme-picto>
        <h3 class="perfkpi-kpi-section-title">
            {{ kpitheme }}
        </h3>
    </a>
```

First we get the last date, and last value. We store it.
Then we format the date, substract 1 year, and refine another dataset. From this previus year dataset, we get the value. 
We are now able to compute the evolution from the last value VS value one year ago.
```html
    <!-- Prerequisites API calls to compute evolution -->
    <span ods-results="items"
          ods-results-context="kpi10solarpower"
          ods-results-max="1"
          ng-repeat="current in items">

        <span ng-init="myvar['currentdate'] = (current.fields.date | moment : 'YYYY-MM-01')"></span>
        <span ng-init="myvar['currentcapacity'] = current.fields.total_installed_solar_pv_capacity_mwh"></span>
        <span ng-init="myvar['lastdate'] = (myvar.currentdate | momentadd : 'years' : -1)"></span>

        {{ kpi10solarpowerlast.parameters['q'] = (myvar.lastdate | moment : 'YYYY-MM-DD') ; "" }}

        <span ods-results="itemslast"
              ods-results-context="kpi10solarpowerlast"
              ods-results-max="1"
              ng-repeat="last in itemslast">

            <span ng-init="myvar['lastcapacity'] = last.fields.total_installed_solar_pv_capacity_mwh"></span>
            <span ng-init="myvar['diff'] = 100 * (myvar.currentcapacity - myvar.lastcapacity) / myvar.lastcapacity"></span>
        </span>
    </span>
```

`myvar` structure contains all pre-computed values to now fill-in KPIs
```html
    <!-- KPI 1 - first row -->
    <a href="/pages/energy/#/#solar"
       class="perfkpi-card perfkpi-card--times-white"
       ng-if="myvar.currentcapacity"
       ng-class="{'perfkpi-card--green perfkpi-card--check-white': myvar.currentcapacity >= goals[kpitheme]['1'].value, 'perfkpi-card--red perfkpi-card--times-white': myvar.currentcapacity < goals[kpitheme]['1'].value}">
        <p>
            <span class="perfkpi-bold perfkpi-white">
                <ods-spinner ng-hide="myvar.currentcapacity|isDefined"></ods-spinner>
                {{ myvar.currentcapacity }}
            </span> {{ goals[kpitheme]['1'].title }}
        </p>
        <p class="perfkpi-small perfkpi-italic">{{ myvar.currentdate | date : 'MMMM yyyy' }}</p>
        <p class="perfkpi-small perfkpi-italic perfkpi-goal"><i class="fa fa-bullseye"
                                                                aria-hidden="true"> </i> {{ goals[kpitheme]['1'].value | number }} {{ goals[kpitheme]['1'].unit }}
        </p>
        <span class="perfkpi-marker"></span>
    </a>

    <!-- KPI 2 - second row -->
    <a href="/pages/energy/#/#solar"
       class="perfkpi-card"
       ng-if="myvar.diff"
       ng-class="{'perfkpi-card--red perfkpi-card--times-white': myvar.diff < goals[kpitheme]['2'].value, 'perfkpi-card--green perfkpi-card--check-white': myvar.diff >= goals[kpitheme]['2'].value}">
        <p>
            <span class="perfkpi-bold perfkpi-white">
                <ods-spinner ng-hide="myvar.diff|isDefined"></ods-spinner>
                {{ myvar.diff>=0?'+':'-' }} {{ myvar.diff | number : 1 }}%</span> {{ goals[kpitheme]['2'].title }}
        </p>
        <p class="perfkpi-small perfkpi-italic">
            {{ myvar.currentdate | date : 'MMMM yyyy' }} compared
            to {{ myvar.lastdate | date : 'MMMM yyyy' }}
        </p>
        <p class="perfkpi-small perfkpi-italic perfkpi-goal"><i class="fa fa-bullseye"
                                                                aria-hidden="true"> </i> {{ goals[kpitheme]['2'].value | number }} {{ goals[kpitheme]['2'].unit }}
        </p>
        <span class="perfkpi-marker"></span>
    </a>
</div>
```

