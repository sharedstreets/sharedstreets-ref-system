# SharedStreets Referencing Sytem

SharedStreets is a framework for creating and exchanging street-linked data. Unlike GIS-based methods for data exchange that depend on geometry or predefined, proprietary identifiers to describe streets, the SharedStreets Referencing system uses topology, and other descriptive properties to define street locations. SharedStreets references allow users to share street-linked data without sharing underlying map data, or require that users agree on a common base map.

**Traffic data:** SharedStreets references are used to share base map-independent descriptions of traffic conditions. In the OpenTraffic project fleet operators convert GPS data to to traffic observations (speeds along OpenStreetMap defined roadway segments). Traffic observations are shared externally using SharedStreets references to describe street segments.

**Street and curb inventory:** Cities produce detailed curb inventories (e.g. parking regulations and physical assets) using internally managed linear referencing systems (LRS), or latitude/longitude coordinates not linked with streets. Internal LRS data can be translated to SharedStreets references to allow interoperability with other city or external data sets.

**Incident/road closure reporting:** transport authorities share data about street conditions in real-time with consumer applications. SharedStreets references can be used to streamline reporting procedures by providing a shared, non-proprietary format for describing roadway incidents and closure events.
