## Sample Data 

This directory contains protocol buffer encoded SharedStreets sample data using the 2017-11-20 draft of the specification and preview release [v0.1.1](https://github.com/sharedstreets/sharedstreets-builder/releases/tag/untagged-2dad20e10ef9b6e50a6d) of the [SharedStreets Builder](https://github.com/sharedstreets/sharedstreets-builder) application. 

The files are named using spherical mercator [z-x-y] tile coordinates, and are encoded using [length delimted protobuf messages](https://developers.google.com/protocol-buffers/docs/techniques#streaming). Features that span a tile boundary are encoded in both tiles. 
