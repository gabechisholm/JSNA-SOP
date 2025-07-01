# JSNA Dashboard Creation – SOP (Concise Version)

_Contact: gabriel.chisholm@hartlepool.gov.uk_

## 1. Start with the Template

Open `Dashboards Defaults.pbit` or use **File > Import > Template**.

## 2. Fingertips Data Setup

- Go to `dataFT > Advanced Editor`.
- On line 2, change the **ProfileID** in `Source = Csv.Document(...)`.
- Apply changes. _(See Fingertips ProfileID list if needed.)_

**Note:** Rank data is included in the Fingertips API. Some profiles (e.g. ID 36) return no data.

## 3. LGInform Data Setup

### a) Get Indicator IDs

- Open **LGInform Indicators Lookup**.
- Filter with slicers.
- Copy values from **"Identifier"**.

### b) Build Query

- Use **Public Private Key** authorisation.
- Paste IDs into `[Metric type(s)]`.
- Set:
  - `[Areas]`: `E06000001`, `E12000001`, `E92000001`
  - `[Period(s)]`: Latest 5
  - `[Value type(s)]`: Raw values
  - `[Rows grouped by]`: area, metric type, period, value type

### c) Insert Query in Power BI

- Copy **JSON table link**.
- In `dataLG > Advanced Editor`, paste link into line 3 inside `Web.Contents(...)`.

### d) Ranking Data

- Concatenate indicator IDs using `%2C` (e.g. via Copilot).
- In `rankingData > Advanced Editor`, paste into `metricType=` section of URL.
- Click **Close & Apply** when done.

## 4. Group Indicators (via SharePoint)

### Fingertips

- Copy table from Power BI → Paste into Excel → Remove duplicates (`IndicatorID`).
- Add IDs and names to **Fingertips Groups** SharePoint.
- Set: `[Parent]`, `[Group]`, `[Subgroup]`. Leave `[ShortDescription]` blank.

### LGInform

- Same as above, using `metricTypeID` and `metricTypeLabel`.
- Add to **LGInform Groups** SharePoint.

**Reminder:** Refresh `FingertipsIndicatorGroups` and/or `LGInformIndicatorGroups` in the semantic model.

## 5. Page Setup
### Main Menu

- Replace `"PLACEHOLDER"` titles.
- To add sections: Duplicate button, set `X=42`, increase `Y` by 35 each time.
- Update button navigation.

### Introduction Page

- Copy/paste intro from the JSNA website.

### Context Page

- Replace `"TOPIC-NAME"` and `"ASSOCIATED-TOPICS"` with relevant content.

### Methodology Page

- Replace `"TOPIC-NAME"` where needed.

### Indicator Pages (`FTInd_#` / `LGInd_#`)

- Only use one data source per page.
- Set report-level filter: `[Parent] = your topic`.
- Page filter: `[Group]`, slicer: `[Subgroup]`.
- Check visuals update:
  - Value card
  - Rank
  - Polarity
  - Graph
  - Slicer
  - Description
  - Smart Narrative
- Pages use bookmarks to switch between Line and Bar charts.

### Help & Support Page

Update:
- `YOUR-EMAIL`
- `TOPIC`
- `CREATE-DATE`
- `YOUR-NAME`
- `Dataset(s)`
- Add "Initial Release" date to release notes.

### Help & Support (Internal)

- For draft-only dashboards.
- Swap for standard Help page when going public.

## Final Check

- Ensure all filters, visuals, buttons, and bookmarks work.
- Check that all navigation buttons work as intended.
- Confirm contact info and release notes are accurate.
- Publish when ready.
