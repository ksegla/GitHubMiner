# GitHubMiner

The GitHub APIs allow software professionals and researchers to explore and gather information on GitHub projects. However, they come with a number of limitations -- token rate limits, query limits (no more  than 1000 results) etc. -- that make them hard to use for large scale mining. GitHubMiner is a tool, built around the GitHub GraphQL API, intended to ease the extraction of data from GitHub, starting with project data. It allows you to easily retrieve information about repositories based on the provided search query, date range, and other parameters. 
Additionally, it provides a simple heuristic to retrieve topics associated to a repository, based on a dictionary which by default is a set of tags taken from Stack Overflow.

## Prerequisites

Before running the script, make sure you have the following dependencies installed:

- **graphql-request**: To make GraphQL API requests
- **fs**: To handle file operations
- **minimist**: To parse command-line arguments
- **csv-writer**: To write data to CSV files

You can install these dependencies using npm:

`npm install graphql-request fs minimist csv-writer`.


## GitHub GraphQL API
To fully understand how to structure queries and make the best use of the available features, refer to the [GitHub GraphQL API Documentation](https://docs.github.com/en/graphql).


## Usage

To use the script, follow these steps:

1. Replace the tokens in the **tokens**  array with your own GitHub personal access tokens. These tokens are used to authenticate API requests; the tool rotates between them to avoid rate limits. The idea here is to pool tokens from different developers working on your project.
2. Set the desired **batchSize** to control the number of results fetched in each API request. The GitHub GraphQL API sometimes returns errors when the batch size is high. That is why our batch size is 10 by default but you can try to change it to higher numbers (max 100) 
3. Adjust the delay time in the **fetchResultsBatch** function if necessary to avoid rate limits between batches.
4. Modify the **writeFiles** function to format the data according to your desired output.
5. Replace the **dictionary.json** file with your own dictionary, if desired.
6. Run the script with the following command:

`node index.js --query "your search query" --start "start date" --end "end date" --date "date type" --filename "output filename" --batchsize 10`

## Placeholders

- **query**: The search query to filter repositories. For example, "mobile AND (android OR ios)".
- **start**: The start date of the search range in YYYY-MM-DD format. By default, it is set to 2013-01-01.
- **end**: The end date of the search range in YYYY-MM-DD format. By default, it is set to the current date. 
- **date**: The type of date field to search on. It can be "created", "pushed", or "updated". By default, it is set to "created".
- **filename**: The base filename for the output JSON and CSV files. By default, it is set to "results".
- **batchsize**: The number of results from a single query. By default, it is set to 10.

  
## Dictionary
The code is designed to handle the use of a dictionary for enhancing the collection of repository topics. If a dictionary is not provided, the code will continue extracting without any issues, ensuring uninterrupted processing. The default **dictionary.json** file included with the code contains a set of common tags for mobile development, but it can be easily substituted as needed.


## Output

The script retrieves the repository information from the search results and saves it in two formats:

- JSON file: The data is saved in a JSON file named `<filename>.json`.
- CSV file: The data is saved in a CSV file named `<filename>.csv`.

The output includes various fields for each repository, such as name, owner, description, URL, creation date, number of users, watchers, stars, forks, projects, issues, pull requests, disk usage, license, languages, primary language, environments, submodules, and topics.

Please note that the script handles pagination and rate limits automatically to fetch all results within the provided date range.

Feel free to customize the script according to your specific requirements and formatting preferences.
