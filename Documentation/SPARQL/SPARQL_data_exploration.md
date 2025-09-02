# Explore Wikidata

This notebook explores **US mayors** using the **Wikidata SPARQL endpoint**.  
We’ll proceed step by step — starting broad (all mayors), then filtering for US and date of birth, and finally extracting detailed information.

---

## Step 1: Count all mayors worldwide
```sparql
# 1) Count humans who have held a mayoral position (any country)
SELECT (COUNT(*) AS ?count)
WHERE {
  ?item wdt:P31 wd:Q5;            # human
        wdt:P39 ?position.        # position held

  ?position wdt:P279* wd:Q30185.  # (subclass of) mayor
}
```
--- 
## Step 2: Restrict to Us mayors only
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
---
## Step 3: Filter US mayors born after 1800
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
- Result: 2813
--- 
## Step 4: List available properties with headcount
This query shows which properties (e.g., gender, party, etc.) exist for US mayors born after 1800,
and how many distinct people have each property filled in.
(Incleded in Wikidata Documents: wdt_mayors_properties.csv)
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
--- 
## Step 5: Retrieve full biographical data
Finally, we can fetch a table of US mayors born after 1800,
including biographical details, political party, term dates, and the city served.
(Included it in Wikidata: mayors_core.csv)
```sparql
# US mayors born after 1800 — all fields required
SELECT ?person ?personLabel ?birthDate ?genderLabel ?partyLabel
       ?start ?end ?positionLabel ?cityLabel
WHERE {
  # person + required biographical properties
  ?person wdt:P569 ?birthDate .
  FILTER (YEAR(?birthDate) > 1800)
  ?person wdt:P21 ?gender .
  ?person wdt:P102 ?party .

  # position statements (term-level)
  ?person p:P39 ?posStmt .
  ?posStmt ps:P39 ?position .
  ?posStmt pq:P580 ?start .
  ?posStmt pq:P582 ?end .

  # office is (subclass of) mayor and applies to a US city
  ?position wdt:P279* wd:Q30185 ;
            wdt:P1001 ?city .
  ?city wdt:P17 wd:Q30 .

  SERVICE wikibase:label { bd:serviceParam wikibase:language "en,fr". }
}
ORDER BY ?personLabel ?start
```
Resluts:
- 957 rows → mayors with all required data
- 1902 rows → if start and end dates are made optional

--- 
## Data with education
```sparql

# US mayors born after 1800 — LONG FORMAT (no GROUP_CONCAT)
# One row per (person, term, education). Education type included for bucketing.

PREFIX wd:   <http://www.wikidata.org/entity/>
PREFIX wdt:  <http://www.wikidata.org/prop/direct/>
PREFIX p:    <http://www.wikidata.org/prop/>
PREFIX ps:   <http://www.wikidata.org/prop/statement/>
PREFIX pq:   <http://www.wikidata.org/prop/qualifier/>

SELECT
  ?person ?personLabel ?birthDate ?genderLabel ?partyLabel
  ?start ?end ?positionLabel ?cityLabel
  ?edu ?eduLabel
  ?eduType ?eduTypeLabel
WHERE {
  # person + required biographical properties
  ?person wdt:P569 ?birthDate .
  FILTER (YEAR(?birthDate) > 1800)
  ?person wdt:P21  ?gender .
  ?person wdt:P102 ?party .

  # position statements (term-level)
  ?person p:P39 ?posStmt .
  ?posStmt ps:P39 ?position .
  ?posStmt pq:P580 ?start .
  ?posStmt pq:P582 ?end .

  # office is (subclass of) mayor and applies to a US city
  ?position wdt:P279* wd:Q30185 ;   # mayor
            wdt:P1001  ?city .
  ?city wdt:P17 wd:Q30 .            # United States

  # Education (REQUIRED for the network)
  ?person wdt:P69 ?edu .
  OPTIONAL { ?edu wdt:P31 ?eduType . }  # immediate type of the school (we’ll bucket later)

  SERVICE wikibase:label {
    bd:serviceParam wikibase:language "en,fr,en-gb" .
    ?person     rdfs:label ?personLabel .
    ?gender     rdfs:label ?genderLabel .
    ?party      rdfs:label ?partyLabel .
    ?position   rdfs:label ?positionLabel .
    ?city       rdfs:label ?cityLabel .
    ?edu        rdfs:label ?eduLabel .
    ?eduType    rdfs:label ?eduTypeLabel .
  }
}
ORDER BY ?personLabel ?start ?eduLabel
```
Results:
-  3117 rows
- However oonly 562 distinct mayors

--- 
## Data with only education and mayoral terms

```sparql
# US mayors born after 1800 — LONG FORMAT (no GROUP_CONCAT)
# One row per (person, term, education). Minimal columns for network use.

PREFIX wd:   <http://www.wikidata.org/entity/>
PREFIX wdt:  <http://www.wikidata.org/prop/direct/>
PREFIX p:    <http://www.wikidata.org/prop/>
PREFIX ps:   <http://www.wikidata.org/prop/statement/>
PREFIX pq:   <http://www.wikidata.org/prop/qualifier/>

SELECT DISTINCT
  ?personLabel
  ?birthDate
  ?genderLabel
  ?start
  ?eduLabel
WHERE {
  # person + birthdate filter
  ?person wdt:P569 ?birthDate .
  FILTER (YEAR(?birthDate) > 1800)

  # position statements (term-level)
  ?person p:P39 ?posStmt .
  ?posStmt ps:P39 ?position .
  OPTIONAL { ?posStmt pq:P580 ?start . }   # start date is optional

  # office is (subclass of) mayor and applies to a US city
  ?position wdt:P279* wd:Q30185 ;          # mayor
            wdt:P1001  ?city .
  ?city wdt:P17 wd:Q30 .                   # United States

  # education (required for the network)
  ?person wdt:P69 ?edu .

  # gender optional to avoid losing rows
  OPTIONAL { ?person wdt:P21 ?gender . }

  SERVICE wikibase:label {
    bd:serviceParam wikibase:language "en,fr,en-gb" .
    ?person  rdfs:label ?personLabel .
    ?gender  rdfs:label ?genderLabel .
    ?edu     rdfs:label ?eduLabel .
  }
}
ORDER BY ?personLabel ?start ?eduLabel
```




