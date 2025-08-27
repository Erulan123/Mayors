## Explore Wikidata

In this notebook we will inspect and find relevant data on US mayors via Wikidata SPARQL endpoint
### Inspect CEOs 

```sparql
# 1) Count humans who have held a mayoral position (any country)
SELECT (COUNT(*) AS ?count)
WHERE {
  ?item wdt:P31 wd:Q5;            # human
        wdt:P39 ?position.        # position held

  ?position wdt:P279* wd:Q30185.  # (subclass of) mayor
}
```
Now we want to filter US mayors
```sparql
# 2) Count humans who have held a mayoral position in the United States
SELECT (COUNT(*) AS ?count)
WHERE {
  ?item wdt:P31 wd:Q5;            # human
        wdt:P39 ?position.        # position held

  ?position wdt:P279* wd:Q30185;  # (subclass of) mayor
            wdt:P17 wd:Q30.       # country = United States
}
```

Now those who are born after 1800
```sparql
# 3) Count DISTINCT humans who have held a mayoral position AND were born after 1800
SELECT (COUNT(DISTINCT ?item) AS ?count)
WHERE {
  ?item wdt:P31 wd:Q5;
        wdt:P39 ?position;
        wdt:P569 ?birthDate.      # birth date

  ?position wdt:P279* wd:Q30185;  # (subclass of) mayor
            wdt:P17 wd:Q30.       # country = United States
  
  FILTER (YEAR(?birthDate) > 1800)
}
```
* count 2813

# List available properties with headcount

```sparql
SELECT ?p ?propLabel (COUNT(DISTINCT ?item) AS ?count)
WHERE {
  ?item wdt:P31 wd:Q5;
        wdt:P39 ?position;
        wdt:P569 ?birthDate.       # birth date
  ?item ?p ?o.                     # get all properties of the item

  ?position wdt:P279* wd:Q30185;   # (subclass of) mayor
            wdt:P17 wd:Q30.        # position located in the United States

  FILTER (YEAR(?birthDate) > 1800)

  # link property to human-readable label
  ?prop wikibase:directClaim ?p .
  SERVICE wikibase:label { bd:serviceParam wikibase:language "en". }
}
GROUP BY ?p ?propLabel
ORDER BY DESC(?count)

```
