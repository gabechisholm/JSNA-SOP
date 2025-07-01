# JSNA Dashboard Repository

This repository contains DAX measures and documentation used in the development of the Joint Strategic Needs Assessment (JSNA) dashboard. The dashboard is built in Power BI and integrates data from Fingertips and LG Inform APIs. All measures referenced here are included in the Power BI template and PBIX files (not provided for security reasons).

## 📁 File Structure

### 🧮 DAX Measures

These files contain reusable DAX expressions used in the JSNA dashboard:

- **FingertipsLatestYearValue.dax**  
  Calculates the latest year value for Fingertips data.

- **FingertipsLinkTooltip.dax**  
  Generates tooltips with hyperlinks for Fingertips indicators.

- **FingertipsNarrative.dax**  
  Constructs narrative text based on Fingertips indicator values.

- **FingertipsPercentile.dax**  
  Computes percentile rankings for Fingertips indicators.

- **FingertipsRank.dax**  
  Calculates rank positions for Fingertips indicators.

- **LGInformLatestYearValue.dax**  
  Calculates the latest year value for LG Inform data.

- **LGInformLinkTooltip.dax**  
  Generates tooltips with hyperlinks for LG Inform indicators.

- **LGInformNarrative.dax**  
  Constructs narrative text based on LG Inform indicator values.

- **LGInformRank.dax**  
  Calculates rank positions for LG Inform indicators.

### 📄 Standard Operating Procedures

- **SOP-Short.md**  
  A concise version of the SOP for quickly setting up a JSNA dashboard.

- ~~**SOP-Long.md**  
  A comprehensive guide covering all steps from data import to dashboard publishing.~~ *(Removed – no longer required)*

## 🛠️ Usage

1. Start with the `Dashboards Defaults.pbit` Power BI template.
2. Use the SOP documents to guide data import, indicator grouping, and page setup.
3. Apply the DAX measures to generate values, rankings, and narratives.
4. Customize the dashboard pages using the provided structure and bookmarks.

## 📬 Contact

For questions or support, please contact:  
**gabriel.chisholm@hartlepool.gov.uk**

---

_This repository supports the development of JSNA dashboards for Hartlepool Borough Council and related public health initiatives._
