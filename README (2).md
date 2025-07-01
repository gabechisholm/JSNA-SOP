JSNA Dashboard Creation – Standard Operating Procedure
Any issues, please contact gabriel.chisholm@hartlepool.gov.uk
This guide provides step-by-step instructions to create a Joint Strategic Needs Assessment (JSNA) dashboard in Power BI. It covers importing data from Fingertips and LGInform APIs, grouping indicators via SharePoint, setting up dashboard pages, and updating metadata. Follow the steps below in order. The tone is friendly and direct, assuming you have basic familiarity with Power BI.
# Starting the Dashboard Template
Open the Template: Launch Power BI and open the “Dashboards Defaults.pbit” template file. (In Power BI Desktop, you can also go to File → Import → Power BI Template and select this template.)
# Importing Data from Fingertips
This section covers pulling data via API for an entire Fingertips profile.
Edit the Fingertips Query: In Power BI’s Query Editor, locate the dataFT query (which retrieves Fingertips data). Open its Advanced Editor.
Change the Profile ID: On line 2 of the query (the line beginning with Source = Csv.Document(...)), replace the existing profile ID with the ID of the desired Fingertips profile. Then click Done to apply the change.
Profile ID list available here (consult the Fingertips API documentation for a list of profile IDs).
Apply the Query: After updating the profile ID, allow the query to refresh. The data for the selected profile will be loaded into the model.
Notes: Some Fingertips profiles may not return any data (for example, profile ID 36 for Mental Health returns no data). Also, rank data is already included in the Fingertips API response, so you do not need to calculate rankings separately for Fingertips indicators.
# Importing Data from LGInform
This section covers pulling data via the LGInform API. It involves selecting indicators, constructing an API query, and loading both the indicator data and ranking data.
1. Gather Indicator Identifiers: Use the LGInform data portal or indicator lookup file to find the indicators you need for your dashboard.
Open the LGInform Indicators Lookup file (an Excel or tool provided for LGInform).
Use the slicers or search function to filter indicators by keyword (e.g., topic-related keywords).
From the filtered results, copy all values from the “Identifier” column – these are the unique IDs for the desired indicators.
2. Set Up the API Query: In the LGInform API query builder (web interface or tool):
Set [Authorisation mode] to “Public Private Key.” When prompted, choose “Use my key and secret.” (This will use your LGInform API credentials. Note: Using your key and secret generates a unique signature for your query.)
Paste the copied indicator IDs into the [Metric type(s)] field (this field accepts one or multiple metric/indicator IDs).
For [Areas], specify the geographic areas to include. For example, enter:
E06000001 – Hartlepool (local area)
E12000001 – North East (region)
E92000001 – England (national)
(You can include any area codes needed; the above are common examples.)
For [Period(s)], enter “Latest 5.” This will retrieve data for the five most recent time periods. (Any number works here; a higher number returns more historical periods but will multiply the number of rows.)
For [Value type(s)], select “Raw values.” (This is the default and usually appropriate, unless you need a different value type.)
For [Rows grouped by], select the following dimensions in this order:
area – to group results by area (e.g., Hartlepool, North East, England)
metric type – to group by indicator/metric
period – to separate data by time period
value type – to distinguish between raw values or other value types
These settings ensure the JSON output is structured in a consistent, hierarchical manner.
3. Get the Data (JSON Link Insertion): Once you have configured the query in the LGInform interface:
Click the option to “Copy a JSON table link to the clipboard.” This will generate a URL that returns the data in JSON format for the chosen indicators, areas, periods, etc.
In Power BI, go back to the Query Editor. Find the query named dataLG (which is set up to fetch LGInform data). Open its Advanced Editor.
On line 3 of the dataLG query (the line containing Web.Contents(...)), paste the copied JSON link inside the quotes of the URL argument. Make sure the URL is enclosed in quotes "".
Click Done to confirm. The dataLG query will now use the new URL and pull in the LGInform data for your selected indicators.
4. Retrieve Ranking Data: LGInform provides ranking information through a separate query. To get the ranking data for the same indicators:
Take the list of indicator IDs you copied earlier (the ones you pasted into [Metric type(s)]). You need to create a single comma-separated string of these IDs (with commas encoded as %2C in the URL).
Concatenate the IDs with the delimiter %2C. You can do this manually (by inserting “%2C” between each ID) or use a tool to join them. (For example, if you have a list of IDs, you could use a text editor or a small script to insert “%2C” between values. In some cases, a co-pilot or macro can help automate this.)
In Power BI’s Query Editor, locate the query named rankingData. Open its Advanced Editor.
In the rankingData query’s URL (it will be similar to the dataLG query URL), insert the concatenated ID string in the appropriate place. Specifically, place the string of IDs after metricType= and before the &area=E06000001 parameter in the URL. Ensure the IDs are separated by %2C with no trailing comma.
After pasting the IDs into the URL, click Done. The rankingData query will now fetch the ranking information for all the specified indicators (typically providing national/regional ranks for the values).
5. Close & Apply: Once you have added both the LGInform data (dataLG query) and the ranking data (rankingData query), click the Close & Apply button in the Power BI Query Editor. This will apply all query changes and load the data into the Power BI data model.
# Grouping Indicators for Dashboard Structure
After loading the data, the next step is to define how indicators are grouped into topics and subtopics in the dashboard. This makes it easier to build the dashboard pages and navigation. The indicator groupings are managed in SharePoint files (or lists) – one for LGInform indicators and one for Fingertips indicators. (Ensure you have access permissions to the SharePoint resources named below.)
LGInform Groups – SharePoint file for grouping LGInform indicator metadata.
Fingertips Groups – SharePoint file for grouping Fingertips indicator metadata.
(These are SharePoint links; you will need the proper permissions to access and edit them.)
Follow the steps below for each data source to populate the grouping files:
## Fingertips Indicator Grouping
Copy Unique Indicators: In Power BI, once the Fingertips data has been loaded, switch to the Data view (table view). Locate the table containing the Fingertips indicators (the data from the dataFT query). Right-click on that table and choose Copy Table.
Paste the copied table into a blank Excel workbook (or another tool of your choice). In that workbook, remove any duplicate rows based on the Indicator ID column. This will leave a unique list of indicators.
From the de-duplicated list, copy the Indicator ID values and their corresponding Indicator Name values.
Open the Fingertips Groups SharePoint file. In this file, find the columns designated for Indicator IDs and Indicator Names, and paste the values you copied into the respective columns. You should now have all relevant Indicator IDs and Names listed.
For each indicator entry, fill in the hierarchy columns to categorize them in the dashboard:
Set the [Parent] field to the dashboard’s main topic that this indicator belongs to. (Typically this is the high-level JSNA topic or theme.)
Set the [Group] field to the page topic – the specific topic of the page within the dashboard. (Often this corresponds to a sub-section or domain within the main topic.)
Set the [Subgroup] field to the page subtopic – a further subdivision if the page has multiple sections. (This often corresponds to sections within the page or segmented views of the data.)
(For example, if the dashboard’s main topic (Parent) is “Health,” a Group might be “Mental Health,” and a Subgroup might be “Adult Mental Health” – depending on how the JSNA topic is structured.)
Leave the [ShortDescription] field blank (if such a column exists). Note: The ShortDescription is not used in the dashboard, so it can remain empty.
Save or submit the changes in the SharePoint file so that the updated grouping information for Fingertips is recorded.
## LGInform Indicator Grouping
Copy Unique Indicators: In Power BI’s Data view, find the table for LGInform indicators (from the dataLG query). Right-click the LGInform data table and choose Copy Table.
Paste this table into a blank Excel workbook and remove duplicate rows based on the metricTypeID column (the unique identifier for each LGInform indicator/metric).
Copy the list of unique metricTypeID values and their corresponding metricTypeLabel (indicator names) from the de-duplicated data.
Open the LGInform Groups SharePoint file. Paste the copied metricTypeID values and metricTypeLabel names into the appropriate columns in this file.
For each LGInform indicator entry, set the hierarchy fields as follows:
[Parent]: the dashboard’s main topic (same approach as for Fingertips – e.g., “Health” if that’s the JSNA topic).
[Group]: the page’s topic within that main topic (e.g., “Mental Health” page, etc.).
[Subgroup]: the subtopic of the page (if applicable, e.g., “Adult Mental Health”).
As with Fingertips, the [ShortDescription] field can be left blank in this file (it’s not used in the dashboard).
Save the changes in the LGInform Groups file to record the grouping information.
Note: After updating the SharePoint grouping files, you will need to refresh the Semantic Model tables in your Power BI dataset that correspond to these groupings. Specifically, refresh the FingertipsIndicatorGroups and/or LGInformIndicatorGroups tables (whichever are used in the dashboard’s data model) so that the newly added group information is pulled into the Power BI report. This ensures your dashboard pages can use the updated Parent/Group/Subgroup data for filtering and navigation.
# Page Setup in the Dashboard
With data loaded and indicators grouped, the next step is to set up the various pages of the JSNA dashboard. The template should already contain placeholder pages for the main menu, introduction, context, methodology, indicator details, and help sections. We will customize each of these pages with the relevant information for your specific JSNA topic. Follow the guidance for each page below.
## Main Menu Page
The Main Menu page contains navigation buttons to all sections of the dashboard. You will need to rename and possibly add section buttons here.
Replace the Placeholder Title: On the main menu, there may be a placeholder text (e.g., a button labeled “PLACEHOLDER”). Rename this to an appropriate section title for your dashboard. For example, if your JSNA topic is "Health and Wellbeing," the section title might be "Health & Wellbeing Overview." This title should reflect the overall topic of the dashboard.
Adding New Section Buttons (if needed): If your dashboard requires additional sections (beyond the placeholders provided), you can add new navigation buttons:
Copy and paste an existing section button on the main menu to create a new button.
With the new button selected, go to the formatting pane: Format (Visual) → General → Properties → Position. Set the X (horizontal) position to 42 (this aligns it with the other buttons on the left side).
Set the Y (vertical) position so that the new button is placed 35 pixels below the previous button. For example, if the last existing section button had a Y position of 410, set the new button’s Y position to 445. This ensures equal spacing between menu buttons.
Update the label of the new button to the name of the new section/page it will link to (e.g., “Demographics” or whatever your new section is called).
Sub-menus: The template supports nested sub-menu buttons (for sections that have sub-sections). This is an advanced feature. (Detailed instructions for creating sub-menus can be provided later, if needed. For now, note that sub-menu functionality exists but may require additional setup.)
Reposition Static Buttons: The main menu might have some static buttons at the bottom (such as “Tutorial”, “Help & Support”, etc.). If you added new section buttons, move these static buttons further down so there is a consistent 35-pixel gap below the last section button. In practice, select each static button and increase its Y position value so that it sits 35 points below the new last section. This keeps the layout neat.
Set Navigation Actions: For each new section button you added (and for any placeholders you renamed), make sure the button’s action is configured to navigate to the correct page in the report. In Power BI, a button can be assigned to page navigation via the Action settings. Set the Type to Page navigation and select the corresponding page for that section. Verify that clicking each button takes you to the intended page in the dashboard when in Reading (or Presentation) mode.
## Introduction Page (Sub_Sec-Introduction)
This page is meant to give an overview of the JSNA topic and what the dashboard covers.
Populate Introduction Text: Obtain the introduction text from the JSNA documentation or web page related to your topic. Often, this is available on the Hartlepool Borough Council (HBC) website as part of the published JSNA report. Copy the relevant introductory paragraphs from the existing JSNA page.
Paste into the Dashboard: In Power BI, click on the Introduction page. There should be text boxes ready for content. Paste the copied introduction content into the text box (or boxes) on this page. Make sure the formatting is clean and matches the dashboard style (e.g., font size, color consistent with other text).
Review and Edit: Read through the text on the page to ensure it fits well. You might need to do minor edits for formatting or length, but avoid changing the meaning. The introduction should provide context and explain what the topic is about and why it’s important, just as it does on the JSNA web page.
## Context Page (Sub_Sec-Context)
The context page typically includes background information, definitions, and related topics for your JSNA subject.
Replace Placeholder Text: The template likely contains placeholder text like “TOPIC-NAME” and “ASSOCIATED-TOPICS.” Update these placeholders with real content:
Replace “TOPIC-NAME” (in the first paragraph of the context page) with the actual name of your topic. For example, if the topic is Smoking in Adults, put that in place of the placeholder. This paragraph usually introduces the context of the topic.
In the section where related or associated topics are discussed (often in the fourth paragraph), replace “ASSOCIATED-TOPICS” with actual topics that are associated with your main topic. For instance, if the main topic is Obesity, associated topics might be Physical Activity and Nutrition. List the relevant topics that link to or impact your main topic.
Review Content: Ensure that any other context information on the page (which might have been part of the template) is still accurate for your topic. Adjust any phrasing if necessary so that the context is clear and specific to your JSNA.
## Methodology Page (Sub_Sec-Methodology)
The methodology page describes how data was gathered, processed, and what definitions or calculations are used.
Update Topic Name in Text: In the template’s methodology text, find any placeholder for the topic name (for example, in the fifth paragraph there might be a phrase like “the TOPIC-NAME data was collected…”). Replace “TOPIC-NAME” with your actual topic. This ensures the methodology references the correct subject.
Check Methodology Details: Verify that the methodology described is applicable to the data you have used. If the template provides generic methodology, make sure to insert any specific details about data sources (e.g., “Data for Smoking prevalence were retrieved from Public Health England’s Fingertips API on [date]”) or calculations (e.g., how rates were calculated, definitions of indicators, etc.) relevant to your dashboard. This ensures transparency about the data.
## Indicator Pages (FTInd_# and LGInd_#)
The template includes one or more Indicator pages – these are the pages that display specific indicator data (charts, values, comparisons) for your topic. There are usually two types of indicator pages in the template: one configured for Fingertips data (FTInd) and one for LGInform data (LGInd). You can duplicate these pages as needed, depending on how many indicators or subtopics you want to show, but each page should be set to one data source type (either Fingertips or LGInform).
For each Indicator page you use or add, perform the following:
Set the Filters for the Page:
At the report level (all pages), there may be a filter on the field [Parent]. Make sure the [Parent] filter is set to your dashboard’s main topic. This ensures that the indicator pages (and potentially other pages) only show data relevant to the overall JSNA topic. (There might be two [Parent] filters listed in the filter pane – one for Fingertips data and one for LGInform data. Use the one appropriate for the page’s data source: e.g., if this is an FTInd page showing Fingertips data, set the Fingertips [Parent] filter.)
At the page level (filters that apply only to this page), set the [Group] filter to the specific page topic. For example, if this indicator page is about Smoking Prevalence, and “Smoking” is considered a Group under the main topic, select Smoking for [Group].
Also set the [Subgroup] filter (this may be presented as a slicer on the page, typically at the top-left). Choose the relevant subtopic (if applicable) for this page. For instance, if the page focuses on Smoking in Adults, you might select the subgroup “Adults.” If there is no further subgroup differentiation, you might leave it at a default or “All” selection.
Note: The filters [Parent], [Group], and [Subgroup] correspond to the hierarchy you defined in the SharePoint grouping files. Because the data model groups items by Parent > Group > Subgroup, you may need to click through a hierarchical slicer to find the right subgroup. In some cases, selecting a subgroup might require expanding the slicer and clicking through three levels to reach the specific item (Parent category, then Group, then Subgroup). This is normal given the grouped structure of assets.
Verify Visuals Update: After setting the filters, the visuals on the page should automatically update to reflect the selected topic and subgroup. On an indicator page, you should see elements such as:
Value Card: displaying the current value of the indicator (for the selected area, e.g., Hartlepool, if that’s the focus).
Polarity Card: indicating the polarity (whether a higher value is “good” or “bad” for this indicator, or perhaps showing a target or benchmark direction).
Rank Card: showing the rank of the local value compared to other areas (e.g., Hartlepool’s rank out of 12 NE authorities or out of 152 nationally, depending on data).
Indicator Slicer: a slicer (likely on the left) that allows switching between different indicators on that page (if multiple indicators share the page). Ensure it’s populated with the correct list of indicators relevant to the page’s topic.
Graph: typically a chart (either line chart or bar chart) showing the trend or comparison for the indicator. For example, a line chart might show the trend over time for Hartlepool vs. region vs. England. A bar chart might show the latest value comparison across multiple areas.
Smart Narrative: a text narrative (auto-generated insights) that might describe key points about the data (ensure it’s refreshed for the data).
Data Link Button: a button (often labeled “Data” or “Download Data”) that links to the data source or a download of the dataset. Ensure this link is still correct or update it if necessary (it might link to the source like Fingertips or LGInform for further data details).
Description Card: a text box providing a brief description of the indicator (what it measures, units, etc.). Update this text if needed to match the indicator.
Look at each of these visuals and confirm they display the expected information for your topic. If something isn’t showing correctly (e.g., the value is blank or the wrong indicator), double-check that the filters (Parent/Group/Subgroup) are set correctly and that the underlying data includes those fields.
Multiple Indicator Pages: If your dashboard needs more indicator detail pages than the template provided (for example, you have several subtopics each needing its own page), you can duplicate the FTInd or LGInd pages as needed. After duplicating a page, you must set up new bookmarks and adjust navigation for the forward/back buttons on those pages. The template uses bookmarks for toggling between a line chart view and a bar chart view on the indicator page. If you add pages, you’ll also need to ensure the navigation (usually arrows or next/previous buttons) is updated so users can move through the indicator pages in sequence. This process of adding new pages and updating their bookmarks/navigation is documented elsewhere (this is covered here in additional documentation). If you’re unsure, refer to that guide or ask a colleague for help with bookmarks.
Chart Toggle (Bookmarks): Each indicator page in the template typically has a toggle between two chart types (for example, a Line Chart view and a Bar Chart view). This is implemented using bookmarks in Power BI. The page actually has both a line chart and a bar chart overlaying each other, and the bookmark button switches which one is visible. You do not need to change this unless you want a different behavior. Just be aware: the page consists of two bookmarks – one shows the line chart and the other shows the bar chart. The toggle (maybe a button or a tab) will allow users to switch the visual display. When you duplicate pages, ensure to duplicate the bookmarks as well or adjust them for the new page.
## Help and Support Page (Sub_Sec-HelpSupport)
This page provides contact information, report details, and release notes for users of the dashboard. You should customize all the placeholder information on this page to match your project:
Contact and Report Details: There will be a list of fields or text placeholders to update. Go through each of the following and replace with the correct details for your dashboard:
YOUR-EMAIL: Replace this with the contact email address for the person or team responsible for the dashboard. This is typically who users should contact for support or questions. For example, it might be your own email or a team mailbox.
Report Name (TOPIC): The report name might be displayed like “JSNA Dashboard – TOPIC”. Replace the word “TOPIC” with your actual topic name. For instance, “JSNA Dashboard – Mental Health” if that’s the topic. Do not remove other parts of the name; just swap out the placeholder.
Created Date (CREATE-DATE): Update this to the date the dashboard was created (or the date of publication). Use a clear format (e.g., “June 2025” or “06/06/2025”) as required.
Created by (YOUR-NAME): Put your name (or the name of the person who created the dashboard) here. This can be an individual or a department, depending on your organization’s preference.
Dataset(s): List the data sources or dataset names that the dashboard uses. For example, you might write “Fingertips – Public Health Profiles API; LGInform – Local Government Inform data.” You can also mention any specific dataset or file names if relevant. This helps users understand where the data is coming from.
Acknowledgements: If there are any acknowledgements to add (such as recognizing contributors, data providers, or partner organizations), add them in this section. If none are needed beyond the standard info, you can leave any extra placeholder text empty or remove it.
Release Notes Section: The Help & Support page includes a Release Notes area. This section is designed to inform users about the version history of the dashboard (updates, changes, etc.). In the template, the release notes might be hidden behind a button or toggle (often implemented via a bookmark that shows/hides the notes). You should update the release notes text to include at least an “Initial Release” entry with the date of the first publication. For example, you might add a bullet or line: “Initial Release – 06 June 2025”. If this is the first version of the dashboard, detailed release notes beyond the initial release are not necessary. Just ensure the initial release date is recorded here.
Example of a Release Notes section with an "Initial Release" entry. The release notes can be kept brief — listing the release date and any key updates. In this example, only the initial release is noted, as detailed version history is not required for a new dashboard.
(The release notes in the dashboard operate via a bookmark that shows or hides the notes text. Test the “Release Notes” toggle on the page to make sure it reveals your updated notes. If you add future updates, you can append them here with dates.)
## Help and Support Page – Internal (Sub_Sec-HelpSupportINTERNAL)
If your JSNA dashboard is initially being developed or circulated internally (not yet public), there might be a separate internal version of the Help & Support page. This internal page is meant to be used only while the dashboard is not public-facing. It typically contains the same fields as the regular Help page, but might be used to display different contact info or notes during development.
Usage Note: Do not use the internal Help page once the dashboard goes public. In the main menu, there might be a button that currently navigates to this internal help page (perhaps labeled “Help & Support” while in draft mode). Once you are ready to publish the dashboard publicly, you should switch that main menu button to point to the standard Help and Support page instead of the internal one. Essentially, the internal page is a temporary placeholder.
Customize Internal Page Details: If you are using the internal page for testing or internal review, update it just as you did the standard Help page:
YOUR-EMAIL – your contact email or team email for internal feedback.
Report Name (replace TOPIC) – the topic name in the title.
Created Date – set the creation date.
Created by – your name or team name.
Dataset(s) – data sources used.
Acknowledgements – any acknowledgements for the internal version (often this may be the same as the public one, or you might acknowledge internal stakeholders).
Essentially, fill in all the same placeholders on this internal help page so it’s not showing dummy text.
Release Notes on Internal Page: Just like the public Help page, include the “Initial Release” date in the release notes section. The internal page’s release notes operate the same way (with a bookmark toggle). You typically don’t need different content here compared to the public page – it can mirror the information. The internal page in the template likely says “See screenshot in section above for an example,” referring to the example on the public Help page. This means you don’t have to duplicate the screenshot or content; just ensure the release notes info is updated here as well.
Note: Once the dashboard is ready for a public audience, remember to remove or replace any navigation pointing to the internal help page. The main menu’s Help button should direct all users to the normal Help and Support page (with your contact info, etc.). The internal page can be kept in the file (maybe for future internal use) but should not be accessible in the published public version.

By following all the steps above, you should have a fully functional JSNA dashboard tailored to your topic. The data will be loaded from Fingertips and LGInform as needed, indicators will be grouped for navigation, and each page (Main Menu, Introduction, Context, Methodology, Indicators, Help) will be customized with the appropriate content. Remember to save your Power BI file and test the dashboard thoroughly: click through all page buttons, verify all slicers and filters work, and ensure the information displayed is correct and up-to-date.