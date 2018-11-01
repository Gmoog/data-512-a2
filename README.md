# Data 512 A-2 Investigating Bias in Data


The overall goal of this project is to analyze potential sources of bias in our dataset, which consists of articles about politicians in english Wikipedia, combined with an overall list of countries and their population's. 

Each of these articles will be parsed through a machine learning algorithm called ORES, which returns a code corresponding to the quality of that article.


What we hope to achieve through this exercise, is to answer questions like why does this country have so few articles about their politicians, or why does this one have so many inspite of its population being small. What sort of biases might have been introduced knowingly and unknowingly into the dataset, and how does this impact our analysis.

All the code used for transforming the data and creating the output tables has been written in Python, and has been listed in the Jupyter notebook 'hcds-a2-bias.ipynb'.

Lets jump in.

### License
The code is released under the MIT license, and is intended to be fully reproducible. 
Additional details can be found at the link below

[https://github.com/Gmoog/data-512-a2/blob/master/LICENSE]

### Data

The data used for the analyses has been merged from 3 different sources.
#### Wikipedia
First up we have the wikipedia data, that lists each article name, followed by which countries politician it addresses, and finally an id field to categorize the last modification done to it. 

The data has been released under the CC-BY-SA 4.0 license, and can be downloaded from the link below

https://figshare.com/articles/Untitled_Item/5513449

All usage is governed under wikipedias policy, which can be found at

https://foundation.wikimedia.org/wiki/Terms_of_Use/en

The file 'page_data.csv' consists of 3 columns
- 'page' - which is the name of the article
- 'country' - which country the politician belongs to
- 'rev_id' - the last edit id

A point to note as per the documentation, is that the country codes are not always consistent, wherever possible they have tried to keep it standard, but thats not always the case.

#### Population
The population data consists of the country name and its corresponding population in millions. 

Again the country name is not strictly countries, as it also lists combined figures for entire continents as well.

The data is in a csv file 'WPDS_2018_data' and can be downloaded from the link below

https://www.dropbox.com/s/5u7sy1xt7g0oi2c/WPDS_2018_data.csv?dl=0

All usage is governed under dropbox data policy, which can be found at

https://www.dropbox.com/terms

The fields in the file are
- 'Geography' - Name of the country/continent
- 'Population mid-2018 (millions)' - Population figures in millions.

#### Article quality
For evaluating the quality for each article, we parse them through a Wikimedia API endpoint for a machine learning service call ORES (Objective Revision Evaluative Services). 

The algorithm returns one out of a possible 6 values for each article as its prediction, and also return the probabilities associated with all the 6 classifiers. 
Obviously the classifier with the highest probability, is the overall prediction. The 6 possibilities in order of best-to-worst are:
- FA - Featured article
- GA - Good article
- B - B-class article
- C - C-class article
- Start - Start-class article
- Stub - Stub-class article

For more details about the API and how its used please refer to the link below

https://www.mediawiki.org/wiki/ORES

Also for information on how to access the API in python and retrieve quality ratings, refer to the code under 'Getting article quality predictions' in the jupyter notebook referenced above.

### Implementation

All details regarding data curation and analysis of the data, are explained in the jupyter notebook present in the same folder.

### Output tables
**Table-1** 

Shows the 10 highest-ranked countries in terms of number of politician articles as a proportion of country population

| country        | article_proportion (as % of population)          
| ------------- |:-------------:| 
| tuvalu     | 0.55   |
| nauru    | 0.53     |
| san marino | 0.273333     |
| monaco     | 0.1 |
| liechtenstein    | 0.0725     |
| tonga | 0.063      |
| marshall islands     | 0.0616667 |
| iceland    | 0.0515     |
| andorra | 0.0425      |
| federated states of micronesia | 0.038      |

**Table-2** 

Shows the 10 lowest-ranked countries in terms of number of politician articles as a proportion of country population

| country        | article_proportion (as % of population)          
| ------------- |:-------------:| 
|india| 7.19E-05 |
|indonesia|8.07E-05|
|china|8.14E-05|
|uzbekistan|8.81E-05|
|ethiopia|9.77E-05|
|zambia|0.000141243|
|korea,north|0.000152344|
|thailand|0.000169184|
|bangladesh|0.000194111|
|mozambique|0.000196721|

**Table-3** 

Shows the 10 highest-ranked countries in terms of number of GA and FA-quality articles as a proportion of all articles about politicians from that country

| country  | total_article_count | good_article_count | proportion (as percentage) 
| ------------- |:-------------:|:-------------:|:-------------:| 
|korea,north|39|7.0|17.948718|
|saudi arabia|119|16.0|13.445378|
|central african republic|68|8.0|11.764706|
|romania|348|40.0|11.494253|
|mauritania|52|5.0|9.615385|
|bhutan|33|3.0|9.090909|
|tuvalu|55|5.0|9.090909|
|dominica|12|1.0|8.333333|
|united states|1092|82.0|7.509158|
|benin|94|7.0|7.446809|

**Table-4**

Shows 10 lowest-ranked countries in terms of number of GA and FA-quality articles as a proportion of all articles about politicians from that country

| country  | total_article_count | good_article_count | proportion (as percentage) 
| ------------- |:-------------:|:-------------:|:-------------:| 
|sao tome|and|principe|22|0.0|0.0|
|mozambique|60|0.0|0.0|
|cameroon|105|0.0|0.0|
|guyana|20|0.0|0.0|
|turkmenistan|33|0.0|0.0|
|monaco|40|0.0|0.0|
|moldova|426|0.0|0.0|
|comoros|51|0.0|0.0|
|marshall islands|37|0.0|0.0|
|costa rica|150|0.0|0.0|

Note - all the countries in this list have no articles that are classified as high quality.


### Final consolidated data file

The final csv file 'en-wikipedia_article_quality_bycountry.csv' is kept in the same folder as the rest of the files and contains the fields listed below

- country -- name of the country
- article_name -- article name
- revision_id -- last edit id
- article_quality -- quality of the article as returned by ORES
- population -- size of the population in millions.

### Reflection

The thing that stands out for me in terms of possible bias, is the fact that we do not know the source of these articles, as in where the authors actually wrote it from and whether they are citizens of the country the politicians belong to. 
In the absense of this information, it is hard to glean too much from information about the article and the population of the country it belongs to, as it might never have originated from that country in the first place.

Lets start by looking at the results from the first table above, which lists the top 10 countries having the most number of articles when compared to its population. 
Its not surprising to see almost all the countries on this list are small in size, infact Tuvalu has a population of only 10000, as per the data. So it is definitely surprising that there were 55 articles written about their politicians, when in all likelihood, they probably have less than 50 people governing them. 

Likewise the 10 lowest ranked countries in this category, are being led by countries having the largest populations like India and China. One reason this could be the case is because the data only looks at English wikipedia and folks in these countries probably prefer using local dialects. Also the fact that China is not a democracy, could curtail the amount of articles coming out of there, especially about their politicians.

Perhaps the most surprising detail though, is the fact the North Korea leads the pack in terms of high quality articles written about their politicians. This is most likely the work of Americans, considering the amount of interest and the popularity of anything related to North Korea. 


 
 

  

