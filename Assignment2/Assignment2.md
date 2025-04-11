# Visual Analytics

## Assignment 2

**Instructor:** Dr. Marco D'Ambros  
**TAs:** Carmen Armenti, Mattia Giannaccari

**Contacts:** marco.dambros@usi.ch, carmen.armenti@usi.ch, mattia.giannaccari@usi.ch

**Due Date:** 28 April, 2025 @ 23:55

---

## Goal

The goal of this assignment is to use ElasticSearch and Kibana to solve different problems. The dataset you need is in the csv named *restaurants.csv*.

## Submission Guidelines

⚠️ Once your solution is ready, please submit a zip file containing the following on iCorsi:

1. A `report` that clearly explains the **steps** you took to arrive at each solution. It should include the code for the query and aggregations. Ensure the queries are properly indented and document each query/aggregation with the number of documents (hits) returned. The report should also present the outputs of the query/aggregation and answer any questions where applicable. Additionally, include screenshots of the canvas and the dashboard, along with the code you wrote for the scripted fields.

2. The `export files` for the dataview, saved search, dashboard, and canvas (one file for each).

3. Any other `scripts` used to clean, ingest, or prepare the data. In the report, please discuss how and why these scripts were used.

### Naming convention
⚠️ Please ensure that the index is called `restaurants`. Also, do not delete or rename any columns in the dataset. If you need to make modifications, only add new columns.

⚠️ Please name the zip file as `SurnameName_Assignment2.zip`, and use the following *naming conventions* for the files contained within the archive:
 - Report: report.pdf
 - Queries export: queries.txt
 - Data view export: view.ndjson
 - Saved search export: saved_search.ndjson
 - Dashboard export: dashboard.ndjson
 - Canvas export: canvas.json


### Additional Information for Exporting the Files
🛟 Instructions for exporting the `queries`:
1. Open the left menu by clicking the hamburger icon in the top-left corner;
2. From the left menu, navigate to the "Dev Tools" page;
3. Ensure that your queries are displayed on the left-hand side of the console;
4. Click the "Export requests" button located at the top-right of the toolbar to export your queries.
	
🛟 Instructions for exporting the `dashboard` and its related `assets` (data view):
1. Open the left menu by clicking the hamburger icon in the top-left corner.;
2. From the left menu, navigate to the "Stack Management" page;\
	ℹ️ The left menu will update once you access this page.
3. Using the updated left menu, navigate to the "Saved Objects" page;
4. Export the following objects that you wish to submit:
	- Dashboard;
	- Saved search for the dashboard table view;
	- Data view, which also includes your scripted fields.\
	⚠️ Please export each object **separately**. To export a single object, select it using the checkbox and click the "Export" button.
	
🛟 Instructions for exporting the `canvas`:
1. Open the canvas;
2. Click "Share" in the top toolbar;
3. Click "Download as JSON".


## Section 1 - Indexing, queries and aggregations (30 points)

### 1. Indexing
Ingest the restaurant dataset provided in CSV format, transforming every row into a JSON document where the name of the fields are the name of the columns in the CSV format. In the report you should briefly explain all the steps you followed to index and import the data in ElasticSearch.

### 2. Queries
1. Get all restaurants which contain the substring 'bar' in the restaurant name but that do not contain neither 'barbecue(s)'/'barbeque(s)' nor 'bar'. Show only the restaurant name, the city, and the number of votes.

2. Which are the 3 most expensive restaurants whose reviews were done in 2016? We are interested in reviews which refer only to places within 20 km from Singapore (1.290270, 103.851959).

3. We would like to get those restaurants that have 'pizza' in the name and not 'pasta'. Get only the restaurants that have been reviewed at least as ‘Good’. Please show only the city, aggregate rating, restaurant name, and number of votes. Please order the results in descending order on the aggregate rating field.

    
### 3. Aggregations

1. Which are the 7 cities with the highest average restaurant price (cost for two)? We are interested in cities which have not less than 10 restaurants and restaurants that have at least 100 votes.

2. What is the weighted average of aggregate rating for all cities within a 10-kilometer radius from Noida (28.535517, 77.391029)? To calculate the weighted average, please use the votes field.

3.  Show the number of restaurants reviewed as 'Very Good' aggregated according to their votes which are not in Asia. Please consider the following ranges: from 100 to 250, from 250 to 400, from 400 to 550. For each bucket we would like to know the minimum and maximum value of the average cost for 2.
    

💡 For each of the aggregations above, discuss whether you are facing the **shard size** problem. If that would be the case, how would you solve it?


## Section 2 - Data visualization (40 points) 📊
This exercise uses the same dataset as the previous one and is divided into two parts: building a dashboard and creating a canvas.

#### Part 1 - Dashboard creation
The goal of this section is to create a dashboard in Kibana to visualize some information about the restaurants collected in the dataset.
Please create a dashboard that:
1. Shows the following metrics:
	- The total number of restaurants reviewed;
	- The total number of cities covered;
	- The average number of restaurants per city.
2. Provides a filter to choose the restaurant price range;
3. Shows the average rating for each continent;
4. Shows the average cost for two for each continent ;
5. Shows the trend of restaurants review over time. Please:
	- Differentiate between positive and negative reviews. Reviews with ratings greater than 3 and up to 5 are considered **positive**, while those with ratings from 0 up to and including 3 are considered **negative**;
	- Provide a comparison between positive, negative and total reviews.
6. Displays a map showing the location of all the restaurants, with the size of each marker representing the number of votes.
7. Shows a table view with details about every restaurant in the dashboard;\
	ℹ️ This should not be a table view in visualization but a saved search.
8. Displays a heat-map that showing the number of votes for each price range and aggregate rating range.\
	ℹ️ The price range is computed by binning the price field `AverageCostForTwo` into 20 bins.\
	ℹ️ The aggregate rating range is computed by binning the `AggregateRange` field into the following bins: [0-1), [1-2), [2-3), [3-4), [4-5].

#### Part 2 - Canvas creation
The goal of this section is to create a canvas that shows the restaurants rating per city.

Please create a canvas that:

1.  Allows filtering the data based on the city selected;
2.  Shows the number of restaurants for each price category. Please use the following categories:
	- `Cheap` if the price is below 50 (included);
	- `Cheap/Medium` if the price is between 50 (not included) and 100 (included);
	- `Medium` if the price is between 100 (not included) and 250 (included);
	- `Pricy` if the price is between 250 (not included) and 1000 (included);
	- `Super Pricy` if the price is over 1000 (not included);
3.  Shows the percentages of Good, Medium and Bad restaurants. Please use the following definitions:
	- Good if `AggregateRating` is over 4 (not included).
	- Medium if `AggregateRating` is between 1.5 (not included) and 4 (included).
	- Bad if `AggregateRating` is below 1.5 (included).
4.  Shows a table - with 5 rows per page - with the following information about each restaurant:
	- Name;
	- Number of votes;
	- Rating text.
5.  Shows some metrics about the restaurants:
	- Total number of reviews;
	- Highest cost for 2 people;
	- Date of the most recent review.

## Section 3 - Ingestion Plugin (30 points)

The goal of this exercise is to extend ElasticSearch by building an ingestion plugin (https://www.elastic.co/guide/en/elasticsearch/plugins/current/index.html). Ingestion plugins enrich the capabilities of ElasticSearch by providing additional logic at ingestion time.

The ingestion plugin to be implemented is a lookup plugin: given a field to operate on, and a lookup map, when ingesting documents the plugin will replace all instances of the keys with the values of the provided lookup map, in the context of the given field. The idea is to, for example, replace product codes with product names, or replace id-emails (used in some universities) with the corresponding name-emails.

> **Example** \
> If my documents are about cars and I know that code `C001` means `tyre`, when ingesting a document I want to replace all occurrences of `C001` with the term `tyre`, so that a document with a field that is "*Need to optimize the **C001** temperature*", would be indexed as "*Need to optimize the **tyre** temperature*".

To implement this plugin, you are provided with a skeleton repo, containing all the scaffolding and structure needed to create an ingestion plugin. You need to add/fill the classes as needed and implement tests. The skeleton repo is available on [GitLab](https://gitlab.com/usi-si-teaching/msde/2024-2025/visual-analytics/elasticsearch-plugin/ingest-lookup-stub).

Make sure that the plugin works as expected by:
1. Installing it on your local instance of elasticsearch;
2. Setting up a pipeline;
3. Ingesting a document;
4. Retrieving the document checking that the text was replaced as expected.

The `README.md` file includes instructions on how to do that.

