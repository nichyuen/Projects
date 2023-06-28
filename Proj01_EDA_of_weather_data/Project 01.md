### Problem Statement

In 2018, Singapore generated around 20.42 billion US dollars in the tourism sector alone. This corresponds to 5.1 percent of its the gross domestic product and approximately 14 percent of all international tourism receipts in Southeast Asia.

Singapore Tourism Board(STB) is interested to understand factors that could influence tourist receipts to design strategies to bring in more tourists in the midst of recovery from the COVID pandemic. An executive have been tasked to analyse the significance of climate and economic factors on the amount of tourist receipts.

Singapore has typical tropical climate with abundant rainfall, high and uniform temperatures and high humidity all year round, since its situated near the equator. The climate in Singapore influence the type of tourist activities they participate thus STB can strategise to cater to their needs.

STB seek to understand the significance of economic factors such as Unemployment rate, Average national income per citizen (GNI per capita) and exchange rate on tourism arrivals so to manage tourist arrival expectations with the study of economic data.

### Data used

* [`rainfall-monthly-number-of-rain-days.csv`](./data/rainfall-monthly-number-of-rain-days.csv)
* [`rainfall-monthly-total.csv`](./data/rainfall-monthly-total.csv)
* Foreign climate data (https://climateknowledgeportal.worldbank.org/download-data)
* Unemployment rate (https://databank.worldbank.org/reports.aspx?source=2&series=SL.UEM.TOTL.ZS&country=MYS,CHN,IDN,IND)
* GNI per capita (https://databank.worldbank.org/reports.aspx?source=2&series=NY.GNP.PCAP.PP.CD&country=MYS,CHN,IDN,IND,AUS)
* Exchange rate (https://databank.worldbank.org/reports.aspx?source=2&series=NY.GNP.PCAP.PP.CD&country=MYS,CHN,IDN,IND,AUS#)

### Approach
 #### Preparation of data
 1. Cleaning of climate (rainfall and temperature) and economic (Unemployment, Exchange rate & GNI per capita) datasets recorded mainly in the period from 2010 to 2020
 2. Formatting data such as renaming column names for merge of data and to annualize certain numeric data
 3. Filtering the top 5 countries of tourist arrivals for analysis(Australia, Indonesia, China, Malaysia, India)
 
 #### EDA
 1. Understanding the distribution of data using visualization
 2. Using scatter plot and heat map to visualize possible relationship between potential factors that influence tourist arrivals
 
### Findings
1. Strong relationship between Tourism arrivals and receipts implying that being able to attract more arrivals will lead to more total expenditure
2. Superficial analysis on factors that might influence tourist arrivals:
     1. **Mean temperature and tourist arrivals**: Relatively no relation displayed
         1. Possible no relation could be due to similar changes to climate between SG and the foreign country which most contries (except for Australia) exhibits especially the asian countries(i.e Malaysia & Indonesia)
     2. **Average annual rainfall and tourist arrivals**: Relatively no relation display between
     (Both analysis display outlier in tourist arrival datasets in year 2020 which is due to COVID traveling restrictions)
     3. **Exchange rates and tourist arrivals**: Relatively no relation display between 
     4. **Unemployment rate and tourist arrivals**: India, Malaysia and China display strong inverse relationship where as Australia display relatively strong inverse relationship and Indonesia have weak inverse relationship
     5. **GNI per capita and tourist arrivals**: Most countries are weak positively related except for Australia that displays weak inverse relationship
 3. **Conclusion**: Climate do not seems to be significant in influencing tourist arrival rates whereas economic factors seem to be more significant, mainly Unemployment rate, though there are mixed results on the type of relationship
### Limitations and future work
#### Limitations
1. Conclusions are drawn from using correlation and identifying possible trends from scatter plots which are superficial
2. Limited availabilty of certain datasets such as the states which tourists belong to are able to provide more meaningful analysis with weather data as climate varies significantly with different geographic locations especially for larger countries such as China
3. Limited time period of data enables analysis within a smaller sample size which might be less representative to the population

#### Future work
1. More detailed data collection on the geography on tourist arrivals
2. Using more sophiscated data analysis model to investigate relationship such as regression
3. Analysis can include more countries to have stronger indication on possible relationship and trends
     

