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
- A graphical chart to show the data
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

This column should provide the following links:  
- Overview  
- Graphics  

The "Graphics" link should render a sub-list of links for every graphic available. For this POC, only one will be provided ("Horns by continents")

- A main column to display the current page

- On the page "Overview", there should be a section dedicated to show all antelopes in a table.  
This section should have a heading title with the text "Overview".  
The table should have a first row with the following headings, one for each properties except picture:  

- Name  
- Continent  
- Weight  
- Height  
- Horns  

Clicking on a heading should sort the table by the choosen key, in ascending order if it's the first time and otherwhise in descending order.  
By default, the table should be sorted by the key `name` in ASC order.  

- On the page "Horns by continents", there should be a graphic showing the repartition of horns-type per continent.  
This section should have a heading title with the text "Antelopes horns type by continent".  
There should be a paragraph to introduce the graphic at the start of the page.  

Each continent should be represented by a column divided by every horns-type you can find on the continent, with a label below the column.  
A legend should be displayed at the right of the columns.  
Each horn-type should have its own color.  
Two buttons should be available to switch between a percentage-based or count-based display of the number of horns-types. Only one button can be pressed at the same time.  

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
