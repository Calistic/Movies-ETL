# Movies-ETL

## Summary
- Movie with most 0.5 star reviews: Syriana, qty. 1061 

- Movie with most 5.0 star reviews: The Million Dollar Hotel, 45511

## Assumptions
- Kaggle data should take precedent over Wikipedia data
- If Kaggle data is empty for runtime, budget, and/or revenue, then Wikipedia data should fill in the missing data (if it exists)
- The vast majority of budget and revenue entries are entered as one of the following formats
  - $123.4 million/billion
  - $123,456,789
  - A range, using one of the formats above
- The vast majority of release dates are entered as one of the following formats
  - YYYY
  - MONTH DD YYYY
  - YYYY MM DD
  - MONTH YYYY
- The vast majority of the Adult column is FALSE
  - If the adult column has many TRUE or null entries, our list will be drastically shortened
- Runtime does not include seconds. All entries are in units of hours, hour+minutes, or minutes.

## Transformation Psuedocode
	Wiki
		Create movie dictionary
			filter for director, directed by, imdb_link, and not no. of episodes
		Clean movie dictionary of alternate titles and merge duplicate columns
		Transform movie dictionary into df
		remove movies with bad imdb_id and duplicates
		remove columns that are more than 90% null
		Box office Transformation
			remove null entries
			convert any lists to strings
			Use regex to extract entries like $123.4 million/billion, $123,456,789, and replace ranges with higher value
			parse box office entries, save to box_office in df
			delete original box off column
		Budget Transformation
			Delete null entries
			convert any lists to strings
			remove ranges, keep higher end
			remove citations
			parse budget entries, save to budget in df
			delete original budget column
		Release Date
			remove null entries
			convert any lists to strings
			extract dates with known date types
			convert to datetime
		Running Time
			remove null entries
			convert any lists to strings
			convert hours to minutes and save to df
			delete original running time column
	Metadata
		Adult
			Save false to df
		Video
			Save true to df
		Transform Columns to correct data type
			Convert budget to int
			Convert id to numeric
			Converto popularity to numeric
			Converto release date to datetime
	Ratings
		Timestamp
			Convert to datetime
		Merge with wiki df
			Release Date
				find outlier between wiki and ratings and delete
		Drop unwanted columns
		Move data from wiki to ratings if ratings is empty
			runtime, budget, box office/revenue
	Formatting
		Reorder and rename Columns
			Omit video column
		Group rating data by movie and rename userID to count. Then rearrange columns to show counts as rating values
		rename rating columns for better understanding
		merge rating df with wiki df
		replace nans with 0s
## Load Pseudocode
	Get sql pw
	create engine
	connect to server
	delete existing data
	save movies df to existing sql table
 	save ratings to existing sql table
