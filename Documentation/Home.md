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

1. **Choose a subject**
   - [Look at wikipedia to find enough data](https://github.com/Erulan123/Mayors/blob/main/Documentation/wikipedia_mayors_links.md)
     
2. **Define the research problem**
   - [Formulate guiding research questions inspired by prosopography](https://github.com/Erulan123/Mayors/blob/main/Documentation/Problematic-Questioning.md)

3. **Identify and collect data** 
   - [Use **SPARQL queries** to extract structured data from Wikidata](https://github.com/Erulan123/Mayors/blob/main/Documentation/SPARQL/SPARQL_data_exploration.md)
   - [Save raw data](https://github.com/Erulan123/Mayors/blob/main/Data/mayors_core.csv)

4. **Clean Data**
   - [Harmonize variables](https://github.com/Erulan123/Mayors/blob/main/Jupyter_notebooks/mayors_data_cleaning.ipynb)
   - [Save cleaned data set](https://github.com/Erulan123/Mayors/blob/main/Data/mayors_cleaned.csv)

5. **Perform temporal analysis**
   - [Track representation and dominance across historical periods](https://github.com/Erulan123/Mayors/blob/main/Jupyter_notebooks/mayors_qulitative_time.ipynb)

6. **Conduct exploratory analysis**
   - [Descriptive statistics on gender, party, and age (Chi², Cramer’s V)](https://github.com/Erulan123/Mayors/blob/main/Jupyter_notebooks/mayors_bivariate_analysis.ipynb)  

7. **Explore educational backgrounds**
   - [Find new data via **SPARQL**](https://github.com/Erulan123/Mayors/blob/main/Documentation/SPARQL/SPARQL_data_exploration.md)
   - [Save new education data](https://github.com/Erulan123/Mayors/blob/main/Data/mayors_edu_long.csv)
   - [Build network representations linking mayors and institutions](https://github.com/Erulan123/Mayors/blob/main/Jupyter_notebooks/mayors_edu_network.jpynb)

8. **Conclusion**
   - [Answer the questions and summarise results](https://github.com/Erulan123/Mayors/blob/main/Documentation/results.md)
