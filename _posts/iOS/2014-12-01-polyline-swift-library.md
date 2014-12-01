---
layout: post
title: "Polyline 1.0.0 : Encodage / Décodage de Polyline en Swift" 
category : iOS
comments: true
---

J'ai le plaisir de vous annoncer la sortie de la version 1.0.0 de ma bibliothèque d'encodage/décodage de __Polyline__ en Swift.

I'm happy to announce version 1.0.0 of my __Polyline__ encoding/decoding Swift library.

[Polyline on GitHub](https://github.com/raphaelmor/Polyline)

[Release 1.0.0](https://github.com/raphaelmor/Polyline/releases/tag/v1.0.0)



##Usage

### Polyline Encoding

Using `CLLocationCoordinate2D` (recommended) :

```swift
let coordinates = [CLLocationCoordinate2D(latitude: 40.2349727, longitude: -3.7707443),
CLLocationCoordinate2D(latitude: 44.3377999, longitude: 1.2112933)]

let polyline = Polyline(coordinates: coordinates)
let encodedPolyline : String = polyline.encodedPolyline
```

Using `CLLocation` :

```swift
let locations = [CLLocation(latitude: 40.2349727, longitude: -3.7707443),
CLLocation(latitude: 44.3377999, longitude: 1.2112933)]

let polyline = Polyline(locations: locations)
let encodedPolyline : String = polyline.encodedPolyline
```

You can specify levels too : 

```swift
let levels : [UInt32] = [0,1,2,255]

let polyline = Polyline(coordinates: coordinates, levels: levels)
let encodedLevels : String = polyline.encodedLevels

```

### Polyline Decoding

You can decode to `CLLocationCoordinate2D` (recommended) :

```swift
let polyline = Polyline(encodedPolyline: "qkqtFbn_Vui`Xu`l]")
let decodedCoordinates : Array<CLLocationCoordinate2D> = polyline.coordinates
```

You can also decode to `CLLocation` :

```swift
let polyline = Polyline(encodedPolyline: "qkqtFbn_Vui`Xu`l]")
let decodedLocations : Array<CLLocation> = polyline.locations
```

You can decode levels too :

```swift
let polyline = Polyline(encodedPolyline: "qkqtFbn_Vui`Xu`l]", encodedLevels: "BA")
let decodedLevels : Array<UInt32>? = polyline.levels
```




