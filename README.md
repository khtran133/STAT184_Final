# STAT 184 Final
Khoi Tran
Final Project, STAT 184 Fall 2018

My final project heavily utilizes web scraping through the rvest package to create a data set of the 50 U.S. states. The function first scrapes a Wikipedia list for url links for all 50 states, and then scrapes each page for every state for more data. This data is then merged with economic and crime data from other sources, and then analyzed through plots and regression.

The function also outputs an image (.png) of all the plots merged into one, a spreadsheet containing the coefficients for all the various regressions run, and a spreadsheet of the data table created through scraping. 

**Requirements:** 
* The included spreadsheet (state_crime_r.csv) needs to be in the same folder as the R project.
* Install and load packages listed at the top of the function.
