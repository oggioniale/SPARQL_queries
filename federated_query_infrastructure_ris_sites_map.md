# Federated query eLTER, ICOS and EDIOS

This query retrieves environmental monitoring facilities from multiple research infrastructures
(eLTER, ICOS and EDIOS) using federated SPARQL queries and visualizes them on a map sites of eLTER and stations for ICOS.

The map output is designed to work with the map plugin of [YASGUI](https://yasgui.triply.cc/).

## SPARQL interface

You can execute the query directly here:

➡ https://yasgui.triply.cc/

## SPARQL Query

[▶ Run query in YASGUI](https://yasgui.triply.cc/#endpoint=https://fuseki.lteritalia.it/eLTER_test$query=PREFIX%20rdf%3A%20%20%3Chttp%3A%2F%2Fwww.w3.org%2F1999%2F02%2F22-rdf-syntax-ns%23%3E%0APREFIX%20ef%3A%20%20%20%3Chttp%3A%2F%2Fwww.w3.org%2F2015%2F03%2Finspire%2Fef%23%3E%0APREFIX%20rdfs%3A%20%3Chttp%3A%2F%2Fwww.w3.org%2F2000%2F01%2Frdf-schema%23%3E%0APREFIX%20geo%3A%20%20%3Chttp%3A%2F%2Fwww.opengis.net%2Font%2Fgeosparql%23%3E%0APREFIX%20cpmeta%3A%20%3Chttp%3A%2F%2Fmeta.icos-cp.eu%2Fontologies%2Fcpmeta%2F%3E%0APREFIX%20dcat%3A%20%3Chttp%3A%2F%2Fwww.w3.org%2Fns%2Fdcat%23%3E%0APREFIX%20dcterms%3A%20%3Chttp%3A%2F%2Fpurl.org%2Fdc%2Fterms%2F%3E%0APREFIX%20locn%3A%20%3Chttp%3A%2F%2Fwww.w3.org%2Fns%2Flocn%23%3E%0A%0ASELECT%0A%20%20%3Fsource%0A%20%20%3Fa%0A%20%20%3FsiteLabel%0A%20%20%3Fwkt%0A%20%20%3FwktLabel%0A%20%20%3FwktColor%0AWHERE%20%7B%0A%20%20%23%20--------%20eLTER%20--------%0A%20%20%7B%0A%20%20%20%20SELECT%20%3Fsource%20%3Fa%20%3FsiteLabel%20%3Fwkt%0A%20%20%20%20WHERE%20%7B%0A%20%20%20%20%20%20SERVICE%20%3Chttp%3A%2F%2Ffuseki1.get-it.it%2Felter%2Fquery%3E%20%7B%0A%20%20%20%20%20%20%20%20%3Fa%20ef%3AbelongsTo%20%3Chttps%3A%2F%2Fdeims.org%2Fnetworks%2F4742ffca-65ac-4aae-815f-83738500a1fc%3E%20.%0A%20%20%20%20%20%20%20%20%3Fa%20rdf%3Atype%20ef%3AEnvironmentalMonitoringFacility%20%3B%0A%20%20%20%20%20%20%20%20%20%20%20ef%3Aname%20%3FsiteLabel%20%3B%0A%20%20%20%20%20%20%20%20%20%20%20dcterms%3Aspatial%20%3Floc%20.%0A%20%20%20%20%20%20%20%20%3Floc%20dcat%3Acentroid%20%3Fwkt%20.%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20BIND(%22eLTER%22%20AS%20%3Fsource)%0A%20%20%20%20%7D%0A%20%20%20%20ORDER%20BY%20%3FsiteLabel%20%3Fa%0A%20%20%7D%0A%20%20%23%20--------%20EDIOS%20--------%0A%20%20UNION%20%7B%0A%20%20%20%20SELECT%20%3Fsource%20%3Fa%20%3FsiteLabel%0A%20%20%20%20WHERE%20%7B%0A%20%20%20%20%20%20SERVICE%20%3Chttps%3A%2F%2Flinked.bodc.ac.uk%2Fsdn%2Fedios%2Fsparql%2Fsparql%3E%20%7B%0A%20%20%20%20%20%20%20%20%3Fa%20rdf%3Atype%20ef%3AEnvironmentalMonitoringFacility%20%3B%0A%20%20%20%20%20%20%20%20%20%20%20rdfs%3Alabel%20%3FsiteLabel%20.%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20BIND(%22EDIOS%22%20AS%20%3Fsource)%0A%20%20%20%20%7D%0A%20%20%20%20ORDER%20BY%20%3FsiteLabel%20%3Fa%0A%20%20%7D%0A%20%20%23%20--------%20ICOS%20--------%0A%20%20UNION%20%7B%0A%20%20%20%20SELECT%20%3Fsource%20%3Fa%20%3FsiteLabel%20%3Fwkt%0A%20%20%20%20WHERE%20%7B%0A%20%20%20%20%20%20SERVICE%20%3Chttps%3A%2F%2Fmeta.icos-cp.eu%2Fsparql%3E%20%7B%0A%20%20%20%20%20%20%20%20%3Fa%20rdf%3Atype%20cpmeta%3AStation%20%3B%0A%20%20%20%20%20%20%20%20%20%20%20cpmeta%3AhasName%20%3FsiteLabel%20%3B%0A%20%20%20%20%20%20%20%20%20%20%20cpmeta%3AhasLongitude%20%3Flon%20%3B%0A%20%20%20%20%20%20%20%20%20%20%20cpmeta%3AhasLatitude%20%3Flat%20.%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20BIND(%22ICOS%22%20AS%20%3Fsource)%0A%20%20%20%20%20%20%0A%20%20%20%20%20%20BIND(%0A%20%20%20%20%20%20%20%20STRDT(%0A%20%20%20%20%20%20%20%20%20%20CONCAT(%22%3Chttp%3A%2F%2Fwww.opengis.net%2Fdef%2Fcrs%2FEPSG%2F0%2F4326%3E%20POINT(%22%2C%20STR(%3Flon)%2C%20%22%20%22%2C%20STR(%3Flat)%2C%20%22)%22)%2C%0A%20%20%20%20%20%20%20%20%20%20geo%3AwktLiteral%0A%20%20%20%20%20%20%20%20)%20AS%20%3Fwkt%0A%20%20%09%20%20)%0A%20%20%20%20%7D%0A%20%20%20%20ORDER%20BY%20%3FsiteLabel%20%3Fa%0A%20%20%7D%0A%20%20BIND(%3Fsource%20AS%20%3Fcategory)%0A%20%20%23%20----%20color%20mapping%20----%0A%20%20BIND(%0A%20%20IF(%3Fsource%3D%22ICOS%22%2C%22%23E41C63%22%2C%0A%20%20%20%20IF(%3Fsource%3D%22eLTER%22%2C%22%232777AF%22%2C%0A%20%20%20%20%20%20IF(%3Fsource%3D%22EDIOS%22%2C%22%230000ff%22%2C%22%23888888%22)%0A%20%20%20%20)%0A%20%20)%20AS%20%3FwktColor%0A)%0A%20%20%23%20----%20popup%20----%0A%20%20BIND(CONCAT(%0A%20%20%20%20%22%3Cdiv%20style%3D%27font-size%3A13px%3Bline-height%3A1.3em%27%3E%22%2C%0A%20%20%20%20%22RI%3A%20%3Cb%3E%22%2C%20STR(%3Fsource)%2C%20%22%3C%2Fb%3E%22%2C%0A%20%20%20%20%22%3Cbr%2F%3ESite%2FStation%20name%3A%20%22%2C%0A%20%20%20%20%22%3Cbr%2F%3E%3Ca%20href%3D%27%22%2C%20STR(%3Fa)%2C%0A%20%20%20%22%27%20target%3D%27_blank%27%3E%22%2C%20STR(%3FsiteLabel)%2C%20%22%3C%2Fa%3E%22%2C%0A%20%20%20%20%22%3C%2Fdiv%3E%22%0A%20%20)%20AS%20%3FhtmlLabel)%0A%0A%20%20BIND(STRDT(%3FhtmlLabel%2C%20rdf%3AHTML)%20AS%20%3FwktLabel)%0A%7D%0AORDER%20BY%20%3Fsource%20%3FwktLabel%0A)

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
