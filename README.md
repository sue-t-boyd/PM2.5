# Wildfire Pollution Health Impacts
Analysis of PM2.5 Pollution Near Kearney Nebraska - Prepared for UW MSDS 512 Human Centered Data Science

## SECTIONS IN THIS README FILE
- [Project Purpose](https://github.com/sue-t-boyd/PM2.5/edit/main/README.md#project-purpose)
- [Contents of the Repo](https://github.com/sue-t-boyd/PM2.5/edit/main/README.md#contents-of-this-repro)
- [Data Sources](https://github.com/sue-t-boyd/PM2.5/edit/main/README.md#data-sources)
- [Known Data Issues](https://github.com/sue-t-boyd/PM2.5/edit/main/README.md#6--summary-of-results)
- Intermediate and Final Data Files
- [Results](https://github.com/sue-t-boyd/PM2.5/edit/main/README.md#6--summary-of-results)
- Repo Steps 

## PROJECT PURPOSE
Smoke from landscape fires – including both “wildfires” and prescribed burns – contributes to fine particulate matter pollution (PM2.5). Such pollution, in turn, increases asthma-related ER visits, hospital admissions, and mortality. This project investigates the impacts of wildfire smoke on the residents of Kearney, Nebraska. The project first examines wildfire activity over the past several decades near Kearney Nebraska and concludes that fire activity is increasing.  It then estimates the level of PM2.5 pollution that can be specifically attributable to landscape fires, as  opposed to other causes of PM2.5 pollution, leveraging data and methods by O’Dell et. al. described herein. Using the fire-specific estimates of PM2.5 pollution, the project then estimates the historic asthma-related ER visits, asthma-related hospital visits, and all-cause mortality among Kearney residents attributable to fire pollution. It also predicts how those values will change over time. It also places those findings in context by comparing premature mortality due to wildfire with other causes of premature mortality relevant to the residents of Kearney, Nebraska. 

**PLEASE NOTE**
There are two preliminary steps you MUST take to use the code in this repository.  First, download the Wildfire.Zip module available in the [Data 512 course materials](https://drive.google.com/drive/folders/1OJktGAx86hvMtirCUkGnS292r-FpPvLo). These files provide a Reader object used to parse the raw data files for the wildfire dataset. Depending on where your download is pointed and your system settings, you may need to update the path variables in the code in the Process_Data.ipbny Notebook to ensure that these files are accessible to your Jupyter Notebook.  

Second, you must request a password and username to access the EPA Air Quality System API. Follow the instructions in the “Get Air Quality Data.ipbny” notebook to obtain these credentials and update the relevant fields in the Get_Air_Quality _Data_From_EPA.ipbny notebook.

## CONTENTS OF THIS REPRO 

  - Folder: Data     
      - Folder: Pop_Raw 
        - us_pop_2000_2010.xls
        - us_pop_2010_2020.csv
      - Folder: Other_Mortality_Raw
        - NCHS_-_Leading_Causes_of_Death__United_States (1).csv 
      - Folder: Wildfire Attributes Raw
        - Wildland_Fire_Combined_Dataset.json
        - USGS_Wildland_Fire_Combined_Dataset.csv
        - Wildfire_short_sample.json
      - Folder: KrigedPM25_Raw
          - krigedPM25_2006_v2.nc
          - krigedPM25_2007_v2.nc 
		     . . .
        - krigedPM25_2019_v2.nc
      - Folder: HIA_Constants_Raw
          - HIA_Constants.xlsx
      - Folder: Intermediate_Data_Files
          - Folder: Pop_And_Mortality
             - Kearney_Pop.csv
             - us_pop_2000_2020.csv
             - Other_Cause_Mortality.csv
          - Folder: Fire_Attributes
            - fires_distance_2.csv  # Note that the numbering starts at 2!
            - fires_distance_3.csv
            . . .
            - fires_distance_27.csv
            - fire_dat_merged.csv
          - Folder: Kriged_O’Dell_Data
            - ODell_Smoke.csv
            - Annual Summary PM25 Data ODell.csv
          - Folder: EPA_API
              - epa_api_pm25.csv 
          - Folder: HIA_Past_And_Future
            - historical_impacts.csv

- Folder: Jupyter_Notebooks
  - 1_Gather_Population_And_Other_Mortality_Cause_Data.ipynb
  - 2_Get_Wildfire_Distance_Data.ipynb
  - 3_Merge_Fire_Attribute_Data.ipynb
  - 4_Visualize_Fire_Attribute_Data.ipynb
  - 5_Read_NCFiles.ipynb
  - 6_Smoke_Pollution_Estimates_3(O_Dell_Data).ipynb
  - 7_Get_Air_Quality_Data_From_EPA_API.ipynb
  - 8_Smoke_Pollution Estimates (ODell v. EPA Raw).ipynb
  - 9_Calculate_HIA.ipynb
  - 10_Predict_Future_HIA_Simple.ipynb
  - 11_Predict_Pollution_From_Fire_Attribute_Data_Final.ipbny

- README.md (This File) 
- LICENSE.txt
- Final Report.docx
      
## DATA SOURCES 

### A] Population Data
US Population data is sourced from the [US Census Bureau](https://www.census.gov/). Thefollowing files were downloaded during the week of November 20, 2023:
 - Intercensal Estimates of the Resident Population by Sex and Age for the United States: April 1, 2000 to July 1, 2010 (US-EST00INT-01)
 - National Population Totals: 2010-2020 (census.gov)
 - Annual Estimates of the Resident Population for the United States, Regions, States, the District of Columbia, and Puerto Rico: April 1, 2010 to July 1, 2019; April 1, 2020; and July 1, 2020 (NST-EST2020)

For each file, I preserved the downloaded data unmodified in the sheet bearing the original file name (e,g “us-est00int-01”, “nst-est2020” ) and created a second sheet called “Data.” The “Data” sheet in each file contained total annual population statistics, manually copy/pasted over from the original dataset. In each file, the sheet “Data” has two columns: (1) “Year” as a str; and (2) “US POP” as a float. When reading in US population data into the notebook “Gather_Population_And_Other_Mortality_Cause_Data,” information was retrieved from the “Data” worksheet. 

For Kearney, census figures for 2000, 2010, and 2020 population figures were sourced from [U.S. Census “Quick Facts”](https://www.census.gov/quickfacts/fact/table/kearneycitynebraska/PST045222#) as well as from summary tables appearing in this [Wikipedia Page](https://en.wikipedia.org/wiki/Kearney,_Nebraska).  

For Nebraska,the 2020 population was manually retrieved from the [US Census Quick Facts website](https://www.census.gov/quickfacts/). 

### B] Other Cause Mortality Data
Data related to premature mortality, by cause, for the state of Nebraska was gathered from various sources. 
- Data on annual traffic fatalities in Nebraska was obtained from the [National Highway Safety Administration]( 
https://www-fars.nhtsa.dot.gov/States/StatesFatalitiesFatalityRates.aspx), visited during the week of November 20, 2023. Relevant values were manually entered into my Jupyter Notebook.
- Data on annual firearms death was manually retrieved from [US gun deaths](usafacts.org), which in turn relies on data from the [CDC WISQARS Data Visualization Tool](cdc.gov).
- Data for homicide rates in Nebraska was likewise sourced from the CDC; values available for 2014-2018 were manually entered into my Jupyter notebook during the week of November 20, 2023.
- Data related to mortality due to heart disease and other leading causes of death in Nebraska was downloaded from the CDC website during the week of November 20, 2023, in a file called “NCHS_-_Leading_Causes_of_Death__United_States (1).csv” available in the raw data folder. This file contains, for each state in the United States and for years 1999-2017, information on leading causes of death.  The fields are: 

  - Year <int>
  - 113 Cause Name <str>
  - Cause Name <str>
  - State <str>
  - Death <int>
  - Age-adjusted Death Rate <float>

I used the absolute deaths in the column “Deaths” and not the Age-adjusted Death Rate to better match the format of my premature mortality estimates, which are also not age-adjusted. Since this file only contained info through 2017, I manually retrieved the value for 2018 from the CDC pressroom.   

### C]  WildFire Attribute Data 
Wildfire Data is obtained from the wildfire dataset aggregated by  the US Geological Survey (“US Wildfire Dataset”). Documentation and data available [here](https://www.sciencebase.gov/catalog/item/61aa537dd34eb622f699df81).   

The file USGS_Wildland_Fire_Combined_Dataset.json was downloaded on October 30, 2001 from the website. It contains data on 135061 fires spanning from 1835 to 
2000. It combines data from 40 different, published wildland fire data sources.  

The data is structured as a JSON dictionary, with each fire listed as a separate feature.  Each feature has specified “attributes” including: 

  - OBJECTID (Str)
  - USGS_Assigned_ID (Str),
  - Assigned_Fire_Type': (Str) 
  - Fire_Year' (Int)
  - Fire_Polygon_Tier': 1,
  - Fire_Attribute_Tiers':(Str),
  - GIS_Acres  (Float) 
  - GIS_Hectares' (Float)
  - Source_Datasets (Str) 
  - Comb_National_WFDSS_Interagency_Fire_Perimeter_History (1) (Str) 
  - Comb_State_California_Wildfire_Polygons (Str)
  - Listed_Fire_Types' (Str)
  - Listed_Fire_Names (Str),
  - Listed_Fire_Codes (Str),
  - Listed_Fire_IDs (Str),
  - Listed_Fire_IRWIN_IDs (Str),
  - Listed_Fire_Dates (Str)
  - Listed Wildfire Discovery Date(s) (Str) 
  - Listed_Fire_Causes (Str) 
  - Listed_Fire_Cause_Class' (Str),
  - Listed_Rx_Reported_Acres (Float),
  - Listed_Map_Digitize_Methods (Str),
  - Processing_Notes(Str),
  - Wildfire_Notice (Str) 
  - 'Prescribed_Burn_Notice (Str) 
  - Wildfire_and_Rx_Flag (Str)
  - Overlap_Within_1_or_2_Flag (Str)
  - Circleness_Scale: (Str) 
  - Circle_Flag (Str),
  - Exclude_From_Summary_Rasters (Str),
  - Shape_Length (Float)
  - 'Shape_Area (FLoat)
  - Geometry (Dictionary)

Within the “Geometry” attribute is a nested dictionary of “rings” or (less commonly) “curvedRings.” As noted below, features with “curvedRing” geometries were discarded.  The “rings” data included a list of lists, each list representing the points on a polygon. The first polygon in the list of polygons is understood to be the largest polygon and was selected for processing.  

The file USGS_Wildland_Fire_Combined_Dataset.csv contains data on the same 135601 fires, and all the attribute fields contained in the JSON file except the  geometry fields.  The USGS_Wildland_Fire_Combined_Dataset.csv is thus a strict subset of the USGS_Wildland_Fire_Combined_Dataset.json data, but was used because the csv format is easier to work with.  Data loaded from the USGS_Wildland_Fire_Combined_Dataset.csv can be easily joined with data loaded from the  USGS_Wildland_Fire_Combined_Dataset.csv via the OBJECTID field.  

Finally, the file Wildfire_short_sample.json contains a small sample of 13 fires, in the same format as the file  USGS_Wildland_Fire_Combined_Dataset.json.  It is used throughout the notebooks to test and illustrate functionality.  

### D] Kriged Air Quality + Smoke Flag Data 2006 - 2018 (O’Dell et. al. data)

To estimate PM2.5 pollution specifically attributable to fire smoke, as opposed to other causes, I follow the data and methods set out in  [O’Dell, K. et al. "Estimated mortality and morbidity attributable to smoke plumes in the United States: not just a western US problem." GeoHealth 5.9 (2021): e2021GH000457](https://agupubs.onlinelibrary.wiley.com/doi/10.1029/2021GH000457). O’Dell et. al. [publish the data](https://doi.org/10.25675/10217/230602) used in their analysis under license terms that allow reuse with attribution. 

O’Dell et al. downloaded annual daily 24-hr average PM2.5 observations from the EPA Air Quality Systems Data Mart, for monitors employing parameter code 88101 ("PM2.5 - Local Conditions") or 88502 ("Acceptable PM2.5 AQI & Speciation Mass”). 

They then “kriged” the data – that is, they applied an inverse-distance-weighted data interpolation method to create daily PM2.5 estimates for each 15km-by-15km grid in the US. They also incorporated smoke plume information from satellite imagery. For each grid cell, and each day in the study period, the authors categorize the kriged PM2.5 concentration as either a “smoke” PM2.5 estimate or a “no smoke” PM2.5 estimate based on whether a smoke plume was present or absent above the relevant grid. More details are available in their ReadMe file.  

The data is provided in 13 files, in netCDF file format, each containing data for one year in the period 2006-2018. Variables that I extract and use in my analysis include:
  - doy - day number for the observations (1-365 or 366)
  - lon - longitude of center point of grid, [degrees]
  - lat - latitude of center point of grid, [degrees]
  - PM25 - 24hr average PM2.5 for each grid cell on each day, [ug/m3]
  - Background_PM25 - no smoke seasonal background PM2.5 for each grid cell on each day, [ug/m3]
  - HMS_Smoke - Smoke flag from the Hazard Mapping System (HMS). 1 = smoke plume, 0 = no smoke plume

### F] Air Quality Data - EPA API Estimates

In addition to the Kriged PM2.5 Estimates from the O’Dell et. al. data, Air Quality Estimates from locations near Kearney, Nebraska were obtained using the EPA Air Quality System API. For each year in the time period studied (1963-2020), I queried the closest air monitoring station that measured particulate matter in either of the two methods relevant to PM2.5:  (These are the same two methods used in the underlying dataset that O’Dell et. al. used to create their kriged estimates). 

- "88101", measuring "PM2.5 - Local Conditions" or
- "88502", measuring "Acceptable PM2.5 AQI & Speciation Mass”

For each year, the search begins with a 50 mile bounding box around Kearney, Nebraska. If no sensors were found within the designated area, the bounding box is increased by 50 mile increments until at least one sensor is found or it is determined that no sensors were available within a 350 mile area. For years prior to 1999, there was no monitoring station within the 350 mile area that collected PM2.5 data, so no data was collected for those years.   

For each year, and for each sensor in the relevant bounding box, data was requested for all available days/readings in the year. 

For each reading, the following fields were extracted: 
  - Date (Str)
  - Local Site Name (Str)
  - Site Address (Str) 
  - State (Str) 
  - County (Str)
  - City (Str)
  - Parameter Name (Str)
  - Units of Measure (Str)
  - Method (Str)
  - Sample Duration (Str)
  - Observation Count (Float)
  - Arithmetic Mean (Float)
  - AQI (Float)

### G] Health Impact Equations and Constants

The Health Impact Equations used to estimate fire related PM2.5 impacts on asthma-related ER visits, asthma-related hospital admissions, and premature mortality rely on a number of empirically estimated constants. These constants were sourced from literature review and consolidated into the raw data file “HIA_Constants.xls”. 

The Health Impact Equations are of a form commonly employed by researchers in this area:   
                                  	
	Δ Event  = Pop*BaselineEvent(1-e- Β_Event*ΔPM2.5)                                                                          	
Where:
  - Pop is the population of the group being studied (here, the residents of Kearney).
  - BaselineEvent is the annual baseline of the event.  For example, BaselineMortality  is estimated  as aprox. 733 per 100,000, meaning that on average 733 of 100,000 people per year die prematurely from all causes.
  - ΔPM2.5 is the average annual increase in PM2.5  due to fires, measured in ug/m3 
  - ΒEvent  is calculated as ΒEvent  = ln (RREvent)/10 and
  - RREvent is the relative risk per increase in 10 ug/m3 PM2.5 concentration.

With one exception, I use the same estimated values as those used by O’Dell et. al., which in turn were based on literature review and their own monte carlo estimations. The exception related to the relative risk ratio for premature mortality, which I source from Ford et. al. which in turn relies on a study by Krewski et. al. 

Baseline Rates Used in Health Impact Estimates

Asthma-Related ER Visits
- Baseline Rate (per 100K people): 625.7
- Source: Supporting Info for O'Dell et. al., ["Estimated Mortality and Morbidity Attributable to Smoke Plumes in the US:  Not Just a Western US Problem", Supplemental Table S2](https://agupubs.onlinelibrary.wiley.com/action/downloadSupplement?doi=10.1029%2F2021GH000457&file=2021GH000457-sup-0001-Supporting+Information+SI-S01.docx). In turn sourced from [HCUPnet, Healthcare Cost and Utilization Project. NIS and NEDS. Agency for Healthcare Research and Quality, Rockville, MD](https://hcupnet.ahrq.gov/) Accessed 28 Jan 2021)

Asthma-Related Hospital Admits
 - Baseline Rate (per 100K people): 129.9
 - Source: Same as for Asthma-Related ER visits 

Premature Mortality (All Causes)
- Baseline Rate (per 100K people): 732.9
- Source: [Supporting Info for O'Dell et. al., "Estimated Mortality and Morbidity Attributable to Smoke Plumes in the US:  Not Just a Western US Problem", Supplemental Table S2.] (https://agupubs.onlinelibrary.wiley.com/action/downloadSupplement?doi=10.1029%2F2021GH000457&file=2021GH000457-sup-0001-Supporting+Information+SI-S01.docx).  (In turn sourced from [Global Burden of Disease Collaborative Network. Global Burden of Disease Study 2019 (GBD 2019) Results. Seattle, United States: Institute for Health Metrics and Evaluation (IHME), 2020] (http://ghdx.healthdata.org/gbd-results-tool), accessed 1 Feb 2021. 


Relative Risk Ratios and Confidence Intervals 
 - Event: Asthma-Related ER Visits
 - RR: 1.07
 - 95% CI RR: (1.04-1.11)
 - Source: Supporting Info for O'Dell et. al, "Estimated Mortality and Morbidity Attributable to Smoke Plumes in the US:  Not Just a Western US Problem", Supplemental Table S2.  (In turn sourced from Borchers Arriagada, N. et. al.,  Association between Fire Smoke Fine Particulate Matter and Asthma-Related Outcomes: Systematic Review and Meta-Analysis. Environmental Research 2019, 179, 108777. https://doi.org/10.1016/j.envres.2019.108777.
- url:  https://agupubs.onlinelibrary.wiley.com/action/downloadSupplement?doi=10.1029%2F2021GH000457&file=2021GH000457-sup-0001-Supporting+Information+SI-S01.docx

- Event: Asthma-Related Hospital Admits
- RR: 1.08
- 95% CI RR: (1.03 – 1.14)
- Source and url: same as for Asthma-Related ER Visits

- Event: Premature Mortality
- RR: 1.06
- 95% CI RR:(1.04-1.08)
- Source:  Ford. et. al, relying on Krewski, D, et al. “Extended follow-up and spatial analysis of the American Cancer Society study linking particulate air pollution and mortality.” Vol. 140. Boston, MA: Health Effects Institute, 2009.
- url: https://westrk.org/CARBdocs/Krewski_052108.pdf




## 4. KNOWN DATA ISSUES 

### Known Issues Re: Population Data
The US Census is performed every ten years, thus annual figures are estimates. For the US, annual estimates are provided by the US Census Bureau. For Kearney Nebraska, I estimated annual historical data using a smooth compound annual growth rate between each census period. Likewise, Kearney population or future years was estimated by growing the 2020 population at the same CAGR as population growth from 2010-2020. 

### Known Issues Re: Other Cause Mortality Data
Data on premature mortality was not available for Kearney, Nebraska specifically. Hence, estimates for Kearney were based on totals for Nebraska, scaled by population of Kearney as a proportion of the population of Nebraska.To the extent Kearney residents are less/more likely to experience each of these causes of premature death, these estimates may be inaccurate. In addition, although the data in each case was retrieved from reputable government sources, the data may have unknown limitations that were not documented by the curating source.  Finally, data for homicides in Nebraska was only available for the years  2014-2018, so I took the average value for those years, multiplied by 13, as the estimate for the 13 year period 2006-2018. 

### Known Issues Re: the US Wildfire Dataset
Potential quality issues are described in detail [here](sciencebase.gov/catalog/file/get/61aa537dd34eb622f699df81?f=__disk__d0%2F63%2F53%2Fd063532049be8e1bc83d1d3047b4df1a5cb56f15&transform=1&allowOpen=true).  A summary: 
  - The data was compiled from 40 different published datasets - not all quality issues inherited from source data are fully known. 
  - Some fires may not have been reported in any dataset, especially for older and smaller fires. In particular, years prior to 1980 may underestimate the number of fires. 
  - Polygons are imperfect representations of the true fire boundary. 
  - Fires may have been mislabelled or mapped improperly.  

Helpful things to know for processing 
  - In the JSON File 
    - Each feature geometry listed multiple “rings” (aka coordinates of Polygons).  The first, largest ring was extracted.  
    - The dataset geometries were expressed in the coordinate system 'ESRI:102008. The data was converted to ESPG 4326" to facilitate distance calculations. 
  - In the CSV file (USGS_Wildland_Fire_Combined_Dataset.csv)
    - The CSV file uses “OID_” rather than OBJECTID as used in the JSON file as the index name. 
    - The CSV file contains footnotes, interspersed with data rows,  that need to be removed from the dataset upon loading.  This was achieved in the Merge_Data notebook by removing “OID” values that were character strings length 8 or longer.
    - Upon loading, the OID_ and Fire_Year and columns contained mixed type, strings and integers.  These were coerced to numeric. 
Filtering the dataset
  - Of the 135601 in the overall dataset, 119030 were in 1960 or later.  (1960 was used as a rough initial cut-off, adjusted later to 1963 - see below).   
  - Of the 119030 fires from 1960 or later, 35 expressed their geometry as a “curvedRing” rather than a ring.  “curvedRing” is not recognized by geoPandas. These values were dropped from the dataset. 
  - Of the 118995 fires remaining after curvedRings and early fires were dropped,  there were 16725 out of the 1250 mile range surrounding Kearney, Nebraska, resulting in 102770 remaining fires.
  - Of the 102270 fires left, there were 1298 between 1960 and 1963, these were also dropped, resulting in a final data set of 100972 fires. 

### Known Issues Re: Kriged Air Quality + Smoke Flag Data 2006 - 2018 (O’Dell et. al. data)
O’Dell et al. reported that 0.2% of the full (all US) dataset contained instances of negative PM2.5 readings. Negative readings are not physically possible and thus point to potential issues with the underlying observations or the Kriging methods. O’Dell et. al. recommend discarding the negative values, however, no negative total PM2.5 values were observed in the vicinity of Kearney, Nebraska so this was not an issue in my filtered dataset. As discussed below, however, Kearney did exhibit negative fire related PM2.5 values.
O’Dell et al. note as a further limitation that they used the already kriged data for all monitors and only separate smoke days and non-smoke days in the kriged dataset. As they summarize, “[t]his allows a smoke-impacted monitor to influence a non-smoke day in a nearby location and artificially increase the non-smoke PM25 background.” 
O’Dell et. al. recommend (in their ReadMe file) to estimate fire-related smoke impact on a particular day by subtracting the “non-smoke PM2.5 background” from that days total PM2.5 reading, where “non-smoke PM2.5 background” is their calculated variable representing seasonal median PM2.5  among non-smoke days.  Applying this methodology to the grid relevant to Kearney resulted in a significant number of negative estimates of smoke pollution. I addressed this by using average annual estimates.  
Using the presence or absence of a smoke plume on a particular day to create a binary classification of “smoke day” or “non-smoke day” assumes that smoke pollution only lingers for one day after the presence of a smoke plume. This may underestimate lingering impacts.

### Known Issues Re Raw EPA PM2.5 estimates using the API
There were no nearby monitoring stations prior to 1999. No data is collected for these years. However, beginning in 1999, monitoring stations were always available within a max 100 mile bounding box.  
Available stations changed over time, with some closing and others coming online. For all years except 2004, each year’s data comes from a single site (2 in 2004), but the site changed over time. This required re-searching the area around Kearney Nebraska on a regular (annual) basis to identify stations. It also impacts the quality of  the PM2.5 estimates because the stations are at somewhat varying distances away from Kearney.
The frequency of the observations is low and variable. For most years there are between (70 and 120) observations, with significantly more (>8K) beginning in 2020. The sparse data in most years (roughly 5-10 per month) may well have missed periods of intense fire activity entirely.  
The sample collection durations vary. For all years 2019 and prior, a 24 Sample Duration is used. Beginning in 2020, there are at least two and possibly three durations used (1 Hour, 24 Hour, and 24-HR BLK Avg).  However, sample duration variance seems unlikely to unduly bias the data.  

### Known Issues Re Health Impact Assessment Constants
Constants used in the Health Impact Assessment equations are sourced from empirical studies referenced in the literature. Each of those empirical studies has its own assumptions and data limitation for estimating the relevant risk ratios from PM2.5 pollution. Following the methodology of O’Dell et. al. I use, where available, estimates from a meta-analysis of available relevant studies, which is thought to be the most precise estimates available. Nonetheless, the fact that different studies result in somewhat differing estimates and confidence intervals is testament to the fact that the estimates are imperfect.  

## 5.INTERMEDIATE AND FINAL DATA FILES 
 
### US Population
The file us_pop_2000_2020.csv contains estimates of the US population by year from 2000 to 2020, merged from the raw data files downloaded from the US Census Bureau. The file contains two fields, each as an integer: (1) Year; and (2) US POP. 

### Kearney Population
The file “Kearney_Pop.csv” contains estimated historical and predicted future population of Kearney, Nebraska, from 2000 to 2045. The file contains two fields, each as an integer: (1) Year; and (2) POP. 

### Other Cause Mortality
The file “"Other_Cause_Mortality.csv" contains two fields: (1) “Cause” as a string  and (2) Deaths as a float. The file summarizes estimated premature mortality among residents of Kearney, Nebraska or each of the four listed causes for the period 2006-2018.  

### Distance Files
The group of files named “fires_distance_{batch_no}.csv contain, for fires in 1960 or later, the following fields: (1) OBJECTID (as a string); (2) Distance to Kearney (as a float); (3) Closest Long (as a float) and (4) Closest lat (as a float).  The distance to Kearney represents the distance between the center of Kearney and the closest point of the relevant fire, as calculated in the notebook Get_Wildfire_Distance.ipbny. Closest long and Closest lat are, respectively, the longitude and latitude of the closest point on the fire to Kearney.  

### Fire Data Merged
The file “fire_dat_merged.csv" contains the combined distance data from the individual “fires_distance_{batch_no}.csv files joined with selected fire attribute data from USGS_Wildland_Fire_Combined_Dataset.csv.  After dropping data related to (1) fires prior to 1963; (2) fires further than 1250 miles from Kearney, Nebraska, and (3) fires that had a “curvedRIng” geometry, there were a total of 100972 fires.  The file contains the following fields: 
  - OID_ <class 'str'>
  - Assigned_Fire_Type <class 'str'>
  - Fire_Year <class 'str'>
  - GIS_Acres <class 'str'>
  - Listed_Fire_Names <class 'str'>
  - Listed_Fire_Dates <class 'str'>
  - OBJECTID <class 'str'>
  - Distance to Kearny <class 'str'>
  - Closest long <class 'str'>
  - Closest lat <class 'str'>
  - Log_GIS_Acres <class 'str'>

### Fire Related Smoke Estimates (O’Dell Data)
The file “ODell Smoke.csv” contains daily readings of PM2.5 in the grid cell closest to Kearney, Nebraska as well as a variable “HMS_Smoke” flag indicating whether smoke  was observed (value = 1) or not (value = 0) above Kearney on that day.  It also contains the “Backgroudnd_PM25” estimate used by O’Dell et. al. in their analysis (but which I ultimately don’t use), and some indicator fields for convenience in later processing.  Fields are:
  - Date <str>
  - PM25 <float>
  - HMS_Smoke <int>
  - Background_PM25 <float>
  - Month <str>
  - Month Number <int>
  - Year<int>
  - Fire_Season <bool>

The file “Annual Summary PM25 Data ODell.csv” contains annualized estimates of smoke impact 2006-2018.  Fields are: 
  - Year <int>
  - Mean PM25 Smoke Days <float>
  - Mean PM25 Non-Smoke Days <float>
  - Diff in Mean PM25 <float>
  - Num Smoke Days <int>
  - Avg. Annual Delta_PM25 <float>
  - Annual Mean Combined <float>

### EPA_API_PM25 Data
The file “epa_api_pm25.csv” contains data obtained from the EPA Air Quality Systems API. It contains, for each year from 1999 to 2023, each available daily air quality reading, with the following fields: (1) Date as a str; (2) Local Site Name as a str; (3) Site Address as as str; (4) State as a str; (5) City as a str; (6) Parameter Name as a str; (7) Units of Measure as a str; (8) Method as a str; (9) Sample Duration as a str; (10) Observation Count as an int; (11) Arithmetic Mean as a float; (12) AQI as an int; (11) Month as a str; (12) Month Number as an int; and (13) Year as an int. The file contains 4445 observations total. 

### Historical HIA estimates. 
The file "historical_impacts.csv" contains a summary of the estimates, and 95% confidence intervals, by year, for each of the health impacts studied: asthma-related ER visits, asthma-related hospital visits, and premature mortality.  The (1)“Year” field is an integer and the remaining columns are all recorded as a float: (2) Excess Asthma ER Visits; (3) ER Visits CI Lower; (4) ER Visits CI Upper; (5) Excess Asthma Hospital Admits; (6) Hospital CI Lower; (7) Hospital CI Upper; (8)Excess Mortality; (9) Mortality CI Lower; (10) Mortality CI Upper. The file contains 13 rows total (14 including the header row).  
     
## 6.  SUMMARY OF RESULTS 

This study concludes that wildfire activity near Kearney, Nebraska – and the related PM2.5 pollution associated with fires – is on the rise. During the period from 2006 to 2018, fire related pollution likely contributed to approximately 5 premature deaths, 5 additional asthma-related ER visits, and 1 additional asthma related hospital admissions. Further, the relative health impact of fire related PM2.5 pollution is likely to increase in future years as wildfire activity continues to trend upward. Although the health impacts from fire pollution are not as significant as other public health concerns such as heart disease, they are nonetheless significant. Any human life lost to pollution is too many. Thus, I am recommending that the residents of Kearney and its city council take the following mitigation measures based on recommendations from Airnow.gov. 

 - For All Residents
 	- Stay indoors during  high exposure days
    	- Use air filters indoors
       	- Create “clean room” for sleeping
- For the City Council
	- Accommodations for the unhoused during high exposure days

Many of these mitigation measures are relatively low cost, as is appropriate given my findings that other public health priorities may be more pressing.  

More detailed discussion of the analysis and findings can be found in the Final Report, a copy of which is in this repo folder.  
    
## 7.  REPRO STEPS

A] Set up your Python Environment. The Jupyter Notebooks in this repository were run using Python version 3.8.16 (default, Mar  2 2023, 03:18:16) [MSC v.1916 64 bit (AMD64)].  
     
B]  Download the Jupyter Notebook files into your working directory.  Download the data files being careful to preserve the file structure. That is, load the entirety of the "Data" folder, complete with the subfolder structure intact, into the same working directory that you loaded the Jupyter notebooks. If you do not do this, you may need to update the file path names in the individual Jupyter notebooks for the code to run properly.  

C] Download the Wildfire.Zip module available in the [Data 512 course materials] (https://drive.google.com/drive/folders/1OJktGAx86hvMtirCUkGnS292r-FpPvLo). Depending on where your download is pointed and your system settings, you may need to update the path variables in the code to ensure that these files are accessible to your Jupyter Notebook. ***THIS REMAINING STEPS WILL NOT WORK IF YOU DO NOT DOWNLOAD THE WIDFIRE.ZIP MODULE AND ENSURE IT CAN BE ACCESSED FROM YOUR PYTHON WORKING DIRECTORY.***  
    
 D] Run notebooks: 

(1)  Run the notebook "1_Gather_Population_And_Other_Mortality_Cause_Data .ipynb." This will result in three output files: (a) "Kearney_Pop.csv"; (b) "us_pop_2000_2020.csv"; and (c) "Other_Cause_Mortality.csv," all as described above. Each will be saved into your working directory, subfolder "Data." 

(2) Run the notebook “2_Get_Wildfire_Distance_Data.ipynb”.  Running this  notebook will save into your working directory, subfolder “Data” each of the  “fires_distance_{batch_number}.csv" files described above. ***RUNNING THIS NOTEBOOK MAY TAKE A WHILE***.  Data is gathered in batches. In the event you encounter a lost connection during run time, you may update the notebook to re-run only those batches of data that were impacted by the lost connection or other run-time error.  

(3) Run the notebook “3_Merge_Fire_Atribute_Data.ipynb.” This will result in the “fire_dat_merged.csv" data file described above, into your working directory, subfolder “Data.” 

(4) Run the notebook “4 _Visualize_Fire_Attribute_Data.ipynb.” This creates figures 1 and 2 in theFinal Report, embedded within the notebook and saved as “Visuals/Figure _{X}.jpg.” 

(5)  Run the notebook "5_Read_NCFiles.ipynb."  This will result in the output file "Odell_Smoke.csv", described above.  It will be saved into your working directory, subfolder "Data."  

(6) Run the notebook "6_Smoke_Pollution_Estimates_3(O_Dell_Data).ipynb"  This will result in the output file “Annual Summary PM25 Data ODell.csv” described above.  It will be saved into your working directory, subfolder "Data."  It also creates figures 4 through 7 in the Final Report, embedded within the notebook, and saved as “Visuals/Figure {X}.jpg.” 

(7A)  Acquire an access token that will enable you to query the EPA Air Quality System API. Follow the instructions in the  Get Air Quality Data.ipynb notebook to acquire the token.  Update the “USERNAME” and “APIKEY” field with your credentials.  

(7B)   Run the  notebook "7_Get_Air_Quality_Data_From_EPA_API.ipynb." This will create and save the file “epa_api_pm25.csv” described above, into your working directory, subfolder “Data.” 

(8)  Run the notebook “8_Smoke_Pollution Estimates (ODell v. EPA API).ipynb" This creates figure 3 in the Final Report, embedded within the notebook and saved as “Visuals/Figure 3.jpg.” 

(9) Run the notebook "9_Calculate_HIA.ipynb" This creates figures 8-10 in the Final Report, as well as Figures A1-A2 and Tables A1-A3  in the Sensitivity Analysis Appendix to the Final Report. Figures embedded within the notebook and also saved as “Visuals/Figure {x}.jpg.”  The notebook creates and saves the file “historical_impacts.csv” described above, into your working directory, subfolder “Data. 

(10) Run the notebook “10_Predict_Future_HIA_Simple”.  This creates figures 11-12 in the Final Report, embedded in the notebook and saved as “Visuals/Figure {X}.jpg.” 
 
(11) Run the notebook “11_Predict_Pollution_From_Fire_Attribute_Data_Final.ipbny.” This creates the data for Tables 3 and 4 in the Final Report, embedded in the notebook. 


