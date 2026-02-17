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

> Data Filtering

To enhance the reliability of the insights, we removed inconsistent data points where:
- Gross Profit <= 0 (to exclude transactions leading to losses)
- Profit Margin <= 0 (to ensure analysis focuses on profitable transactions)
- Total Sales Quantity = 0 (to eliminate inventory that was never sold)

- It is then followed by a Statistical validation of Profit Margin Differences using **Hypothesis Testing**.
  $H_0$ (Nulll Hypothesis) -> No significant difference in profit margin between top vendors and low-performing vendors.
  $H_1$ (Alternate Hypothesis) -> A significant difference exists in profit margins between two vendor groups.

  **Result:** The null hypothesis is rejected, confirming that the two groups operate under distinctly different profitability models.

  **Implication:** High-margin vendors may benefit from better pricing strategies, while top-selling vendors could focus on cost efficiency.

  ### Final Recommendations
- Re-evaluate pricing for low sales, high margin brands to boost sales volume without sacrificing profitability.
- Diversify vendor partnerships to reduce dependency on a few suppliers and mitigate spply chain risks.
- Leverage bulk purchasing advantages to maintain competitive pricing whileoptimizing inventory management.
- Optimize slo-moving inventories by adjusting purchase quantities, arranging clearance sales or revising storage strategies.
- Enhance marketing and distribution strategies for low-performing vendors to drive higher sales volumes without compromising profit margins.
- By implementing these recommendations, the company can achieve sustainable profitability, mitigate risks and enhance overall operational efficiency. 
