# Federated query eLTER, ICOS and EDIOS

This query retrieves environmental monitoring facilities from multiple research infrastructures
(eLTER, ICOS and EDIOS) using federated SPARQL queries and visualizes them on a map sites of eLTER and stations for ICOS.

The map output is designed to work with the map plugin of [YASGUI](https://yasgui.triply.cc/).

## SPARQL interface

You can execute the query directly here:

➡ https://yasgui.triply.cc/

## SPARQL Query

```sparql
PREFIX rdf:  <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX ef:   <http://www.w3.org/2015/03/inspire/ef#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX geo:  <http://www.opengis.net/ont/geosparql#>
PREFIX cpmeta: <http://meta.icos-cp.eu/ontologies/cpmeta/>
PREFIX dcat: <http://www.w3.org/ns/dcat#>
PREFIX dcterms: <http://purl.org/dc/terms/>
PREFIX locn: <http://www.w3.org/ns/locn#>

SELECT
  ?source
  ?a
  ?siteLabel
  ?wkt
  ?wktLabel
  ?wktColor
WHERE {
  # -------- eLTER --------
  {
    SELECT ?source ?a ?siteLabel ?wkt
    WHERE {
      SERVICE <http://fuseki1.get-it.it/elter/query> {
        ?a ef:belongsTo <https://deims.org/networks/4742ffca-65ac-4aae-815f-83738500a1fc> .
        ?a rdf:type ef:EnvironmentalMonitoringFacility ;
           ef:name ?siteLabel ;
           dcterms:spatial ?loc .
        ?loc dcat:centroid ?wkt .
      }
      BIND("eLTER" AS ?source)
    }
    ORDER BY ?siteLabel ?a
  }
  # -------- EDIOS --------
  UNION {
    SELECT ?source ?a ?siteLabel
    WHERE {
      SERVICE <https://linked.bodc.ac.uk/sdn/edios/sparql/sparql> {
        ?a rdf:type ef:EnvironmentalMonitoringFacility ;
           rdfs:label ?siteLabel .
      }
      BIND("EDIOS" AS ?source)
    }
    ORDER BY ?siteLabel ?a
  }
  # -------- ICOS --------
  UNION {
    SELECT ?source ?a ?siteLabel ?wkt
    WHERE {
      SERVICE <https://meta.icos-cp.eu/sparql> {
        ?a rdf:type cpmeta:Station ;
           cpmeta:hasName ?siteLabel ;
           cpmeta:hasLongitude ?lon ;
           cpmeta:hasLatitude ?lat .
      }
      BIND("ICOS" AS ?source)
      
      BIND(
        STRDT(
          CONCAT("<http://www.opengis.net/def/crs/EPSG/0/4326> POINT(", STR(?lon), " ", STR(?lat), ")"),
          geo:wktLiteral
        ) AS ?wkt
  	  )
    }
    ORDER BY ?siteLabel ?a
  }
  BIND(?source AS ?category)
  # ---- color mapping ----
  BIND(
  IF(?source="ICOS","#E41C63",
    IF(?source="eLTER","#2777AF",
      IF(?source="EDIOS","#0000ff","#888888")
    )
  ) AS ?wktColor
)
  # ---- popup ----
  BIND(CONCAT(
    "<div style='font-size:13px;line-height:1.3em'>",
    "RI: <b>", STR(?source), "</b>",
    "<br/>Site/Station name: ",
    "<br/><a href='", STR(?a),
   "' target='_blank'>", STR(?siteLabel), "</a>",
    "</div>"
  ) AS ?htmlLabel)

  BIND(STRDT(?htmlLabel, rdf:HTML) AS ?wktLabel)
}
ORDER BY ?source ?wktLabel
```

```markdown
[▶ Run query in YASGUI](https://yasgui.triply.cc/#query=ENCODED_QUERY)
```
