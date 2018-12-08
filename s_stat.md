# How my final project works

My final projects uses a series of functions nested within the _s_stats()_ function in order to create a data table and visualize it with plots and analyze it with regressions.

# Functions

Each function runs after an initial scraping of a Wikipedia page to create a single-column data frame containing a list of each state's Wikipedia link. The input for each function is the aforementioned data frame, and a for loop within each function goes down the list of Wikipedia links to create a list of scraped data.

_s_name()_ : Scraping for every state name
```s_name <- function(df) {
  s1 <- NULL
  for (i in 1:nrow(df)) {
    s2 <- gsub(' \\W\\S{0,4}\\s?state\\W', '', as.character(read_html(toString(df[i,1])) %>% html_nodes(xpath = '//div/h1[@id = "firstHeading"]') %>% html_text()))
    s1 <- c(s1, s2)
    message(s2)
  }
  return(s1)
}
```

Every state's Wikipedia page has its name under the xpath `//div/h1[@id = "firstHeading"]`, and _s_name()_ extracts this with a regex (` \\W\\S{0,4}\\s?state\\W`) in order to get the state name alone.

Some states, such as New York, have their name appended with "(state)" to differentiate it from other Wikipedia pages about different 'New Yorks'. The regex within the _gsub()_ extracts the state name, but ignores strings and their surrounding parentheses.

_s_pop_rank()_ : Scraping for every states' population rank (out of 50)
```s_pop_rank <- function(df) {
  s1 <- NULL
  for (i in 1:nrow(df)) {
    s2 <- as.numeric(read_html(toString(df[i,1])) %>% html_nodes('table') %>% .[[1]] %>% html_nodes(xpath = '//table[contains(@class, "vcard")]//th[@scope][text() = "Population"]/following-sibling::td/a') %>% html_text() %>% str_extract('[:digit:]{1,2}'))
    s1 <- c(s1, s2)
    message(s2)
  }
  return(s1)
}
```

The function _s_pop_rank()_ functions similarly to _s_name()_, using an xpath (`//table[contains(@class, "vcard")]//th[@scope][text() = "Population"]/following-sibling::td/a`) and a regex (`[:digit:]{1,2}`) to exclusively extract a string from the designated Wikipedia page that resembles a number one or two digits in length, and converts it to a numeric value.

_s_pop()_ : Scraping for every states' population
```s_pop <- function(df) {
  s1 <- NULL
  for (i in 1:nrow(df)) {
    s2 <- as.numeric(gsub(",", "", read_html(toString(df[i,1])) %>% html_nodes('table') %>% .[[1]] %>% html_nodes(xpath = '//table[contains(@class, "vcard")]//th[@scope][text() = "Population"]/ancestor::tr/following-sibling::tr[1]/td/text()[1]') %>% html_text() %>% str_extract('\\S+'), perl = TRUE))
    s1 <- c(s1, s2)
    message(s2)
  }
  return(s1)
}
```

The function _s_pop()_ functions similarly to the previously mentioned functions, using an xpath (`//table[contains(@class, "vcard")]//th[@scope][text() = "Population"]/ancestor::tr/following-sibling::tr[1]/td/text()[1]`) to extract a string from the designated Wikipedia page. This string is then processed by the function _gsub()_, which removes commas, and is then converted into a numeric value.

_s_cap()_ : Scraping for every states' capital city
```s_cap <- function(df) {
  s1 <- NULL
  for (i in 1:nrow(df)) {
    s2 <- as.character(read_html(toString(df[i,1])) %>% html_nodes(xpath = '//table[contains(@class, "vcard")]//a[contains(@href, "capital")]/ancestor::th/following-sibling::td/a') %>% html_text())
    s1 <- c(s1, s2)
    message(s2)
  }
  return(s1)
}
```

The function _s_cap()_ functions similarly to the previously mentioned functions, using an xpath (`//table[contains(@class, "vcard")]//a[contains(@href, "capital")]/ancestor::th/following-sibling::td/a`) to extract a string from the designated Wikipedia page.

_s_l_city()_ : Scraping for every states' largest city
```s_l_city <- function(df) {
  s1 <- NULL
  for (i in 1:nrow(df)) {
    s2 <- as.character(read_html(toString(df[i,1])) %>% html_nodes(xpath = '//table[contains(@class, "vcard")]//a[contains(@href, "largest_cities")]/ancestor::th/following-sibling::td/a') %>% html_text())
    s1 <- c(s1, s2)
    message(s2)
  }
  return(s1)
}
```

The function _s_l_city()_ functions similarly to the previously mentioned functions, using an xpath (`//table[contains(@class, "vcard")]//a[contains(@href, "largest_cities")]/ancestor::th/following-sibling::td/a`) to extract a string from the designated Wikipedia page.

_s_dens_rank()_ : Scraping for every states' population density rank (out of 50)
```s_dens_rank <- function(df) {
    s1 <- NULL
    for (i in 1:nrow(df)) {
      s2 <- as.numeric(read_html(toString(df[i,1])) %>% html_nodes(xpath = '//table[contains(@class, "vcard")]//td/a[contains(@href, "population_density")]') %>% html_text() %>% str_extract('[:digit:]{1,2}'))
      s1 <- c(s1, s2)
      message(s2)
    }
    return(s1)
  }
  ```
The function _s_dens()_ functions similarly to the previously mentioned functions, using an xpath (`//table[contains(@class, "vcard")]//td/a[contains(@href, "population_density")]`) and a regex (`[:digit:]{1,2}`) to exclusively extract a string from the designated Wikipedia page that resembles a number one or two digits in length, and converts it to a numeric value.

_s_dens()_ : Scraping for every states' population density
```s_dens <- function(df) {
  s1 <- NULL
  for (i in 1:nrow(df)) {
    s2 <- as.numeric(read_html(toString(df[i,1])) %>% html_nodes(xpath = '//table[contains(@class, "vcard")]//th[@scope]/a[text() = "Density"]/ancestor::tr/td/text()[1]') %>% html_text() %>% str_extract('[:digit:]{1,3}[.]?[:digit:]{1,2}'))
    s1 <- c(s1, s2)
    message(s2)
  }
  return(s1)
}
```
The function _s_dens()_ functions similarly to the previously mentioned functions, using an xpath (`//table[contains(@class, "vcard")]//th[@scope]/a[text() = "Density"]/ancestor::tr/td/text()[1]`) and a regex (`[:digit:]{1,3}[.]?[:digit:]{1,2}`) to exclusively extract a string from the designated Wikipedia page that resembles a number with an optional decimal points, and converts it to a numeric value.


---
