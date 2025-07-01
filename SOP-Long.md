# JSNA Dashboard Creation – Standard Operating Procedure

_Any issues, please contact **gabriel.chisholm@hartlepool.gov.uk**_

This guide provides step-by-step instructions to create a **Joint Strategic Needs Assessment (JSNA) dashboard** in Power BI. It covers importing data from **Fingertips** and **LGInform** APIs, grouping indicators via SharePoint, setting up dashboard pages, and updating metadata. Follow the steps below in order. The tone is friendly and direct, assuming you have basic familiarity with Power BI.

## Starting the Dashboard Template

**Open the Template:** Launch Power BI and open the **“Dashboards Defaults.pbit”** template file.  
(In Power BI Desktop, you can also go to **File → Import → Power BI Template** and select this template.)

## Importing Data from Fingertips

This section covers pulling data via API for an entire Fingertips profile.

**Edit the Fingertips Query:**  
In Power BI’s **Query Editor**, locate the `dataFT` query (which retrieves Fingertips data). Open its **Advanced Editor**.

**Change the Profile ID:**  
On line 2 of the query (the line beginning with `Source = Csv.Document(...)`), replace the existing profile ID with the **ID of the desired Fingertips profile**. Then click **Done** to apply the change.  
_Profile ID list available **here** (consult the Fingertips API documentation for a list of profile IDs)._

**Apply the Query:**  
After updating the profile ID, allow the query to refresh. The data for the selected profile will be loaded into the model.

**Notes:**  
Some Fingertips profiles may not return any data (e.g., profile ID 36 for Mental Health).  
Also, **rank data is already included** in the Fingertips API response, so you do not need to calculate rankings separately for Fingertips indicators.

## Importing Data from LGInform

This section covers pulling data via the **LGInform API**. It involves selecting indicators, constructing an API query, and loading both the indicator data and ranking data.

### 1. Gather Indicator Identifiers
Use the LGInform data portal or indicator lookup file to find the indicators you need for your dashboard.

- Open the **LGInform Indicators Lookup** file.
- Use the slicers or search function to filter indicators by keyword (e.g., topic-related keywords).
- From the filtered results, copy all values from the **“Identifier”** column – these are the unique IDs for the desired indicators.

### 2. Set Up the API Query
In the LGInform API query builder:

- Set **[Authorisation mode]** to **“Public Private Key.”**
- Paste the copied indicator IDs into the **[Metric type(s)]** field.
- For **[Areas]**, specify:
  - `E06000001` – Hartlepool
  - `E12000001` – North East
  - `E92000001` – England
- For **[Period(s)]**, enter **“Latest 5.”**
- For **[Value type(s)]**, select **“Raw values.”**
- For **[Rows grouped by]**, select:
  - `area`
  - `metric type`
  - `period`
  - `value type`

...

