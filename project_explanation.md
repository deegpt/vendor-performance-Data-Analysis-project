### Data Ingestion

SQLite has been used to ingest data from CSV files to the database as a table.
Python scripting is used, so as to refresh the data in the DB in a scheduled timeframe and inegsted automatically.
**Things to remember:** When scripting is done, everything needs to be inside a function and logging should be added to monitor the warnings and errors in a specific step during the program execution. Logging also helps to determine the steps completed already and steps left.
