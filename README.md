# Overview

**Stakeholder**

It is a climate-and-energy nonprofit team that needs us, as data analysts, to decide where to decide where to send community-protection and climate-support efforts.

**What does each column actually hold, and what data type is it?**

Each column holds values pertaining to its name.
Country column holds names of countries, which are strings.
Iso_code column holds the name of countries iso_code, which are strings.
Year column holds the year the data was collected, which is a string.
Population column holds the population of each country, which are integers.
Gdp column holds the gdp of each country, which are integers.
Co2 column holds the co2 of each country, which are floats.


**Is this dataset structured or unstructured?**

This dataset is structured because not only does it has rows and columns, the same data type is in each column.

**Is there anything about the data — missing values, suspicious rows — that should make someone cautious before using it to decide which communities need support?**

There are missing values across the iso_code column, the gdp column, and the population column. There are suspicious rows like why is the continent  seperated from their countries.


**What's one specific question you'd ask the data's source before trusting it enough to act on?**

There are many missing rows of data, is this data just unavaiable or was it intentionally hidden?

**What's something about energy and emissions you're personally curious to explore in this data?**

I want to see how population affects co2 levels and how gdp affects co2 levels. Does a higher population yield higher co2 levels? Does a richer country yeild higher co2 levels?

I also want to compare energy and emissions across two opposing countries like Pakistan and India, Russia and Ukraine, China and Taiwan. Whether they are currently at war, had hostile relations in the past, or there is just rooted problems across the two countries.

# Source and License

**Source**

Github repo  
Rosado, P., et al. Data on CO2 and greenhouse gas emissions by Our World in Data. GitHub, 2026, https://github.com/owid/co2-data.

Co2 Data   
Hannah Ritchie, Pablo Rosado, and Max Roser (2023) - “CO₂ and Greenhouse Gas Emissions” Published online at OurWorldinData.org. Retrieved from: 'https://ourworldindata.org/co2-and-greenhouse-gas-emissions' [Online Resource]

Population Data  
Edouard Mathieu and Lucas Rodés-Guirao (2022) - “What are the sources for Our World in Data's population estimates?” Published online at OurWorldinData.org. Retrieved from: 'https://archive.ourworldindata.org/20260910-004756/population-sources.html' [Online Resource] (archived on September 10, 2026).


**License**

https://creativecommons.org/licenses/by/4.0/
