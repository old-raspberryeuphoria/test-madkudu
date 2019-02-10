### Technical Requirement Document for 'The Antelopes Codex' (Proof Of Concept)
Draft 0.1

## Objective of this POC

'The Antelopes Codex' aims to provide an easy way to visualise and compare various data on antelopes species. This web application will show a table containing the antelopes data, graphical charts, and specific statistics about the antelopes.  
The end users are, in no particular order, data scientists, curious people, or wandering antelopes looking for data about their counterparts.

## Overview

In the following document, you will first find the functionalities provided by this application:  
- a description for every action available on the user interface  
- which devices are available to use the app  

Then, you will find specific and technical requirements:  
- technologies  
- inputs and outputs  
- components available  
- required performance  

Finally, the document includes details on how to set up the project on your computer.

## Functionalities

# The user interface must include the following:  

- A table to view every antelopes on the main page. The table must display the name, the continent, the weight, the height, the hornes, and a picture of every antelope.
- A graphical chart to show the data [WIP]  
- A way to change representation for the graphical chart  
- A way to export the data as a .csv file

# The application must be available first in foremost on desktop, as a web application  

As this is a Proof Of Concept, it should be focused on desktop.

## Specific requirements (inputs / outputs, components, performance)  

# The POC must use these technologies:

- VueJS  
- Sass  
- Webpack  

As time is of the essence for this POC, and because there is no real need for a back-end, the application should be only client-side.  

A proper application could include the following technologies later:  
- D3.js (https://d3js.org/), a library for manipulating documents based on data  

# Data

The front-end should use data from this json file: https://s3-us-west-2.amazonaws.com/team.madkudu.com/species.json.

# The following components must be available to every users on the application:  

- A fixed side column of the left of the screen for navigation

This column should provide the following buttons:  
- Overview (jump to start of the page)  
- Data (jump to anchor #data)  
- Export (see "Export functionnality")

- A main column to display content

- At the start of the main column, an overview section to show all antelopes in a table.  
This section should have a heading title with the text "Overview".  
The table should have a first row with the following columns :  

- Name  
- Continent  
- Weight  
- Height  
- Horns  

Hovering the name should give a preview picture of the antelope, while clicking it should redirect to the real picture.  

- Below the table, there should be a data section.  
This section should have a heading title with the text "Data".  
This section should have a sub-section "Filters".  

On a first horizontal block inside this sub-section, there should be two "Select" components to let the user update the data by grouping or filtering antelopes. The default selected values should be `null` for the two selects.  
- The first select should have the label "Group by" and the options "[Continent, Horns]", and there could be only 1 option pickable at any time  
When selecting an option, every antelopes should be grouped under either their "continent" property or their "horns" propert.  
- The first select should have the label "Filter by" and the options "[`ContinentsList`, `HornsList`]", where `ContinentsList` is a list of every continents extrapolated from the data, and `HornsList` is a list of every horns extrapolated from the data, and there should be multiple options pickable at anytime.  
When selecting an option, the antelopes set should filter out the data who does not correspond to the picked option(s).  

On a second horizontal block inside this sub-section, there should two "Slider" components to let the user update the data by filtering antelopes by.  
- The first slider should have the label "Weight". The default values should be the minimum and maximum weight extrapolated from the data.  
- The first slider should have the label "Height". The default values should be the minimum and maximum height extrapolated from the data.  

Below the filters, there should be a graphical chart as vertical bars to show and compare data.  

## Project setup
```
yarn install
```

### Compiles and hot-reloads for development
```
yarn run serve
```

### Compiles and minifies for production
```
yarn run build
```

### Lints and fixes files
```
yarn run lint
```
