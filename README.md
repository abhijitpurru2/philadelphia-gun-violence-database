# Introduction

The code included in this folder was used to support 2 tasks: (i) clean up and standardize data from various sources, namely [OpenDataPhilly](https://www.opendataphilly.org/dataset/shooting-victims) and the [Gun Violence Database](http://gun-violence.org/), (ii) use the [Newscatcher API](https://rapidapi.com/newscatcher-api-newscatcher-api-default/api/newscatcher/) to collect news articles about gun violence in Philadelphia and scrape the articles for keywords and links referencing other news articles.

**File Directory**
<br>

code
*   *GunViolence.ipynb* - unpacks and organizes data from the Gun Violence Database
*   *shootings_data_cleaning.ipynb* - clean the  shooting victim dataset obtained from OpenDataPhilly

*    *bodyparts.txt* - a text file capturing common bodyparts seen in the 
`shootings.csv` to train `textblob` for wound correction function in 
`Shooting_data_cleaning.ipynb`


*   *Scrape_Articles.ipynb* - master script for producing scraped articles dataset
*   *Scrape_Crime_Data.ipynb* - scraping CSV's from OpenDataPhilly crime database and turning them into dataframes


<br>

produced_datasets
*   *scraped_articles_with_keywords.csv* - dataset produced by Scrape_Articles.ipynb when run on 12/04/2021
*   *firearm_realted_crimes.csv* - CSV file that records the dataframe created by Scrape_Crime_Data.ipynb
*   *intermediate_shooting_data.csv* - CSV file that contains the preprocessing shooting victim dataset obtained from OpenDataPhilly
*   *gv_events.csv*  - CSV file has details of shootings in Philadelphia from the gun_violence-archive
*   *gv_event_participants.csv*  - CSV file has details about the shooter and victims in Philadelphia from the gun_violence-archive

<br>

sample_data
*   crime_data - CSV data downloaded from OpenDataPhilly "crime_incidents"
*   Events.tsv - sample events data from Gun Violence Database
*   Articles-with-extracted-info.tsv - sample article data from Gun Violence Database
   
<br>
<br>
<br>

# Data Dictionaries

*scraped_articles_with_keywords.csv*

|     Column     |     Data Type     |     Description     |
|     :----:     |     :----:        |---------------------|
|index           |  int              |index of article in dataframe|
|link            |  str              |link of article returned by API|
|title           |  str              |article title|
|summary         |  str              |article summary|
|published_date  |  str              |article published date|
|keywords        |  list of str      |article keywords|
|external_links  |  set of str       |links to other news articles|

<br>

*firearm_related_crimes.csv*

|     Column     |     Data Type     |     Description     |
|     :----:     |     :----:        |---------------------|
|index           |  int              |index of data point in dataframe|
|dispatch_time   |  datetime64[ns]   |date and time when the crime occured|
|crime_type      |  str              |type of crime (firearm related)|
|latitude        |  float64          |latitude of crime|
|longitude       |  float64          |longitude of crime|

<br>

*intermediate_shooting_data.csv*

|     Column       |     Data Type      |     Description                                                 |
|     :----:       |     :----:         |-----------------------------------------------------------------|
|objectid	   |  int64		|	Unique identifier of a victim in the incidence|
|year	           |  int64		|	Year of incidence|
|date	           |  object		|	Date of incidence|
|time	           |  object		|	Time of incidence (up to minute)|
|race	           |  object		|	Race of the victim described|
|sex	           |  object		|	Sex of the victim described|
|age	           |  float64		|	Age of the victim described|
|officer_involved  |  bool		|	Is any officer involved|
|offender_injured  |  bool		|	Is any offender injured|
|offender_deceased |  bool		|	Did any offender decease|
|location	   |  object		|	Location of the incidence|
|latino		   |  float64		|	Is the victim described latino|
|point_x	   |  float64		|	Longitude of the location of the incidence|
|point_y	   |  float64		|	Latitude of the location of the incidence|
|dist	           |  float64		|	unclear|
|inside	           |  float64		|	The incidence happened indoor|
|outside	   |  float64		|	The incidence happened outdoor|
|fatal	           |  float64		|	Fatality of the victim described|
|lat	           |  float64		|	Latitude of the location of the incidence|
|lng	           |  float64		|	Longitude of the location of the incidence|
|datetime	   |  datetime64[ns]    |	Timestamp of the incidence|
|month	           |  object		|	Month of the incidence|
|day	           |  object		|	Day of month of the incidence|
|hour	           |  object		|	Hour of day of the incidence|
|lng_diff	   |  float64		|	Difference in longitude between 'lng' and 'point_x|
|lat_diff	   |  float64		|	Difference in latitude between 'lat' and 'point_y|
|rev_wound	   |  object		|	Revised location(s) of wound of the person described|

<br>

*gv_events.csv*

|     Column     |     Data Type     |     Description     |
|     :----:     |     :----:        |---------------------|
|index           |  int              |index of article in dataframe|
|Address         |  str              |address of the shooting incident|
|Date            |  datetime         |article date|
|Info about participants  |  list              |list with dict of details on shooting participants|
|clock-time      |   str             |Time of shooting|
|details         |   str             |Additional location information about the shooting|
|number-of-shots fired    |   str              |str of shots fired|
|time-day  	 |   str             |Coarse-grained time of shooting (e.g. "morning", "late evening")|
|type-of-gun     |   str             |gun type|
|24_hour         |   int             |derived column from clock-time converted to 24hr times and int type|
|minute          |   int             |derived column from clock-time converted to int type|

<br>

*gv_event_participants.csv*

|     Column     |     Data Type     |     Description     |
|     :----:     |     :----:        |---------------------|
|injured          |  bool           |whether participant was injured|
|name        |  str              |participant's name|
|hospitalized          | bool             |whether participant was hospitalized|
|gender        |  str              |participant gender|
|age |  int or str             |participant age|
|race       |  str      |participant race|
|killed  |  bool       |whether participant was killed|
|is_vitcim |  bool       |whether participant was the victim|

<br>
<br>
<br>

# Usage

**Scrape_Articles.ipynb**

Functions:

*   *collect_articles* - makes call to Newscatcher API
*   *extract_article_info* - extracts article info from call response
*   *create_dataframe* - builds dataframe of desired info
*   *scrape_keywords* - scrapes keywords from soup of links in dataframe
*   *scrape_external_links* - scrapes links from soup of links in dataframe
*   *colab_to_excel* - saves dataframe as an Excel spreadsheet
*   *colab_to_csv* - saves dataframe as a CSV file
<br>

<br>

Steps Taken: 
>Get Response from API:


To make calls to the API, an account and user key will be required. A Rapid API account can be made at <https://rapidapi.com/hub>. The specific API is called Newscatcher. Documentation for the API can be found at <https://rapidapi.com/newscatcher-api-newscatcher-api-default/api/newscatcher/>.


Collecting articles from the API using the `collect_articles` function requires a specified user key and query. Query formatting can be found in the API documentation. The query used for this project is `"shooting AND Philadelphia"` which extracts articles that include both "shooting" and "Philadelphia".
<br>
<br>
>Get Desired info from API Response:

The response to the API should include whether the call was successful, the number of articles returned, and the number of pages of articles. To get every article, a call will need to be made for each page of articles.

The `extract_article_info` function can be used to extract the information stored in the articles dictionary returned by the API call. Possible extracted information include the article `title`, `author`, `published_date`,  `link`, `summary`, `rights`, `topic`, `country`, `language`, etc. The function will return a dictionary where the key is the category of information (i.e title) and the value is a list of the categories (i.e titles) of each of the articles included in the API call.
<br>
<br>
>Store Data in Dataframe:

Given a dictionary of article info, the `create_dataframe` function can be used to convert the dictionary into a Pandas dataframe. This function also supports masking to remove articles with undesired keywords. To do so, the user should provide a list of undesired keywords and a column name to search for the keywords. For example, if a user wanted to remove articles that contain the word "76ers" from the "summary" column of the dataframe, the `unwanted_topic` would be `["76ers"]` and the `column_name` would be `"summary"`.
<br>
<br>
>Scraping Keywords and Links:

To scrape the articles returned by the API for keywords and links that link to other news articles, the `scrape_keywords` and `scrape_external_links` can be used. It is important to note that the links of the articles to be scraped must be in a column titled 'link' within the dataframe. Some websites do not allow for scraping, which returns a "Forbidden" error. More investigation is required to determine other methods of scraping for these websites. For the purposes of this project, the websites that didn't allow for scraping return an empty list of keywords and an empty set of external links.
<br>
<br>
>Exporting data as a CSV or Excel Spreadsheet:

To save and export the resulting dataset as a CSV or Excel Spreadsheet, the `colab_to_csv` or `colab_to_excel` functions can be used. These functions will download the dataframe to your computer in the desired format.

<br>
<br>

**Scrape_Crime_Data.ipynb**

Functions:

*   *scrape_website_links* - scrapes the website that is passed into the function
*   *years_links* - gets all the CSV file links from a BeautifulSoup object for each year
*   *csv_links* - gets all the CSV download links from the links of each year
*   *download_csvs* - downloads all CSV files from the links and saves in local storage
*   *create_pandas_dataframe* - uses all CSV files to create a complete dataframe for all crime data
*   *crime_type_database* - returns all unique crime types in dataframe
*   *return_firearm_data* - returns a dataframe of crime that involves firearms
*   *create_map* - creates map of philadelphia and plots the location of the crimes
*   *save_data* - saves dataframe to csv

<br>

Steps Taken:

> Retrieve Data From Website: 

 The link (<https://www.opendataphilly.org/dataset/crime-incidents>) was used to find the crime dataset of all crime in Philadelphia. It was found using the OpenDataPhilly website. The dataset has information of crimes spanning from 2006 to current day. Each year is divided into CSV files for each year. The CSV file for 2021 is constantly being updated everyday, so the information is fresh. 

Using `scrape_website_links` function, the BeautifulSoup object for the website was retrieved for later use. Since there were multiple CSVs for the data, `years_links` function was used to find the links of the each CSV file. Using this link however, didn't lead to downloading the CSV. The links lead to another page where there was a link to download the CSV. Using `csv_links` function, the download links were scraped. 

`download_csvs` function was used to download each year's CSV file and that was loaded onto local storage inside the `crime_data` folder.

<br>


> Combine Data and Create Dataframe:

With the year CSVs downloaded, the `create_pandas_dataframe` function can be used to combine all the data into one workable dataframe. First, from each CSV, only the `dispatch_date_time`, `text_general_code`, `lat` and `lng` columns were used. Each year data was then loaded into a dataframe and then appended to the complete dataframe called `full_list`. The names of the columns were changed to `dispatch_time`, `crime_type`, `latitude` and `longitude`. The `dispatch_time` was changed to datetime format. 

<br>

> Cleaning Data and Finding Vital Information:

Now that the data was combined, it can be finally be cleaned. First, the `crime_type_database` function was used to find the unique crime types in the dataset. From that data, it was determined that there were specific crime types that related to the use of firearms. Unfortunately, the crime types `Homicide - Criminal`, `Homicide - Justifiable` and `Homicide - Gross Negligence` don't specify if it happened with/without a gun. These crime types had to be dropped due to this problem. Using `return_firearm_data` function, the dataframe was cleaned to have only firearm related crimes. The keyword of `Firearm` was included and the keyword of `No` was not included. This cleaning brought the data from about 2.7 million to about 90 thousand data points.

<br>

> Showing Use Case of Data:

With the use of the cleaned dataset, the data points can be displayed on a map to show as an example. Using the `geopandas` and `shapely` libraries, the crimes that related to firearms can be plotted on a map. The `create_map` function used the `latitude` and `longitude` from the dataset to plot red dots on where the crime happened. The map created using `map.shp` which was also obtained from OpenDataPhilly.

> Export Data to CSV:

Finally, the dataframe was saved to local storage as a CSV file using the `save_data` function. This makes it possible to load this dataframe from a CSV file to be combined with other datasets in the future.

<br>
<br>
<br>

**GunViolence.ipynb**

Functions:

*   *read_tsv* - reads events.tsv to dataframe optional parameters to parse dates and filter locations
*   *extract_keys* - extracts details about the evnts into a dictionary and then a dataframe.
*   *parse_time* - uses regular expression to convert user supplied time of the event(i.e 10:30 p.m) into integer values for 24 hour time to be used a datetime object
*   *get_participants* - extract information about the participants in events and returns a cleaned dictionary
*   *clean* - iterates over Json column, loads the json into a Json string, normalizes the Json string and then appends it to a new dataframe
*   *extract_shooter* - extracts the shooter data from  shooter-section
*   *extract_victim* - extracts the victim data from  victim-section
*   *get_philly_data* - extracts only the rows with 'Philadelphia' as the city

<br>

Steps Taken:


>Read and Parse TSV File

The `read_tsv` function loads the `events.tsv` file with the `read_csv` function and finds the events containing the location if the location is provided as a parameter.

>Extract 'Value' Key/Value Pair

Next, the 'value' key/value pair is extracted from each row and create a new dataframe called `info_gun_time`.

>Print Unique Values

The unique values of each column except the details is printed.

>Concatenation

The `phl_events` and `info_gun_time` dataframes are concatenated and the "Info about time, type of gun etc." column is dropped.

>Parse Time

The time is parsed and added to a new dataframe called `time` which is concatenated to the combined dataframe.

>Literal Eval

The `literal_eval` module is used to clean the 'Info about participants' column.

>Get Participants

The `get_participants` function is used to get information about the participants from the 'Info about participants' column in the phl_events dataframe.

>Articles dataset

The 'Articles-with-extracted-info.tsv' dataset is read using the `read_csv` function into the `articles` variable.

>Drop Dummy Data

There were approximately 7200 rows with an invalid url. These were dropped from the dataframe.

>Clean Dataset

The `articles` dataframe is cleaned by making a copy of the "Json" column in the `articles` dataframe and then iterating over each row in that copy, using the `json_loads` function to load each row into a json string, normalizing that string and then appending each string to a row in a new dataframe. The dataframe is returned.

>Extract Shooter and Victim

The shooter and victim information is extracted from the dataframe by getting the values inside of the `value` dictionary in each part of the shooter and victim information. The shooter and victim info are added back to the dataframe.

>Get Philly Info

Data for only the articles from Philadelphia are found using the `get_philly_data` function.

>New Dataframe

The `'shooter_names'`,`'shooter_ages'`, `'shooter_genders'`, `'shooter_races'`, `'victim_names'`,`'victim_ages'`, `'victim_genders'`, `'victim_races'`, `'victim_was'`, `'circumstances.number-of-shots-fired.value'`, `'circumstances.type-of-gun.value'`, `'date-and-time.city.value'`, `'date-and-time.clock-time.value'`, `'date-and-time.date'`, `'date-and-time.details.value'`, `'date-and-time.time-day.value'`, `'radio1.The firearm was used during another crime.'`, `'radio1.The firearm was used in self defense.'`, `'radio1.The incident was a case of domestic violence.'`, `'radio1.The shooter and the victim knew each other.'`, `'radio2.Alcohol was involved.'`, `'radio2.Drugs (other than alcohol) were involved.'`, `'radio2.The shooting was a suicide or suicide attempt.'`, `'radio2.The shooting was self-directed.'`, `'radio3.The firearm was owned by the victim/victims family.'`, `'radio3.The firearm was stolen.'`, `'radio3.The shooting was by a police officer.'`, `'radio3.The shooting was directed at a police officer.'`, and `'radio3.The shooting was unintentional.'` columns are set to the `philly_data` dataframe.

<br>
<br>
<br>

**shootings_data_cleaning.ipynb**

Functions:
*   *collect_articles* - makes call to Newscatcher API

*   *remove_col* -  removes multiple columns at once
*   *convert_to_bool* - changes binary column into boolean data type (limited by `np.nan`/ `None` values)
*   *reformat_datetime* - creates a new column with date and time info (timestamp format), and generates separate columns to capture month, day of 
    month, and hour of day. All these columns are added to the original dataframe
*   *verify_lat_lng* - calculates the differences between the two sets of longitude and latitude columns and return a
    data frame with entries of non-zero difference for longitude and latitude respectively
*   *col_lower_case_and_fillna* - returns lower case and fill out null value with empty string for a column
*   *correct_wound* - trains textblob using a source file carrying known common bodyparts (user can modify as necessary)
    and correct the wound (string). The corrected spelling is then saved as a string
*   *standardize_col* - takes the wound column that is already in lower case and correct the mispelling using correct_wound
    function built previously. Morevoer, it cleans up a few common alternative spellings (multi, butt, unk) and the
    misspelling regarding shoulder that is not fixed previously. The corrected strings are saved as a new column in the input dataframe
*  *format_wound* - standardizes the format of wound, mainly for those containing more than one bodyparts. 
    E.g., "head,multiple", "multiple,head" are the same and hence are formatted to show the same value. The results are saved as a new column in the input dataframe

Steps Taken:


> Read CSV File

Read and save `shootings.csv` into a data frame using `pd.read_csv()` function. 

> Initial Data Cleaning

Column renaming, removal, change of data types were performed. Original column `date_` did not following naming convention of the data set and was updated 
to `date`. 

Columns `the geom`, `the_geom_webmercator`, `dc_key`, and `code` were redundant and removed. A function `remove_col` was created to removed the 
aforementoined columns at once. 

Columns `officer_involved`, `offender_injured`, and `offender_deceased` used Y/N coding initially. They were changed into 1/0 coding so that there was a 
consistent coding format for all binary variables. Six binary variable (`inside`, `outside`, `fatal`, and the aforementioned) were recoded into True/False coding to adopt boolean data type using function `convert_to_bool`. The process was limited by null values because they cannot be presented as boolean data.

> Date and Time Information

Date and time data were captured in `date` and `time` columns in string format. A `datetime` column (datetime format) was created together with `month`,
`day` (day of month) and `hour` (hour of day) columns to allow users group data by these factors. 

> Longitude and Latitude Information

The data set contains two sets of longitude (`lng`, `point_x`) and latitude (`lat`, `point_y`) information. With the initial intention to select one column
for each variable, a function `verify_lat_lng` was created to compute the degree difference between the two. While most differences are relatively small, all
variables are retained to allow future users to make an informed decision and compare with external sources. 

> Standardize Wound Data

Since the wound data was mannually entered to the system, there was no standard format of the info. Capitalization, mispelling, abbreviations, use
of plural/ single forms were some of the identified problems. After turning all values into lower case and filling up null values with an empty string,
module `textblob` was utitlized to standardize wound column. A text file `bodyparts.txt` with common bodyparts seen in the dataset was created to train 
`textblob` to correct and standardize wounds. There were initially 125 different values in `wound` and evenutally 54 values in the final data set. 
Future users can update the training file according to their needs to improve the standardization process.  

> Export Data in CSV Format

Finally, the data frame was saved into a csv format to the local machine using `to_csv` function. 

<br>
<br>

# Concatenation
After all the datasets were cleaned, it was determined that they could not be combined together. There was no clear join point for all the datasets. Each dataset had multiple missing values so, even when joined, there would be many NULL column values. 

<br>
<br>

# Future Work
Machine learning analysis could be used on scraped articles dataset
* Words and phrases that probabilistically lead to relevant articles include “shooting”, “guns”, “shots”, “murdered”, “gang”, “drug deal”, “dangerous”, “police-involved”, “stray bullet”, “shot over”, “suspects”, “homicide”, “gunmen”, “shooting”, “turf war”, and “gunfire”.

Natural language processing could be used on scraped articles
* NLP models could be trained using a set of articles known to provide information about gun violence. The model could then identify relevant information about gun violence in upcoming articles without those articles being directly identified as articles about gun violence.

Integration of zip code based census data
* Poverty, Education level etc.

<br>
<br>

# Conclusion

* The challenges faced in this dataset production project are not novel
 * Researchers have struggled for years
 * Political hindrances on research
 * No standardization on data reporting methods
   * Unmatching data structures
   * Misspellings
* Project progress is important stepping stone towards a comprehensive dataset
 * Less reliance on volunteer effort
 * Combination of official data from PD as well as qualitative data from news articles (typically discovered after police reporting)

* Figuring out what mediums are the most effective for data collection

<br>
<br>

# Sources
OpenDataPhilly Datasets

><https://www.opendataphilly.org/dataset/shooting-victims>

><https://www.opendataphilly.org/dataset/crime-incidents>

Gun Violence Database
><http://gun-violence.org/>

Rapid Newscatcher API 

><https://rapidapi.com/newscatcher-api-newscatcher-api-default/api/newscatcher/details>
