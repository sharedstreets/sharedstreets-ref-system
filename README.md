# SharedStreets Referencing System

SharedStreets is a framework for creating and exchanging street-linked data. Unlike GIS-based methods for data exchange that depend on geometry or predefined, proprietary identifiers to describe streets, the SharedStreets Referencing system uses topology, and other descriptive properties to define street locations. SharedStreets references allow users to share street-linked data without sharing underlying map data, or require that users agree on a common base map.

SharedStreets was originally created as part of the [OpenTraffc project](https://github.com/opentraffic/architecture/issues/1) as a non-proprietary system for sharing street-linked data. It builds on the core concepts from [OpenLR](http://www.openlr.info/) and provides a set of data formats and open source tools to re-imagine collaboration and sharing of geospatial data.

### Example Applications
**Traffic data:** SharedStreets references are used to share basemap-independent descriptions of traffic conditions. In the OpenTraffic project fleet operators convert GPS data to to traffic observations (speeds along OpenStreetMap defined roadway segments). Traffic observations are shared externally using SharedStreets references to describe street segments.

**Street and curb inventory:** Cities produce detailed curb inventories (e.g. parking regulations and physical assets) using internally managed linear referencing systems (LRS), or latitude/longitude coordinates not linked with streets. Internal LRS data can be translated to SharedStreets references to allow interoperability with other city or external data sets.

**Incident/road closure reporting:** transport authorities share data about street conditions in real-time with consumer applications. SharedStreets references can be used to streamline reporting procedures by providing a shared, non-proprietary format for describing roadway incidents and closure events.

## DRAFT SharedStreets Referencing System

The SharedStreets Referencing system generates four layers of data:
1. **SharedStreets References:** basemap-independent references to street segments
2. **SharedStreets Intersection:** nodes connecting street street segments references
3. **SharedStreets Geometries:** geometries related to street segment references
4. **OSM Metadata:** underlying OSM way and node references used to construct SharedStreets data

### Stable, non-proprietary shorthand IDs
SharedStreets uses 128-bit shorthand identifiers to relate data within the SharedStreets referencing system. These IDs provide a basemap-independent system for linking to street segment references, intersections and geometries. **These identifiers are generated deterministically using a hash of the underlying data.** This means that two different users with the same input data can generate matching SharedStreets identifiers. This simplifies data sharing, allowing users to match data using shorthand IDs whenever possible.

### Generating references + data tiles
SharedStreets street and intersection references can be pre-generated from OSM data using the [SharedStreets Builder](https://github.com/sharedstreets/sharedstreets-builder) application. This allows anyone to rapidly match their data against existing segment references. 

As part of the DRAFT release pre-generated SharedStreets tiles for New York City are included in this repository. Users can generate their own tiles for any betray OSM data set using the Builder application. Once the data specification is finalized a SharedStreets will generate and maintain a global data tile set. 

The DRAFT release of SharedStreets uses JSON files, cut into mercator tiles at zoom level 10. These DRAFT tiles are verbose -- they are designed to  

### BYOM 
SharedStreets is based on the idea that users can/should maintain their own internal basemaps. Any basemap--open or proprietary--can generate SharedStreets references that can be shared with others using a different map. With open base maps users are encouraged to share the full set of SharedStreets data layers (references, geometries and metadata).

### SharedStreets References
Building on the OpenLR location referencing system, SharedStreets references provide a global, basemap-independent system for describing streets. Each SharedStreets Reference (SSR) consists of two or more location references (LRs) that describe the latitude and longitude of the beginning or end of a street segment. SSRs also describe type of road, and length of the geometry connecting terminal points. In combination these attributes uniquely describe any road segment, and can be used to look up corresponding streets in users’ internal maps.

- Each SSR describes a directional street segment, between corresponding intersections. Two-way streets are described using two SSRs, one in each direction, while a one-way street only has one SSR.
- For long segments LRs are repeated every 15km, segments shorter than 15km have only a beginning and end LR. 
- LRs describe the compass bearing of the street geometry immediately following the the LR. The final LR of a SSR does not provide a bearing. 
- An initial set of SSRs are built using OpenStreetMap (OSM) data, however, in the future SharedStreets users will be able to publicly register SSRs for streets not found in OSM. These could include data found in commercial or government maintained basemaps. Users do not have to share the underlying data, only the SSR descriptor. 


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

### SharedStreets Intersections
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


### SharedStreets Geometries
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


### SharedStreets OSM Metadata
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


### Frequently Asked Questions

**How does this relate to OpenStreetMap? (Aka doesn't OSM already do this?)**

SharedStreets is complementary to OSM. OSM does not attempt to provide stable IDs, and complex OSM ways make many application challenging to build using raw OSM data. 

SharedStreets provides a layer of abstraction on top of OSM, allowing users to work with the *topology* of OpenStreetMaps data without dealing with the details how OSM ways are encoded. 

By providing direct references to OSM way and node IDs users can always query and relate the underlying OSM data in cases where needed.


**How does SharedStreets relate to OSMLR v1.0?**

OSMLR v1.0 was developed by Mapzen to support OpenTraffic, under contract from the World Bank. SharedStreets is effectively OSMLR v2.0, but drops the OSMLR name as it intends to support broad range of map data formats including but not limited to OSM. 

SharedStreets also addresses two serious problems in the v1.0 implementation:

**The OSMLR 1.0 implementation splits street segments into 1km sections.** This design conflates location referencing (the identification of street segments) with linear reference (the identification of locations along segments). 

As segment geometries change due to improved mapping the length of segments varies -- sometimes substantially. For longer segments this could create or destroy OSMLR references (e.g. a 10km stretch of road becomes 12km when the map is improved, resulting in two new OSMLR 1km segments). This undermines one the of the core ideas of OpenLR: underlying map geometries can change while  segment references remain stable.

This problem is especially serious in developing countries where the quality of maps are rapidly improving. It also creates problems sharing data between users with different quality basemaps. Users with more precise maps will in most cases have longer map segments, resulting in new OSMLR segments that cannot be generated by users with coarser maps.

Pre-spliting segments into 1km sections also undermines applications that describe locations along segments (e.g curbside parking regulations). Pre-spliting streets into 1km OSMLR chunks creates challenges for encoding features that span longer sections of road.  Given the geometry changes may move (or even create or destroy) segment reference, precise linear referencing is not possible using OSMLR v1.0.

SharedStreets addresses this problem by decoupling location referencing and linear reference. All streets segments are mapped at intersection to intersection. Users with different quality maps should be able to overcome variations in geometry length by using other properties of the reference (segment end points, bearing and "form of way"). SharedStreets provides a separate linear referencing format to describe points or sections of the segment. 

**2) The OSMLR 1.0 implementation depends on Valhalla, an open source routing engine developed by Mapzen, to generate segment IDs.** These IDs cannot be generated without deploying the Valhalla system. This creates a dependency on a specific piece of software, likely reducing the uptake and portability of OSMLR spec by non-Valhalla users.

SharedStreets addresses this by creating a deterministic, software-independent process for generating shorthand IDs. SharesStreets provides open source software as a reference, but the methods can be easily incorporated into other software to independently generate matching IDs.


The above issues noted, OSMLR v1.0 can be mapped to SharedStreets segments when needed. SharedStreets segments cannot be accurately described using OSMLR v1.0 due to limitations of the OSMLR spec. 





