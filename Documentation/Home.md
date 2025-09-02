# Home Page

Welcome to the **U.S. Mayors** project.  
This project uses a prosopographical approach to analyze the collective biographies of mayors across American cities. By combining socio-demographic, political, and educational data, it aims to highlight shared trajectories, institutional affiliations, and their evolution over time.

---

## Table of Contents

1. [Introduction](https://github.com/Erulan123/Mayors/blob/main/README.md)
2. [Research Problem](https://github.com/Erulan123/Mayors/blob/main/Documentation/Problematic-Questioning.md)
3. [Data Collection](https://github.com/Erulan123/Mayors/blob/main/Documentation/SPARQL/SPARQL_data_exploration.md)
5. [Methodology](https://github.com/Erulan123/Mayors/blob/main/Documentation/Home.md)
6. [Findings](https://github.com/Erulan123/Mayors/blob/main/Documentation/results.md)
7. [References](https://github.com/Erulan123/Mayors/blob/main/Documentation/wikipedia_mayors_links.md)

---

## Project Workflow

The project followed a simple, structured workflow:

1. [**Define the research problem**](https://github.com/Erulan123/Mayors/blob/main/Documentation/Problematic-Questioning.md)
   - Formulate guiding research questions inspired by prosopography.  

2. [**Identify and collect data**](https://github.com/Erulan123/Mayors/blob/main/Documentation/SPARQL/SPARQL_data_exploration.md)
   - [Find data in wikipedia](https://github.com/Erulan123/Mayors/blob/main/Documentation/wikipedia_mayors_links.md)
   - Retrieve biographical, political, and educational information.  
   - Use **SPARQL queries** to extract structured data from Wikidata.  

3. [**Clean and prepare datasets**](https://github.com/Erulan123/Mayors/blob/main/Jupyter_notebooks/mayors_data_cleaning.ipynb)
   - Harmonize variables (gender, parties, universities, time periods).
   - Save [cleaned data set](https://github.com/Erulan123/Mayors/blob/main/Data/mayors_cleaned.csv)
   - Create derived fields (age groups, parties, periods).  

4. [**Perform temporal analysis**](https://github.com/Erulan123/Mayors/blob/main/Jupyter_notebooks/mayors_qulitative_time.ipynb)
   - Track representation and dominance across historical periods.

5. [**Conduct exploratory analysis**](https://github.com/Erulan123/Mayors/blob/main/Jupyter_notebooks/mayors_bivariate_analysis.ipynb)
   - Descriptive statistics on gender, party, and age.  
   - Bivariate tests (Chi², Cramer’s V) to assess associations.

6. [**Explore educational backgrounds**](https://github.com/Erulan123/Mayors/blob/main/Jupyter_notebooks/mayors_edu_network.jpynb)
   - Identify most influential universities.  
   - Build network representations linking mayors and institutions.  
