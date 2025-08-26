## Explore Wikidata

In this notebook we will inspect and find relevant data on US mayors via Wikidata SPARQL endpoint
### Inspect CEOs 

```sparql
## Count the total number of ceos
SELECT (COUNT(*) as ?eff)
WHERE {
    ?item wdt:P31 wd:Q5;  # Any instance of a human.

        wdt:P106 wd:Q30185  # Occupation: Mayor
}  
#LIMIT 10
```
eff = 6615



