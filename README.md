# SharedStreets Referencing System

## Introduction
SharedStreets data standards provide tools to describe streets and exchange data on how they are used. Cities today depend on geographic information system (GIS) based methods to share street data, but this process requires users to agree on a common base map, or to use predefined, and often proprietary tools to identify and describe streets.

This limits the potential for collaboration and data sharing between government agencies and with the private sector. And use of proprietary maps and identification systems can undermine cities’ ability to use and share critical information about public streets.

SharedStreets provides a global, non-proprietary system for describing streets, designed to incorporate any source of transportation data. This allows public and private entities to communicate with clarity and precision about streets while ensuring full compatibility with organizations’ internal map data.

The SharedStreets referencing system was originally created as part of the [OpenTraffc project](https://github.com/opentraffic/architecture/issues/1), as a mechanism for sharing dynamic traffic information. SharedStreets builds on the core concepts from [OpenLR](http://www.openlr.info/) and provides a collection of data formats and open source tools to re-imagine collaboration and sharing of transport data.
 
The SharedStreets referencing system is being released as a DRAFT proposal, along with data samples and tools for generating references from OpenStreetMap and Shapefile data sources. Comments and suggestions on the format will be managed via this Github repository. Please open an Github issue or pull request to contribute. 

**A production version of the SharedStreets Referencing System and a global data set will be released in Winter 2017.**

### Example Applications
**Traffic data:** SharedStreets references are used to share basemap-independent descriptions of traffic conditions. In the OpenTraffic project fleet operators convert GPS data to to traffic observations (speeds along OpenStreetMap defined roadway segments). Traffic observations are shared externally using SharedStreets references to describe street segments.

**Street and curb inventory:** Cities produce detailed curb inventories (e.g. parking regulations and physical assets) using internally managed linear referencing systems (LRS), or latitude/longitude coordinates not linked with streets. Internal LRS data can be translated to SharedStreets references to allow interoperability with other city or external data sets.

**Incident/road closure reporting:** transport authorities share data about street conditions in real-time with consumer applications. SharedStreets references can be used to streamline reporting procedures by providing a shared, non-proprietary format for describing roadway incidents and closure events.

## Core Concepts

The SharedStreets Referencing system is built on four layers of data:

1. **SharedStreets References:** basemap-independent references for intersection to intersection street segments
2. **SharedStreets Intersection:** nodes connecting street street segments references
3. **SharedStreets Geometries:** geometries used to generate street segment references
4. **OSM Metadata:** underlying OSM way and node references used to construct SharedStreets data

### Segment references 

The OpenLR-style street segment references are the foundation of the SharedStreets Referencing System. These references allow users to uniquely describe any street segment in the world using just a few high-level characteristics of the road. This allows users with different map geometries to describe the same street segments in identical or nearly identical terms. This protects users' intellectual property and enables rapid reconciliation of street segments derived from different maps.

### BYOM 
SharedStreets is based on the idea that users will maintain their own internal basemaps. Any basemap--open or proprietary--can generate SharedStreets references for sharing with others using a different map. With open basemap data users are encouraged to share the full set of SharedStreets data layers (references, geometries and metadata). Users with proprietary basemap data can share only the segment references, allowing exchange of information while protecting map street geometries and other intellectual property.

### Stable, non-proprietary shorthand IDs
SharedStreets uses 128-bit shorthand identifiers to relate data within the SharedStreets referencing system. These IDs provide a basemap-independent addressing system for street segment references, intersections and geometries. **These identifiers are generated deterministically using a hash of the underlying data.** This means that two different users with the same input data can generate matching SharedStreets identifiers. This simplifies data sharing, allowing users to match data using shorthand IDs whenever possible.

### Generating references + data tiles
SharedStreets street references, intersections and geometries can be generated from OSM data using the [SharedStreets Builder](https://github.com/sharedstreets/sharedstreets-builder) application. 

As part of the DRAFT release pre-generated a sample of SharedStreets tiles for New York City are included in this repository. A zip archive of the full NYC tile set can be downloaded here (xxx MB file). Users can generate their own tiles for any arbitrary OSM data set using the Builder application. 

Once the data specification is finalized a SharedStreets will generate and maintain a global data tile set. 

The DRAFT release of SharedStreets uses JSON files, cut into mercator tiles at zoom level 10. These DRAFT tiles are verbose -- they are designed to support exploration and refinement of the specification. Once the spec is finalized SharedStreets will provide JSON and Protocol buffer tile formats. SharedStreets is also exploring use of lossy vector tile formats for distribution of data for web visualization. 


## DRAFT Data Formats

### SharedStreets References
Building on the OpenLR location referencing system, SharedStreets references provide a global, basemap-independent system for describing streets. 

SharedStreets References (SSR) are directional edges in a road network. Two-way streets have two SSRs, one for each direction of travel, while a one-way street only has one SSR. In the draft specification these are label "forward references" and "back references," with the forward reference following the direction of the map geometry used to create the reference.

Each SharedStreets Reference consists of two or more location references (LRs) that describe the latitude and longitude of the beginning or end of a street segment. SSRs also describe type of road, and length of the geometry connecting terminal points. In combination these attributes uniquely describe any road segment, and can be used to look up corresponding streets in users’ internal maps.

####Forward Reference

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

####Back Reference

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

*Notes:*

- For long segments LRs are repeated every 15km, segments shorter than 15km have only a beginning and end LR. 
- LRs describe the compass bearing of the street geometry immediately following the the LR. The final LR of a SSR does not provide a bearing. 
- An initial set of SSRs are built using OpenStreetMap (OSM) data, however, in the future SharedStreets users will be able to publicly register SSRs for streets not found in OSM. These could include data found in commercial or government maintained basemaps. Users do not have to share the underlying data, only the SSR descriptor. 




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

SharedStreets is complementary to OpenStreetMap. OSM does not attempt to provide stable IDs, and complex OSM ways make many application challenging to build using raw OSM data. 

SharedStreets provides a layer of abstraction on top of OSM, allowing users to work with the *topology* of OpenStreetMaps data without dealing with the details how OSM ways are encoded. 

By providing direct references to OSM way and node IDs users can always query and relate the underlying OSM data in cases where needed. 

We believe that SharedStreets will allow users to more rapidly improve OpenStreetMap data by making it easier to identify missing streets, or opportunities to improve street geometries.  


**How does SharedStreets relate to OSMLR v1.0?**

OSMLR v1.0 was developed by Mapzen to support OpenTraffic, under contract from the World Bank. SharedStreets is effectively OSMLR v2.0, but drops the OSMLR name as it intends to support broad range of map data formats including but not limited to OSM. 

SharedStreets also addresses two serious problems in the v1.0 implementation:

**The OSMLR 1.0 implementation splits street segments into 1km sections.** This design conflates location referencing (the identification of street segments) with linear reference (the identification of locations along segments). 

As segment geometries change due to improved mapping the length of segments varies -- sometimes substantially. For longer segments this could create or destroy OSMLR references (e.g. a 10km stretch of road becomes 12km when the map is improved, resulting in two new OSMLR 1km segments). This undermines one the of the core ideas of OpenLR: underlying map geometries can change while  segment references remain stable.

This problem is especially serious in developing countries where the quality of maps are rapidly improving. It also creates problems sharing data between users with different quality basemaps. Users with more precise maps will in most cases have longer map segments, resulting in new OSMLR segments that cannot be generated by users with coarser maps.

Pre-spliting segments into 1km sections also undermines applications that describe locations along segments (e.g curbside parking regulations). Pre-spliting streets into 1km OSMLR chunks creates challenges for encoding features that span longer sections of road.  Given the geometry changes may move (or even create or destroy) segment reference, precise linear referencing is not possible using OSMLR v1.0.

SharedStreets addresses this problem by decoupling location referencing and linear reference. All streets segments are mapped at intersection to intersection. Users with different quality maps should be able to overcome variations in geometry length by using other properties of the reference (segment end points, bearing and "form of way"). SharedStreets provides a separate linear referencing format to describe points or sections of the segment. 

**2) The OSMLR 1.0 implementation depends on Valhalla, an open source routing engine developed by Mapzen, to generate segment IDs.** These IDs cannot be generated without deploying the Valhalla system. This creates a dependency on a specific piece of software, likely reducing the uptake and portability of OSMLR spec by non-Valhalla users.

SharedStreets addresses this by creating a deterministic, software-independent process for generating shorthand IDs. SharedStreets provides open source software as a reference, but the methods can be easily incorporated into other applications to independently generate matching IDs.

The above issues noted, OSMLR v1.0 can be mapped to SharedStreets segments when needed. SharedStreets segments cannot be accurately described using OSMLR v1.0 due to limitations of the OSMLR spec. 





