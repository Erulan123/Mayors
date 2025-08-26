## Explore Wikidata

In this notebook, we refine and document the main requests available on the page [Exploration of Wikidata](../documentation/wikidata/Wikidata-exploration.md) 


When you prepare the queries, you can execute them on the Wikidata SPARQL endpoint, and then document and execute them in this notebook.

Countries: France, Sweden & New Zealand

### Inspect CEOs 

```sparql
## Count the total number of ceos
SELECT (COUNT(*) as ?eff)
WHERE {
    ?item wdt:P31 wd:Q5;  # Any instance of a human.

        wdt:P106 wd:Q484876  # Occupation: CEO
}  
#LIMIT 10
```

