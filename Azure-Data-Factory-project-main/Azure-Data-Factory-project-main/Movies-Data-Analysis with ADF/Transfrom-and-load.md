# Transforming Data with Mapping Data Flow

Now that you have moved the data into Blob, you are ready to build a Mapping Data Flow which will transform your data at scale via a spark cluster and then load it into a Data Warehouse. For more information on Mapping Data Flows.


1.Turn on Data Flow Debug If you haven't already done so, turn the Data Flow Debug slider located at the top of the authoring module on. Data Flow clusters take 5-7 minutes to warm up and users are recommended to turn on debug first if they plan to do Data Flow development. For more information
![dataflow1](https://github.com/RaghucharanV/Azure-Data-Factory-project/assets/81848656/5b543fc3-6227-4fdf-9f40-47ed6ca7bb94)

![Screenshot (795)](https://github.com/RaghucharanV/Azure-Data-Factory-project/assets/81848656/5c922b29-d817-4a05-8afe-306fccce7758)


2.Add an Blob source Open the Data Flow canvas. Click on the Add Source button in the Data Flow canvas. Name your source Movies. In the source dataset dropdown, select your blob dataset used in your Copy activity.

![Screenshot (795)](https://github.com/RaghucharanV/Azure-Data-Factory-project/assets/81848656/5aaa65b0-2085-41d4-af3b-d42bd81c012f)

After a few seconds, you should see a schema appear below.

You can verify your source is configured correctly via the Data Preview tab. Click the Refresh button. Mapping data flow will use the debug cluster to pull a snapshot of your data at that specific transformation
![Screenshot (797)](https://github.com/RaghucharanV/Azure-Data-Factory-project/assets/81848656/1382ce83-a47a-4231-8db9-918f64577e38)


3.Add a Select transformation to rename and drop a columnYou may have noticed that the Rotton Tomatoes column is misspelled. To correctly name it and drop the unused Rating column, you can add a Select transformation by clicking on the + icon next to your ADLS source node and choosing Select under Schema modifier.


![dataflow5](https://github.com/RaghucharanV/Azure-Data-Factory-project/assets/81848656/d83f85ea-4801-474d-93a2-6f65a94fb389)

Give your Select transformation a identifiable name like SelectAndRename. In the Name as field, change 'Rotton Tomato' to 'Rotten Tomato'. To drop the Rating column, Click on the trash can icon next to the column name
You should now have 5 remaining columns.
![Screenshot (798)](https://github.com/RaghucharanV/Azure-Data-Factory-project/assets/81848656/abe203c2-a11c-4cdf-bf75-22cf745ca6ff)


You can verify your output using data preview.

![Screenshot (799)](https://github.com/RaghucharanV/Azure-Data-Factory-project/assets/81848656/abdf8830-f046-4f68-89c5-3ed44cd406dc)

4.Add a Filter Transformation to filter out unwanted years Say you are only interested in movies made after 1951. You can add a Filter transformation to specify a filter condition by clicking on the + icon next to your Select transformation and choosing Filter under Row Modifier. Click on the expression box to open up the Expression builder and enter in your filter condition.



<img width="321" alt="filter-settings" src="https://github.com/RaghucharanV/Azure-Data-Factory-project/assets/81848656/04fca90b-3e1e-41fe-9750-1fdb0a682e34">

This will open up the data flow expression builder. The expression builder allows you to construct expressions composed of column values, parameters, functions, operators, and literals. Using the syntax of the Mapping Data Flow expression language, year > 1950 will filter rows where the year column value is greater than 1950.


![Screenshot (801)](https://github.com/RaghucharanV/Azure-Data-Factory-project/assets/81848656/e04eae21-5782-45ec-a538-16982d1808d0)


Data Preview:
![Screenshot (803)](https://github.com/RaghucharanV/Azure-Data-Factory-project/assets/81848656/002670ca-bc78-4187-8bfa-10905b65c978)



4.Add a Derive Transformation to calculate primary genre As you may have noticed, the genres column is a string delimited by a '|' character. If you only care about the first genre in each column, you can derive a new column via the Derived Column transformation by clicking on the + icon next to your Filter transformation and choosing derived column under Schema Modifier.
<img width="331" alt="derive-settings" src="https://github.com/RaghucharanV/Azure-Data-Factory-project/assets/81848656/2fecfd61-44f1-4aea-804e-ef01e3be540a">

Name your transformation `DerivePrimaryGere. Similar to the filter transformation, the derived column uses the Mapping Data Flow expression builder to specify column values. In the left textbox selector, enter Primary Genre. Click on the expression box to assign the new column a value. The expression builder will open.
In this scenario, you are trying to extract the first genre from the genres column which is formatted as 'genre1|genre2|...|genreN'. While there are a variety of ways to do this, one way is using the split() which converts a delimited string into an array. split() takes in two parameters, the string to split and the delimiter. In this case, the string is the column genres and the delimiter is the character '|'. We are only interested in the first element of the output array which can be accessed using bracket notation, [1]. The finished expression is split(genres, '|')[1].

![Screenshot (804)](https://github.com/RaghucharanV/Azure-Data-Factory-project/assets/81848656/261c510a-152d-4d62-a62c-987ee7a08e08)




![Screenshot (805)](https://github.com/RaghucharanV/Azure-Data-Factory-project/assets/81848656/f5bd7182-653a-44a1-99d7-8bc879bbfdd2)

5.Rank movies via a Window Transformation Say you are interested in how a movie ranks within its year for its specific genre. You can add a Window transformation to define window-based aggregations by clicking on the + icon next to your Derived Column transformation and clicking Window under Schema modifier.
<img width="311" alt="window-settings" src="https://github.com/RaghucharanV/Azure-Data-Factory-project/assets/81848656/8b9796fb-4e3f-4f4c-b305-d7b6e226302a">

Name your window transformation RankMoviesByRatings. First select your group by columns to specify which rows fall into each window. The two group by columns will be PrimaryGenre and year. To add a group by column, click on the plus icon next to any existing group by column.
![Screenshot (806)](https://github.com/RaghucharanV/Azure-Data-Factory-project/assets/81848656/5ce90155-8b7d-4fd3-9475-bee084e075b8)

Next, go to the Sort tab and specify a sort order for each window group. Because we are interested in ranking the highest rated movies according to Rotten Tomato first, the sort column is Rotton Tomato and the order is descending.
The Range by tab allows you to specify your window bounds. Keep the default setting of Unbounded selected.

In the Window columns tab, specify new columns to generate based upon your windows. Add a column RatingRank that has a value of rank(). rank() is a unique window function that returns the rank of a row in its window according the the sort order specified in the Sort tab.


![Screenshot (808)](https://github.com/RaghucharanV/Azure-Data-Factory-project/assets/81848656/0ac819ef-2815-4326-a5c1-ed113118381e)


![Screenshot (807)](https://github.com/RaghucharanV/Azure-Data-Factory-project/assets/81848656/152992bf-d248-4c1c-862a-846decb4b553)

6.Aggregate ratings with an Aggregate Transformation Now that you have gathered and derived all your required data, we can add an Aggregate transformation to calculate metrics based on a desired group by clicking on the + icon next to your Window transformation and clicking Aggregate under Schema modifier.


<img width="341" alt="aggregate-settings" src="https://github.com/RaghucharanV/Azure-Data-Factory-project/assets/81848656/a1c74e64-13b4-4933-98e1-6442813f2bc9">
Name your aggregate transformation AggregateRatings. As you did in the window transformation, select PrimaryGenre and Year as group by columns.
In the Aggregates tab, you calculate aggregations over specified group by columns. For every primary genre and year, lets get the average Rotten Tomatoes rating, the highest and lowest rated movie (utilizing the windowing function) and the number of movies that are in each group. As the data was sorted in the window transformation, you can use the first() and last() functions to get the highest and lowest rated values. Create four columns with the following names and expressions:
![Screenshot (810)](https://github.com/RaghucharanV/Azure-Data-Factory-project/assets/81848656/72601c9e-bca5-44cb-bc0a-7f04d619bd68)

![Screenshot (809)](https://github.com/RaghucharanV/Azure-Data-Factory-project/assets/81848656/a6553565-6962-4b2b-b6e0-28aed8f42ebb)

![Screenshot (811)](https://github.com/RaghucharanV/Azure-Data-Factory-project/assets/81848656/40d154df-bfea-4f22-9914-28b5a014015b)

![Screenshot (812)](https://github.com/RaghucharanV/Azure-Data-Factory-project/assets/81848656/ecea9e20-3d33-4124-80bd-a6e660c68852)
7.Add sink as a ADLS at end of Data Flow pipeline
![Screenshot (817)](https://github.com/RaghucharanV/Azure-Data-Factory-project/assets/81848656/95836aeb-d370-4222-8730-4b448d9035fb)

![Screenshot (820)](https://github.com/RaghucharanV/Azure-Data-Factory-project/assets/81848656/a1ac96c4-6871-4b3e-a6e1-fb92ef8544f7)

![Screenshot (822)](https://github.com/RaghucharanV/Azure-Data-Factory-project/assets/81848656/8fce82fb-dc27-4611-a55e-9be97db098f3)

![Screenshot (818)](https://github.com/RaghucharanV/Azure-Data-Factory-project/assets/81848656/3c36962c-f70b-43f4-a963-9b42925c5f5c)


# Running the Pipeline


Go to the pipeline canvas. In the Execute Data Flow activity's settings tab, you will see the data flow selected and the compute environment used. In this lab, you should use the default 4 core general purpose cluster. Since you are writing to Synapse, PolyBase staging is enabled by default. This allows for efficient bulk loading into Azure Synapse Analytics. To use this feature, you must specify a staging storage account. In the Staging linked service dropdown, select the ADLS linked service you created earlier. For Staging storage folder, select sample-data for the container and staging for the folder path.

![Screenshot (819)](https://github.com/RaghucharanV/Azure-Data-Factory-project/assets/81848656/0623edee-03ed-4d95-832f-4d1e90379bcf)

This is the end of pipe the transformed data is loaded in a ADLS and future used in BI insights using Power BI




