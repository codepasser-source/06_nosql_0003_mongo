db.places.createIndex(
    {"geometry" : "2dsphere" }
)

db.places.insert(
	{
	    "targetId": "0000-0000",
	    "locationType": "USER_LOCATION",
	    "description": {
		"location": {
		    "lat": 39.984154,
		    "lng": 116.30749
		},
		"address": "北京市海淀区彩和坊路北四环西路66号0"
	    },
	    "geometry": {
		"type": "Point",
		"coordinates": [
		    116.30749,
		    39.984154
		]
	    }
	}
)

db.places.insert(
	{
	    "targetId": "0000-0001",
	    "locationType": "USER_LOCATION",
	    "description": {
		"location": {
		    "lat": 39.984154,
		    "lng": 116.30749
		},
		"address": "北京市海淀区彩和坊路北四环西路66号1"
	    },
	    "geometry": {
		"type": "Point",
		"coordinates": [
		    116.30749,
		    39.984154
		]
	    }
	}
)

db.places.insert(
	{
	    "targetId": "0000-0002",
	    "locationType": "USER_LOCATION",
	    "description": {
		"location": {
		    "lat": 39.984154,
		    "lng": 116.30749
		},
		"address": "北京市海淀区彩和坊路北四环西路66号2"
	    },
	    "geometry": {
		"type": "Point",
		"coordinates": [
		    116.30749,
		    39.984154
		]
	    }
	}
)

db.places.insert(
	{
	    "targetId": "0000-0003",
	    "locationType": "USER_LOCATION",
	    "description": {
		"location": {
		    "lat": 39.984154,
		    "lng": 116.30749
		},
		"address": "北京市海淀区彩和坊路北四环西路66号3"
	    },
	    "geometry": {
		"type": "Point",
		"coordinates": [
		    116.30749,
		    39.984154
		]
	    }
	}
)

db.places.insert(
	{
	    "targetId": "0000-0004",
	    "locationType": "USER_LOCATION",
	    "description": {
		"location": {
		    "lat": 39.984154,
		    "lng": 116.30749
		},
		"address": "北京市海淀区彩和坊路北四环西路66号4"
	    },
	    "geometry": {
		"type": "Point",
		"coordinates": [
		    116.30749,
		    39.984154
		]
	    }
	}
)



db.places.find(
    {
            "geometry": {
                $near: {
                    $geometry: {
                        type: "Point",
                        coordinates: [
                            116.30749,
                            39.984154
                        ]
                    },
                    $maxDistance: 100000
                }
            }

    }
).pretty()

======================================

db.places.createIndex( { "creationDate": 1 }, { expireAfterSeconds: 60 } )

db.places.createIndex(
    {"geometry" : "2dsphere" }
)
db.places.insert(
	{
	    "targetId" : "0000-0000",
	    "locationType" : "USER_LOCATION",
	    "creationDate" : ISODate("2016-05-30T15:53:07.036Z"),
	    "description" : {
		"location" : {
		    "lat" : 39.984154,
		    "lng" : 116.30749
		},
		"address" : "北京市海淀区彩和坊路北四环西路66号0"
	    },
	    "geometry" : {
		"type" : "Point",
		"coordinates" : [ 
		    116.30749, 
		    39.984154
		]
	    }
	}
)
db.places.insert(
	{
	    "targetId" : "0000-0001",
	    "locationType" : "USER_LOCATION",
	    "creationDate" : ISODate("2016-05-30T15:53:08.036Z"),
	    "description" : {
		"location" : {
		    "lat" : 39.984154,
		    "lng" : 116.30749
		},
		"address" : "北京市海淀区彩和坊路北四环西路66号1"
	    },
	    "geometry" : {
		"type" : "Point",
		"coordinates" : [ 
		    116.30749, 
		    39.984154
		]
	    }
	}
)

db.places.find(
    {
            "geometry": {
                $near: {
                    $geometry: {
                        type: "Point",
                        coordinates: [
                            100.30749,
                            39.984154
                        ]
                    },
                    $maxDistance: 10000000
                }
            }

    }
).pretty()



===========================================================================

db.places.insert(
{
    "_class" : "com.askdog.model.data.UserLocation",
    "ip" : "127.0.0.1",
    "creationDate" : new Date(),
    "provider" : "TENCENT_MAP",
    "description" : {
        "_class" : "com.askdog.service.impl.position.tencent.PlaceLocationDescription",
        "addressInfo" : {
            "addressCode" : "110108",
            "name" : "中国,北京市,北京市,海淀区",
            "location" : {
                "lat" : 39.984154,
                "lng" : 116.307487
            },
            "nation" : "中国",
            "province" : "北京市",
            "city" : "北京市",
            "district" : "海淀区"
        },
        "location" : {
            "lat" : 39.984154,
            "lng" : 116.30749
        },
        "address" : "北京市海淀区彩和坊路北四环西路66号"
    },
    "geometry" : {
        "type" : "Point",
        "coordinates" : [ 
            116.30749, 
            39.984154
        ]
    },
    "target" : "7c88e332-5c43-4013-a8f5-dd3244785431",
    "targetType" : "USER"
}
)

======================================

db.userLocation.aggregate(
   [
        {
             $geoNear: {
                near: { type: "Point", coordinates: [ 116.30749, 39.984154 ] },
                distanceField : "geometry",
                maxDistance : 200000,
                spherical: true
             }
        },
        {
            $group : {
                _id : "$target", 
                loginCount : {$sum : 1},
                creationDate : { $max : "$creationDate"}
            }
        },
        { 
            $sort : { 
                creationDate : 1 
            } 
        }
    ]
)
