ODS+ PerfKPI - CSS guide and templates
=======

Welcome to the PerfKPI CSS and templates guide. 

You'll find here all CSS classes and HTML blocks used in the application, and all the pointers to reuse the [PerfKPI stylesheet](./resources/web/perfkpi.css).
Feel free to reuse, modify, enrich or override it as you please !

### CSS Stylesheet 

Overall page

Remove borders, take all the width

```css
.perfkpi-page {
    margin: -40px -30px 0 -30px;
}
```

Page title, navigation bar over themes, and background placeholder 

```css
.perfkpi-page__header {
    background-position: center center;
    background-size: cover;
    display: flex;
    height: 320px;
    overflow: hidden;
}

.perfkpi-page__header .container {
    position: relative;
    align-self: flex-end;
    display: flex;
}

.perfkpi-page__subheader {
    padding: 25px 0;
    border-bottom: 1px solid #CCC;
    background-color: white;
    font-size: 1.3rem;
    margin-bottom: 40px;
}

.perfkpi-page__title {
    margin: 0px -10px;
    font-size: 2.43rem;
    line-height: 2.9rem;
    color: #1A1A1A;
    padding: 25px 20px;
    overflow: hidden;
    background-color: white;
    border-bottom: 1px solid #CCC;
    display: inline-block;
}

@media screen and (max-width: 767px) {
    .perfkpi-page__title {
        width: 100vw;
    }
}

@media screen and (min-width: 768px) {
    .perfkpi-page__header .container:before {
        content: ' ';
        background: #fff;
        width: 1000px;
        height: 320px;
        position: absolute;
        left: -980px;
        top: 0;
    }
}
```

KPI Title


```css
.perfkpi-page__subtitle {
    border-left: 6px solid #0bbfbf;
    overflow: auto;
    padding: 20px 10px 20px 26px;
    font-size: 2.43rem;
    font-weight: 400;
    line-height: 2.9rem;
    margin: 40px 0 40px -32px;
}

.perfkpi-page__subtitle:first-child {
    margin-top: 0;
}

@media screen and (min-width: 768px) {
    .perfkpi-page__subtitle-wrapper {
        display: flex;
        align-items: baseline;
        justify-content: space-between;
    }
}
```

Aside goal (right side)

```css
.perfkpi-page-aside {
    border: 5px solid #0bbfbf;
    font-size: 1.3rem;
    margin: 40px 0;
    padding: 20px 40px;
}

@media screen and (min-width: 768px) {
    .perfkpi-page-aside--inline {
        display: inline-block;
    }
}

.perfkpi-page-aside-title {
    color: #013e63;
    background: white;
    font-weight: bold;
    padding: 0 20px;
    margin-top: -35px;
    float: left;
    margin-left: -20px;
}

.perfkpi-page-aside-content {
    clear: both;
}

.perfkpi-page__subheader-link {
    border-bottom: 1px solid #1A1A1A;
    color: #1A1A1A;
    display: inline-block;
    line-height: 1.5rem;
    padding: 2px 0;
    position: relative;
    text-decoration: none;
    transition: all 0.35s ease;
    margin-right: 2rem;
}

.perfkpi-page__subheader-link--active,
.perfkpi-page__subheader-link:hover {
    color: #0bbfbf;
    text-decoration: none;
}

.perfkpi-page__subheader-link:after {
    background-color: #0bbfbf;
    bottom: -1px;
    content: "";
    display: block;
    height: 1px;
    left: 0;
    position: absolute;
    transition: width 0.35s ease;
    width: 0;
}

.perfkpi-page__subheader-link--active:after,
.perfkpi-page__subheader-link:hover:after {
    width: 100%;
}

.perfkpi-page__subheader-link .odswidget-picto {
    width: 1em;
    display: inline-block;
    fill: #1A1A1A;
}

.perfkpi-page__subheader-link svg, 
.perfkpi-page__subheader-link circle, 
.perfkpi-page__subheader-link path, 
.perfkpi-page__subheader-link polygon, 
.perfkpi-page__subheader-link rect {
    fill: inherit !important;
}

.perfkpi-page__subheader-link--active .odswidget-picto,
.perfkpi-page__subheader-link:hover .odswidget-picto {
    fill: #DF225A;
    transition: fill 0.35s ease;
}
```

A KPI block / placeholder with achieved/non achieved colors and markers

```css
.perfkpi-card {
    display: block;
    font-size: 1.3rem;
    font-weight: 300;
    position: relative;
    padding: 20px;
    border-radius: 5px;
    background-color: #eeeeee7d;
    color: black;
    margin-bottom: 20px;
}

.perfkpi-card > * {
    z-index: 2;
}

.perfkpi-card:before {
    bottom: 0;
    content: "";
    display: block;
    height: 100%;
    left: 0;
    position: absolute;
    top: 0;
    transition: 0.2s ease-in-out width;
    width: 0%;
    border-radius: 5px;
    z-index: -1;
    background-color: #eee;
}

.perfkpi-card:hover {
    text-decoration: none;
}

.perfkpi-card:hover:before {
    width: 100%;
}

/* Card colors */

.perfkpi-card--green {
    background-color: rgba(28, 128, 45, 0.6);
}

.perfkpi-card--green:before {
    background-color: #42A769;
}

.perfkpi-card--red {
    background-color: rgba(175, 29, 29, 0.7);
}

.perfkpi-card--red:before {
    background-color: #E95756;
}

/* Card indicators */

.perfkpi-card--check:after,
.perfkpi-card--check-white:after,
.perfkpi-card--check-green:after,
.perfkpi-card--times:after,
.perfkpi-card--times-white:after,
.perfkpi-card--times-red:after,
.perfkpi-card--down:after,
.perfkpi-card--down-red:after,
.perfkpi-card--down-green:after,
.perfkpi-card--up:after,
.perfkpi-card--up-green:after,
.perfkpi-card--up-red:after {
    font-family: 'FontAwesome';
    font-size: 4rem;
    position: absolute;
    right: 0;
    bottom: -20px;
    color: inherit;
}

.perfkpi-card--right:after {
    font-family: 'FontAwesome';
    font-size: 4rem;
    position: absolute;
    right: 0;
    bottom: -15px;
    color: inherit;
}


.perfkpi-card--check:after,
.perfkpi-card--check-white:after,
.perfkpi-card--check-green:after {
    content: '\f00c';
}

.perfkpi-card--times:after,
.perfkpi-card--times-white:after,
.perfkpi-card--times-red:after {
    content: '\f00d';
}

.perfkpi-card--down:after,
.perfkpi-card--down-green:after,
.perfkpi-card--down-red:after {
    content: '\f063';
}

.perfkpi-card--up:after,
.perfkpi-card--up-green:after,
.perfkpi-card--up-red:after {
    content: '\f062';
}

.perfkpi-card--right:after {
    content: '\f061'
}

.perfkpi-card--times-white:after,
.perfkpi-card--check-white:after {
    color: #FFFFFF;
}

.perfkpi-card--check-green:after,
.perfkpi-card--down-green:after,
.perfkpi-card--up-green:after {
    color: #42A769;
}

.perfkpi-card--times-red:after,
.perfkpi-card--down-red:after,
.perfkpi-card--up-red:after {
    color: #E95756;
}

/* Card font changes */

.perfkpi-card .perfkpi-big {
    font-size: 2rem;
}

.perfkpi-card .perfkpi-small {
    font-size: 1rem;
}

.perfkpi-card h3 {
    font-size: 1.2em;
    font-weight: 400;
}

/* STYLE HELPERS */

.perfkpi-italic {
    font-style: italic;
}

.perfkpi-bold {
    font-weight: bold;
    font-size:1.5em;
    font-weight: 400;
}

.perfkpi-green {
    color: #42A769;
}

.perfkpi-red {
    color: #E95756;
}

.perfkpi-white {
    color: #FFFFFF;
}

.perfkpi-space-between {
    display: flex;
    justify-content: space-between;
    flex-wrap: wrap;
}
```

Grid system, rows and columns

```css
.perfkpi-page-grid {
    margin: -10px;
}
.perfkpi-page-row {
    display: flex;
    align-items: stretch;
    flex-grow: 1;
}

@media (max-width: 768px) {
    .perfkpi-page-row {
        flex-wrap: wrap;   
    }
}

.perfkpi-page-col {
    display: flex;
    flex-grow: 1;
    flex-direction: column;
    align-items: stretch;
}

.perfkpi-page-row > div:not(.perfkpi-page-col),
.perfkpi-page-row > a,
.perfkpi-page-col > div:not(.perfkpi-page-row),
.perfkpi-page-col > a {
    flex-grow: 1;
    /*margin: 10px;*/
}

/* Footnote */

.perfkpi-page__footnote {
    padding: 16px 0 16px 45px;
    margin: 40px 0;
    border-bottom: 1px solid #ccc;
    border-top: 1px solid #ccc;
    position: relative;
}

.perfkpi-page__footnote:before {
    font-family: 'FontAwesome';
    content: '\f069';
    position: absolute;
    top: 16px;
    left: 16px;
}

/* OPENDATASOFT DATA VISUALIZATION */

.highcharts-background {
    fill: none;
}

.ods-chart {
    height: 300px;
}

.perfkpi-gauge--green .odswidget-gauge__svg-filler {
    stroke: #42A769;
}

.perfkpi-gauge--red .odswidget-gauge__svg-filler {
    stroke: #E95756;
}



/* Font-awesome OVERRIDE */
i.fa.fa-bullseye {
    background-image: url(https://s3-eu-west-1.amazonaws.com/aws-ec2-eu-1-opendatasoft-staticfileset/perfkpi-odsplus/theme_image/goal.png);
    background-repeat: no-repeat;
    background-size: contain;
    height: 40px;
    width: 40px;
    float: left;
    margin-top: -10px;
}
i.fa.fa-bullseye:before {
    content: '';
}

.perfkpi-card--check-white .perfkpi-marker {
    background-image: url(/assets/theme_image/OK.png);
    background-repeat: no-repeat;
    background-size: contain;
    height: 40px;
    width: 40px;
    position: absolute;
    right: 5px;
    bottom: 0px;
}

.perfkpi-card--times-white .perfkpi-marker {
    background-image: url(/assets/theme_image/KO.png);
    background-repeat: no-repeat;
    background-size: contain;
    height: 35px;
    width: 35px;
    position: absolute;
    right: 5px;
    bottom: 5px;
}

.perfkpi-card--times-white:after, .perfkpi-card--check:after, .perfkpi-card--check-white:after {
    content: '';
}
```


### Templates

With this existing stylesheet you are now free to copy paste templates from the [template page example](./resources/web/templates.html)

#### Template example

KPI container
```html
<div class="container">
```

KPI title (displayed on the left) and KPI goal (displayed on the right)
```html
    <div class="paris-page__subtitle-wrapper">
        <h2 class="paris-page__subtitle" id="electric-car-sharing-service">
            Template 2 - 1
        </h2>
        <div class="paris-page-aside paris-page-aside--inline">
            <div class="paris-page-aside-title">
                <i class="fa fa-bullseye" aria-hidden="true"></i> Goal
            </div>
            <div class="paris-page-aside-content">
                {{ 123456 | number }} Things
            </div>
        </div>
    </div>
```

Then, a first row, pre-requisite from columns
```html
    <div class="paris-page-row">
```

Then, a column of `col-sm-5` width ([see grid layout for more information](http://docs.opendatasoft.com/en/customizing_look_and_feel/responsive.html#responsive-grid-layouts))
```html
        <div class="col-sm-5 paris-page-col">
```

A first KPI block with a title, a value, and a date
```html
            <div class="paris-card"
                 ng-init="metric = 123456"
                 ng-class="{'paris-card--check-green': metric >= goal, 'paris-card--times-red': metric < goal}">
                <h3>
                    Metric placeholder
                </h3>
                <p>
                    <span class="paris-big paris-bold"
                          ng-class="{'paris-green': metric >= goal, 'paris-red': metric < goal}">
                        <ods-spinner ng-hide="metric | isDefined"></ods-spinner>
                        {{ metric | number : 0 }}
                    </span>
                </p>

                <p class="paris-small paris-italic"> Month Year</p>
            </div>
```

A second KPI block
```html
            <div class="paris-card"
                 ng-init="metric = 123456"
                 ng-class="{'paris-card--check-green': metric >= goal, 'paris-card--times-red': metric < goal}">
                <h3>
                    Metric placeholder
                </h3>
                <p>
                    <span class="paris-big paris-bold"
                          ng-class="{'paris-green': metric >= goal, 'paris-red': metric < goal}">
                        <ods-spinner ng-hide="metric | isDefined"></ods-spinner>
                        {{ metric | number : 0 }}
                    </span>
                </p>

                <p class="paris-small paris-italic"> Month Year</p>
            </div>
        </div>
```

The second column of `col-sm-7` width with 1 KPI block (1 title, 1 chart, 1 date and 1 link to see source data)
```html
        <div class="col-sm-7 paris-page-col">
            <div class="paris-card">
                <h3>
                    Chart placeholder (replace existing chart here)
                </h3>
                <ods-dataset-context context="kpigeneric"
                                     kpigeneric-dataset="kpi-generic">
                    <ods-chart timescale="month" single-y-axis="true">
                        <ods-chart-query context="kpigeneric" field-x="date"
                                         series-breakdown="category" maxpoints="0"
                                         timescale="month">
                            <ods-chart-serie expression-y="value" chart-type="areaspline" function-y="AVG" label-y="Charging slots" color="range-custom" display-values="true" scientific-display="true">
                            </ods-chart-serie>
                        </ods-chart-query>
                    </ods-chart>
                </ods-dataset-context>

                <div class="paris-small paris-italic paris-space-between">
                    <span>Date range</span>
                    <a href="/explore/dataset/kpi-generic"><i class="fa fa-table" aria-hidden="true"></i> See source data</a>

                </div>
            </div>
        </div>
```

```html
    </div>
</div>
```