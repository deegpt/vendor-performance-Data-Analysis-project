### Data Ingestion

- SQLite has been used to ingest data from CSV files to the database as a table.
- Python scripting is used, so as to refresh the data in the DB in a scheduled timeframe and inegsted automatically.

**Things to remember:** When scripting is done, everything needs to be inside a function and logging should be added to monitor the warnings and errors in a specific step during the program execution. Logging also helps to determine the steps completed already and steps left.

- Moved all the functions together in one cell and put them into a .py file which is called the 'Python Scripting'

---

### Exploring data
- Exlpore the ingested data into the database tables to find what type of values are there and what values and columns are releated to our business problem that needs to be extracted 
in the final dataframe.
- As per the **business problem**, we need to analyse the **vendor performance**, **pricing strategies** and the **inventory**. Hence, we need to explore the columns related to these pointers.
- After performing EDA, cleaned the data in the existing columns and new features were introduced relevant to the business problem needs.
- For pushing the final data for analysis into the database, created an empty table in the DB using SQL query and then inserted the data into it.
- To perform the clean data ingestion in the DB, written a Python Script.
