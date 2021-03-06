Any publication resulting from the use of this work must cite the following publication::

Benjamin D. Horne, William Dron, Sara Khedr, and Sibel Adali. "Assessing the News Landscape: A Multi-Module Toolkit for Evaluating the Credibility of News" WWW (2018).

# The News Landscape (NELA) Toolkit

The News Landscape (NELA) Toolkit is built using Flask, Javascript, and PostgreSQL.

The project is currently being served using NGNIX and Gunicorn at **nelatoolkit.science**. 

Under “Check a News Article" users can provide a url to a news
article or manually enter news article text. The tool then performs
several predictions on the article: reliability, political impartiality,
title objectivity, text objectivity, and several online community
interest predictions. Each of these predictions is displayed as a
probability and each article with associated predictions are entered
into a table. As more article entries are provided, this table can be
sorted and filtered by different predictions using the table filters
menu at the top of the page. Further, more details about the article
and analysis of the article can be found by clicking on the entry
in the table. The ultimate goal of this page is to allow journalist
and information analyst to quickly filter articles down to ones that
need to be fact-checked or are of interest.

Under “Compare News Sources" users can explore and compare
a variety of news sources using content-based features. Specifically,
users can select multiple features, sources, and a time range to
visualize on a 2-dimensional scatter plot. For example, a user can
select “reading complexity" for the x-axis and “negative sentiment"
for the y-axis using the chart setting menu on the left side of the
page. They can then select any number of sources from our data set
and a data range over which to explore. The tool will then generate
a scatter plot of the selected sources for comparison. If a user wants
more details about a source, they can double-click the source bubble
in the scatter plot. This detailed page will show source metadata,
credibility predictions, and Facebook engagement over time. These
details can also be found on the “View All Sources" page.

## Using the toolkit locally

### PostgreSQL

To support the tool's back-end, [PostgreSQL](https://www.postgresql.org/) must be downloaded. In PostgreSQL, create a database with any name (e.g. NELA). You will need to create a database credentials JSON file named **dbCredentials.json** outside of the cloned directory. The format for the JSON file is shown below:

    {
    	"host":"",
    	"user":"",
    	"passwd":"",
    	"db":"",
    	"port":""
    }

### Python Dependencies

To run the tool, [Python 2.7](https://www.python.org/downloads/) must be downloaded.  

The easiest way to download all necessary Python packages is using [pip](https://pypi.python.org/pypi/pip). To do so, navigate to the project root directory and run:

	pip install -r requirements.txt

## Loading Data

To load the data into the newly created PostgreSQL database, navigate to the /dbSetup directory in the project directory. Extract the contents of the data.zip folder in the /dbSetup directory. Then, run:

	python load.py

## Running The Tool

Within the root project directory, run:
	
	python app.py

## Project Data Details

The toolkit is divided into two main parts: Check a News Article and Compare News Sources.

### Check a News Article
#### classifiers/resources/

This folder contains the pre-trained machine learning models for predicting reliability, bias, subjectivity, and community interest. 

#### features/resources/

This folder contains all the needed lexicons, pre-trained models, and features selection lists for the feature computation and selection portion of the toolkit. 

### Compare News Sources
#### dbSetup/data.zip

This folder contains the data which is loaded into the tool's database. For each CSV, the data labels and types can be found by referring to its .sql file located under dbSetup/sql. 

**_articleFeatures.csv_**: Contains all computed features for each article in the dataset; it also includes the article's published date and its news source 

**_articleMetadata.csv_**: Contains the link and full title for each article within the dataset 

**_sourceMetadata.csv_**: Contains a credibility and impartiality score for each news source, along with a flag indicating if the source is self-identified as satire

**_topSourcePhrases.csv_**: Contains the top ten phrases for each news source, for every month (identified by a month number and year); sources that have more than 1 phrase but less than 10 for a given month will have a "NULL" field for phrases not contained

#### static/js/sourceData.js

Additional news source metadata is stored here (e.g. source's display name, date founded, etc). Each news source has an associated image, which is located in the /static/newsSourceImages directory.

A portion of this data is shown below for the source AP:

	"AP":
	{
		"name": "Associated Press (AP)",
		"type": "Radio, television, and online",
		"country": "United States",
		"founded": "May 22, 1846",
		"website": "ap.org",
		"imageName": "AP.jpg"
	}

#### static/js/featureData.js

The mapping between each of the article's computed features and its display name on the tool is stored here. Each name aims to be concise, while providing more information on the nature of the feature.

This file also stores the top ten features, which are the initial options in the tool-set.

A portion of this data is shown below:

	{
	...
	    "Average Word Length": "wordlen",
        "Word Count": "WC",
        "Probability of Objectivity": "NB_pobj",
        "Probability of Subjectivity": "NB_psubj",
        "Quote Usage": "quotes",
	...
	}


 


	 






 

