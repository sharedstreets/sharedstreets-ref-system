# SharedStreets Referencing Sytem

SharedStreets is a framework for creating and exchanging street-linked data. Unlike GIS-based methods for data exchange that depend on geometry or predefined, proprietary identifiers to describe streets, the SharedStreets Referencing system uses topology, and other descriptive properties to define street locations. SharedStreets references allow users to share street-linked data without sharing underlying map data, or require that users agree on a common base map.

**Traffic data:** SharedStreets references are used to share base map-independent descriptions of traffic conditions. In the OpenTraffic project fleet operators convert GPS data to to traffic observations (speeds along OpenStreetMap defined roadway segments). Traffic observations are shared externally using SharedStreets references to describe street segments.

**Street and curb inventory:** Cities produce detailed curb inventories (e.g. parking regulations and physical assets) using internally managed linear referencing systems (LRS), or latitude/longitude coordinates not linked with streets. Internal LRS data can be translated to SharedStreets references to allow interoperability with other city or external data sets.

**Incident/road closure reporting:** transport authorities share data about street conditions in real-time with consumer applications. SharedStreets references can be used to streamline reporting procedures by providing a shared, non-proprietary format for describing roadway incidents and closure events.



```javascript
{
	"id": "BXox6JBSoxKehSfbttGXtp",
	"geometryId": "GujiXxsS7m3aAXgjkQLjtc",
	"formOfWay": 3,
	"locationReferences": [{
		"sequence": 1,
		"point": [-74.245474, 41.026703],
		"bearing": 305.787486,
		"distanceToNextRef": 215.985158,
		"intersectionId": "DMYeEMvYTcz9B6iXG3QtgV"
	}, {
		"sequence": 2,
		"point": [-74.247557, 41.02784],
		"intersectionId": "LjTq6GRWvfmJk8HETDUwPQ"
	}]
}
```

```javascript
{
	"id": "8iw418RLK27coNzyuYKyLX",
	"geometryId": "GujiXxsS7m3aAXgjkQLjtc",
	"formOfWay": 3,
	"locationReferences": [{
		"sequence": 1,
		"point": [-74.247557, 41.02784],
		"bearing": 125.786372,
		"distanceToNextRef": 215.985158,
		"intersectionId": "LjTq6GRWvfmJk8HETDUwPQ"
	}, {
		"sequence": 2,
		"point": [-74.245474, 41.026703],
		"intersectionId": "DMYeEMvYTcz9B6iXG3QtgV"
	}]
}
```

```javascript
{
	"type": "Feature",
	"properties": {
		"id": "DMYeEMvYTcz9B6iXG3QtgV",
		"osmNodeId": 103157499,
		"outboundSegmentIds": ["PbmCXms5WpBrGph1atGoRy", "BXox6JBSoxKehSfbttGXtp", "8iw418RLK27coNzyuYKyLX", "4x2ceuTYV5Bk2hyeueBEnX"],
		"inboundSegmentIds": ["Eugdb2g745w6uDoJKtDf4A", "XhNgPcpLcyZ8hVenGCZ7q8"]
	},
	"geometry": {
		"type": "Point",
		"coordinates": [-74.245474, 41.026703]
	}
}
```

```javascript
{
	"type": "Feature",
	"properties": {
		"id": "LjTq6GRWvfmJk8HETDUwPQ",
		"osmNodeId": 103263349,
		"outboundSegmentIds": ["9p6W1Chtkhwb8a8NM3p5We", "WDmw8jdkXGdGkcfqiL7c1p"],
		"inboundSegmentIds": ["EU8qWsz7efFGNJ6rPVss99", "8iw418RLK27coNzyuYKyLX", "BXox6JBSoxKehSfbttGXtp", "Hv813fJDasteNAjPW7mrgQ"]
	},
	"geometry": {
		"type": "Point",
		"coordinates": [-74.247557, 41.02784]
	}
}
```


```javascript
{
	"type": "Feature",
	"properties": {
		"id": "GujiXxsS7m3aAXgjkQLjtc",
		"startIntersectionId": "DMYeEMvYTcz9B6iXG3QtgV",
		"endIntersectionId": "LjTq6GRWvfmJk8HETDUwPQ",
		"forwardReferenceId": "BXox6JBSoxKehSfbttGXtp",
		"backReferenceId": "8iw418RLK27coNzyuYKyLX",
		"roadClass": 5
	},
	"geometry": {
		"type": "LineString",
		"coordinates": [
			[-74.245474, 41.026703],
			[-74.247557, 41.02784]
		]
	}
}
```


```javascript
{
	"geometryId": "GujiXxsS7m3aAXgjkQLjtc",
	"waySections": [{
		"wayId": 11583251,
		"roadClass": 5,
		"oneWay": false,
		"roundabout": false,
		"link": false,
		"nodeIds": [103157499, 103263349]
	}]
}
```
