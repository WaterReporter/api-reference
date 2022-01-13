The Water Reporter API is organized around [REST](http://en.wikipedia.org/wiki/Representational_State_Transfer). Our API has predictable resource-oriented URLs, accepts form-encoded request bodies, returns [JSON-encoded](http://www.json.org/) responses, and uses standard HTTP response codes, authentication, and verbs.

## Authentication

The Water Reporter API uses API keys to authenticate requests. You can view and manage your API keys in your Water Reporter dashboard.

Your API keys carry many privileges, so be sure to keep them secure! Do not share your API keys in publicly accessible areas such as GitHub, client-side code, and so forth.

## Errors

Water Reporter uses conventional HTTP response codes to indicate the success or failure of an API request. In general: Codes in the 2xx range indicate success. Codes in the 4xx range indicate an error that failed given the information provided (e.g., a required parameter was omitted, etc.). Codes in the 5xx range indicate an error with Water Reporter's servers.

## Base URL

All URLs referenced in this documentation have the following base:

```
https://api.waterreporter.org
```

The Water Reporter API is served over HTTPS. To ensure data privacy, unencrypted HTTP is not supported.

## Resources

* [Datasets](#datasets)
* [Readings](#readings)
* [Parameters](#parameters)
* [Stations](#stations)
* [Watersheds](#watersheds)

### Datasets

#### Retrieve a list of data sources

`GET /datasets`

Retrieve a collection of data sources belonging to an organization.

**Parameters**

| Name | Type| Description |
| :--- | :--- | :--- |
| `access_token`<br /><sub>required</sub> | string |  Your Water Reporter access token. | |

**Request**

```
GET https://api.waterreporter.org/datasets?access_token={token}
```

**Response**

```json
```

#### Retrieve a single data source

`GET /datasets/:id`

Retrieve a data source.

**Parameters**

| Name | Type| Description |
| :--- | :--- | :--- |
| `access_token`<br /><sub>required</sub> | string |  Your Water Reporter access token. |

**Request**

```
GET https://api.waterreporter.org/datasets/1?access_token={token}
```

**Response**

```json
{
    "contributor_count": 5,
    "created": "2021-01-01T00:00:00",
    "description": "...",
    "id": 1,
    "last_sampled": "2021-01-01T00:00:00",
    "name": "Most Recent Data",
    "organization": {
        "description": "...",
        "id": 1,
        "logo_url": "https://img.waterreporter.org/8b14473a5c114cf7920bab13f7b8753d_square.png",
        "name": "Blue Water Baltimore"
    },
    "parameter_count": 10,
    "private": true,
    "reading_count": 10,
    "sample_count": 1,
    "station_count": 100,
    "stub": "f2f899a4b4a2ea70",
    "updated": "2021-01-01T00:00:00"
}
```

### Readings

#### Retrieve a list of readings

`GET /readings`

Retrieve a collection of measurements for a given environmental parameter at a specific monitoring location (station).

**Parameters**

| Name | Type| Description |
| :--- | :--- | :--- |
| `access_token`<br /><sub>required</sub> | string |  Your Water Reporter access token. | |
| `station_id`<br /><sub>required</sub> | integer | The primary key of a station. | |
| `parameter_id`<br /><sub>required</sub> | integer | The primary key of a parameter. The parameter and station must belong to the same data source. | |
| `date_format` | string | A string that specifies the desired format of all timestamps in the response object. Using `epoch` will format timestamps in seconds since the Unix epoch. Allowed values are `iso` and `epoch`. **Default:** `iso` |
| `page` | integer | The page number of a result set. **Default:** `1` |
| `limit` | integer | The number of readings to return. The maximum is `100`. **Default:** `10` |
| `start_date` | date | Return readings on or after this date. Required format: `yyyy-mm-dd` | |
| `end_date` | date | Return readings before this date. Required format: `yyyy-mm-dd` | |
| `min_value` | integer or float | Return readings greater than or equal to this value. | |
| `max_value` | integer or float | Return readings less than or equal to this value. | |
| `colorize` | boolean | Include threshold colors with reading data. Accepted values are `false`, `0`, `true`, `1`. **Default:** `true` |
| `label` | boolean | Include threshold labels and descriptions with reading data. Accepted values are `false`, `0`, `true`, `1`. **Default:** `false` |
| `include_graph` | boolean | Include minimal data source, station, and parameter dictionaries in the top-level of the response object. Accepted values are `false`, `0`, `true`, `1`. **Default:** `true` |

**Request**

```
GET https://api.waterreporter.org/readings?station_id=1&parameter_id=1&access_token={token}
```

**Response**

```json
{
    "data": [{
            "certified": true,
            "collection_date": "2021-01-01T00:00:00",
            "color": "#008000",
            "dataset_id": 1,
            "id": 1,
            "parameter_id": 1,
            "sample_key": "88c7ee59208722e0548e1a516943e4f4",
            "station_id": 1,
            "user": "Alice Smith",
            "value": 118.7
        },
        "..."
    ],
    "dataset": {
        "id": 1,
        "name": "2021 Swimming Conditions",
        "stub": "1ES07dJvbrH4"
    },
    "parameter": {
        "alias": null,
        "chart_schema": {
            "ranges": [{
                    "color": "#da222b",
                    "description": "Bacteria measurements exceed healthy limits.",
                    "height": null,
                    "label": "High Caution",
                    "lower_bound": 235,
                    "lower_op": {
                        "label": "greater than",
                        "op": "gt",
                        "symbol": ">"
                    }
                },
                {
                    "color": "#008000",
                    "description": "Bacteria measurements are within healthy limits.",
                    "height": 400,
                    "label": "Fair",
                    "lower_bound": 0,
                    "lower_op": {
                        "label": "greater than or equal to",
                        "op": "gte",
                        "symbol": ">="
                    },
                    "upper_bound": 235,
                    "upper_op": {
                        "label": "less than or equal to",
                        "op": "lte",
                        "symbol": "<="
                    }
                }
            ]
        },
        "display_name": "E. coli Concentration (CFU/100 mL)",
        "id": 1,
        "normalized_name": "e_coli_concentration",
        "plottable": true
    },
    "station": {
        "dataset_id": 1,
        "hibernate": false,
        "id": 1,
        "is_active": true,
        "key": "b5c6dcce3abed32c43b1ebcab1fa648b",
        "name": "City Point",
        "raw_id": "A01"
    },
    "summary": {
        "count": 10,
        "page": 1,
        "total_pages": 1
    },
    "unit": {
        "detail": "Colony Forming Units per 100 Milliliters",
        "id": 1,
        "notation": null
    }
}
```

#### Retrieve a single reading

`GET /readings/:id`

Retrieve a single measurement for a given environmental parameter at a specific location (station).

**Parameters**

| Name | Type| Description |
| :--- | :--- | :--- |
| `access_token`<br /><sub>required</sub> | string |  Your Water Reporter access token. |
| `date_format` | string | A string that specifies the desired format of all timestamps in the response object. Using `epoch` will format timestamps in seconds since the Unix epoch. Allowed values are `iso` and `epoch`. **Default:** `iso` |

**Request**

```
GET https://api.waterreporter.org/readings/1?access_token={token}
```

**Response**

```json
{
    "certified": true,
    "collection_date": "2021-01-01T00:00:00",
    "color": "#008000",
    "dataset": {
        "id": 1,
        "name": "2021 Swimming Conditions",
        "stub": "1ES07dJvbrH4"
    },
    "id": 1,
    "parameter": {
        "alias": null,
        "chart_schema": {
            "ranges": [{
                    "color": "#da222b",
                    "description": "Bacteria measurements exceed healthy limits.",
                    "height": null,
                    "label": "High Caution",
                    "lower_bound": 235,
                    "lower_op": {
                        "label": "greater than",
                        "op": "gt",
                        "symbol": ">"
                    }
                },
                {
                    "color": "#008000",
                    "description": "Bacteria measurements are within healthy limits.",
                    "height": 400,
                    "label": "Fair",
                    "lower_bound": 0,
                    "lower_op": {
                        "label": "greater than or equal to",
                        "op": "gte",
                        "symbol": ">="
                    },
                    "upper_bound": 235,
                    "upper_op": {
                        "label": "less than or equal to",
                        "op": "lte",
                        "symbol": "<="
                    }
                }
            ]
        },
        "display_name": "E. coli Concentration (CFU/100 mL)",
        "id": 1,
        "normalized_name": "e_coli_concentration",
        "plottable": true
    },
    "sample_key": "88c7ee59208722e0548e1a516943e4f4",
    "station": {
        "dataset_id": 1,
        "hibernate": false,
        "id": 1,
        "is_active": true,
        "key": "b5c6dcce3abed32c43b1ebcab1fa648b",
        "name": "City Point",
        "raw_id": "A01"
    },
    "unit": {
        "detail": "Colony Forming Units per 100 Milliliters",
        "id": 1,
        "notation": null
    },
    "user": "Alice Smith",
    "value": 118.7
}
```

### Parameters

#### Retrieve a list of parameters

`GET /parameters`

Retrieve a collection of environmental parameters from a single data source or station.

**Parameters**

| Name | Type| Description |
| :--- | :--- | :--- |
| `access_token`<br /><sub>required</sub> | string |  Your Water Reporter access token. |
| `dataset_id`<br /><sub>required</sub> | integer |  The primary key of a data source. |
| `station_id`<br /><sub>required</sub> | integer |  The primary key of a station. |
| `date_format` | string | A string that specifies the desired format of all timestamps in the response object. Using `epoch` will format timestamps in seconds since the Unix epoch. Allowed values are `iso` and `epoch`. **Default:** `iso` |

**Request**

```
GET https://api.waterreporter.org/parameters?dataset_id=1&station_id=1&access_token={token}
```

**Response**

```json
{
    "features": [{
            "alias": null,
            "chart_schema": {
                "ranges": [{
                        "color": "#26D72A",
                        "height": null,
                        "label": "Pass",
                        "lower_bound": 5,
                        "lower_op": {
                            "label": "greater than",
                            "op": "gt",
                            "symbol": ">"
                        }
                    },
                    {
                        "color": "#D62D31",
                        "draw_threshold": true,
                        "height": 400,
                        "label": "Fail",
                        "lower_bound": 0,
                        "lower_op": {
                            "label": "greater than or equal to",
                            "op": "gte",
                            "symbol": ">="
                        },
                        "upper_bound": 5,
                        "upper_op": {
                            "label": "less than or equal to",
                            "op": "lte",
                            "symbol": "<="
                        }
                    }
                ]
            },
            "created": "2021-01-01T00:00:00",
            "id": 1,
            "last_sampled": "2021-01-01T00:00:00",
            "max": 19.11,
            "mean": 9.3936,
            "min": 0.0,
            "name": "Dissolved Oxygen",
            "newest_value": 9.03,
            "normalized_name": "dissolved_oxygen_mg_l",
            "oldest_value": 11.88,
            "plottable": true,
            "reading_count": 5676,
            "sample_count": 5440,
            "station_count": 51,
            "unit": "Milligrams per Liter",
            "updated": "2021-01-01T00:00:00"
        },
        "..."
    ]
}
```

#### Retrieve a single parameter

`GET /parameters/:id`  

Retrieve a single environmental parameter.

**Parameters**

| Name | Type| Description |
| :--- | :--- | :--- |
| `access_token`<br /><sub>required</sub> | string |  Your Water Reporter access token. |
| `station_id` | integer |  The primary key of a station. Use this parameter to constrain the scope of statistical computations for `reading_count`, `station_count`, `sample_count`, `min`, `max`, `mean`, `standard deviation`, and `variance`. |
| `date_format` | string | A string that specifies the desired format of all timestamps in the response object. Using `epoch` will format timestamps in seconds since the Unix epoch. Allowed values are `iso` and `epoch`. **Default:** `iso` |

**Request**

```
GET https://api.waterreporter.org/parameters/1?access_token={token}
```

**Response**

```json
{
    "alias": null,
    "chart_schema": {
        "ranges": [{
                "color": "#26D72A",
                "height": null,
                "label": "Pass",
                "lower_bound": 5,
                "lower_op": {
                    "label": "greater than",
                    "op": "gt",
                    "symbol": ">"
                }
            },
            {
                "color": "#D62D31",
                "draw_threshold": true,
                "height": 400,
                "label": "Fail",
                "lower_bound": 0,
                "lower_op": {
                    "label": "greater than or equal to",
                    "op": "gte",
                    "symbol": ">="
                },
                "upper_bound": 5,
                "upper_op": {
                    "label": "less than or equal to",
                    "op": "lte",
                    "symbol": "<="
                }
            }
        ]
    },
    "created": "2021-01-01T00:00:00",
    "id": 1,
    "last_sampled": "2021-01-01T00:00:00",
    "max": 19.11,
    "mean": 9.39,
    "min": 0.0,
    "name": "Dissolved Oxygen",
    "newest_value": 7.65,
    "normalized_name": "dissolved_oxygen_mg_l",
    "oldest_value": 11.35,
    "plottable": true,
    "reading_count": 5676,
    "sample_count": 5440,
    "station_count": 51,
    "std": 2.92,
    "unit": "Milligrams per Liter",
    "updated": "2021-01-01T00:00:00",
    "var": 8.52
}
```

### Stations

#### Retrieve a list of stations

`GET /stations`

Retrieve a collection of stations (monitoring locations).

**Parameters**

| Name | Type| Description |
| :--- | :--- | :--- |
| `access_token`<br /><sub>required</sub> | string |  Your Water Reporter access token. |
| `sets`<br /><sub>required</sub> | string | A comma-separated list of one or more data source identifiers. Required if `orgs` not present. |
| `orgs`<br /><sub>required</sub> | string | A comma-separated list of one or more organization identifiers. Required if `sets` not present. |
| `date_format` | string | A string that specifies the desired format of all timestamps in the response object. Using `epoch` will format timestamps in seconds since the Unix epoch. Allowed values are `iso` and `epoch`. **Default:** `iso` |
| `geo_format` | string | A string that specifies the desired geometry transformation. Use `xy` to retrieve station coordinates as separate `lat` and `lng` values. Allowed values are `geojson` and `xy`. **Default:** `geojson` |
| `bbox` | string | A comma-separated list of bounding box coordinates in the format `minX,minY,maxX,maxY`. Example: `-76.616539,39.269442,-76.58255,39.291366`  |

**Request**

```
GET https://api.waterreporter.org/stations?sets=1&access_token={token}
```

**Response**

```json
{
    "features": [{
            "created": "2021-01-01T00:00:00",
            "dataset_id": 1,
            "description": "...",
            "hibernate": false,
            "huc_12": "Bailey Creek-James River",
            "id": 1,
            "image_url": "https://images.waterreporter.org/5151/fef367c3928d45d691cc8aafd8c4021f_original.jpg",
            "is_active": true,
            "key": "b5c6dcce3abed32c43b1ebcab1fa648b",
            "last_sampled": "2021-01-01T00:00:00",
            "lat": 37.316523,
            "lng": -77.273569,
            "name": "Station A",
            "organization_id": 1,
            "parameter_count": 5,
            "raw_id": "A01",
            "reading_count": 100,
            "sample_count": 20,
            "updated": "2021-01-01T00:00:00"
        },
        "..."
    ]
}
```

#### Retrieve a single station

`GET /stations/:id`

Retrieve a station. A station represents a single monitoring location.

**Parameters**

| Name | Type| Description |
| :--- | :--- | :--- |
| `access_token`<br /><sub>required</sub> | string |  Your Water Reporter access token. |
| `date_format` | string | A string that specifies the desired format of all timestamps in the response object. Using `epoch` will format timestamps in seconds since the Unix epoch. Allowed values are `iso` and `epoch`. **Default:** `iso` |
| `geo_format` | string | A string that specifies the desired geometry transformation. Use `xy` to retrieve station coordinates as separate `lat` and `lng` values. Allowed values are `geojson` and `xy`. **Default:** `geojson` |
| `nn` | boolean | If true, the response will include a station's four nearest neighbors in the same data source (if any). The `neighbors` object will contain a dictionary that specifies the stations immediately to the east and west of the target station. Allowed values are `true` and `false`. **Default:** `false` |

**Request**

```
GET https://api.waterreporter.org/stations/1?geo_format=xy&nn=true&access_token={token}
```

**Response**

```json
{
    "created": "2021-01-01T00:00:00",
    "dataset_id": 1,
    "description": "...",
    "hibernate": false,
    "huc_12": "Northwest Harbor-Patapsco River",
    "id": 1,
    "image_url": "...",
    "is_active": true,
    "key": "79f255537964bafb",
    "last_sampled": "2021-01-01T00:00:00",
    "lat": 39.28499,
    "lng": -76.609811,
    "name": "Dragon Boats",
    "neighbors": {
        "all": [{
                "geometry": {
                    "coordinates": [
                        -76.623106,
                        39.263507
                    ],
                    "type": "Point"
                },
                "id": 2,
                "lng": -76.623106,
                "name": "Middle Branch A"
            },
            {
                "geometry": {
                    "coordinates": [
                        -76.611053,
                        39.282422
                    ],
                    "type": "Point"
                },
                "id": 3,
                "lng": -76.611053,
                "name": "Science Center"
            },
            {
                "geometry": {
                    "coordinates": [
                        -76.603718,
                        39.283701
                    ],
                    "type": "Point"
                },
                "id": 4,
                "lng": -76.603718,
                "name": "Jones Falls Outlet"
            },
            {
                "geometry": {
                    "coordinates": [
                        -76.596738,
                        39.276857
                    ],
                    "type": "Point"
                },
                "id": 5,
                "lng": -76.596738,
                "name": "Northwest Branch A"
            }
        ],
        "immediate": {
            "east": {
                "geometry": {
                    "coordinates": [
                        -76.603718,
                        39.283701
                    ],
                    "type": "Point"
                },
                "id": 4,
                "name": "Jones Falls Outlet"
            },
            "west": {
                "geometry": {
                    "coordinates": [
                        -76.611053,
                        39.282422
                    ],
                    "type": "Point"
                },
                "id": 3,
                "name": "Science Center"
            }
        }
    },
    "organization_id": 1,
    "parameter_count": 10,
    "parameters": [
        "..."
    ],
    "raw_id": "Dragon Boats",
    "reading_count": 100,
    "sample_count": 10,
    "updated": "2021-01-01T00:00:00"
}
```

#### Retrieve the station closest to a geographic point

`GET /stations/nearest`

Retrieve the station closest to a geographic point. A station represents a single monitoring location.

**Parameters**

| Name | Type| Description |
| :--- | :--- | :--- |
| `access_token`<br /><sub>required</sub> | string |  Your Water Reporter access token. |
| `lng`<br /><sub>required</sub> | integer or float | A geographic coordinate that specifies the east–west position of a point on the Earth's surface. Must be in decimal degrees. |
| `lat`<br /><sub>required</sub> | integer or float | A geographic coordinate that specifies the north–south position of a point on the Earth's surface. Must be in decimal degrees. |
| `date_format` | string | A string that specifies the desired format of all timestamps in the response object. Using `epoch` will format timestamps in seconds since the Unix epoch. Allowed values are `iso` and `epoch`. **Default:** `iso` |
| `geo_format` | string | A string that specifies the desired geometry transformation. Use `xy` to retrieve station coordinates as separate `lat` and `lng` values. Allowed values are `geojson` and `xy`. **Default:** `geojson` |
| `nn` | boolean | If true, the response will include a station's four nearest neighbors in the same data source (if any). The `neighbors` object will contain a dictionary that specifies the stations immediately to the east and west of the target station. Allowed values are `true` and `false`. **Default:** `false` |

**Request**

```
GET https://api.waterreporter.org/stations/1?geo_format=xy&nn=true&access_token={token}
```

**Response**

```json
{
    "created": "2021-01-01T00:00:00",
    "dataset_id": 1,
    "description": "...",
    "hibernate": false,
    "huc_12": "Northwest Harbor-Patapsco River",
    "id": 1,
    "image_url": "...",
    "is_active": true,
    "key": "79f255537964bafb",
    "last_sampled": "2021-01-01T00:00:00",
    "lat": 39.28499,
    "lng": -76.609811,
    "name": "Dragon Boats",
    "neighbors": {
        "all": [{
                "geometry": {
                    "coordinates": [
                        -76.623106,
                        39.263507
                    ],
                    "type": "Point"
                },
                "id": 2,
                "lng": -76.623106,
                "name": "Middle Branch A"
            },
            {
                "geometry": {
                    "coordinates": [
                        -76.611053,
                        39.282422
                    ],
                    "type": "Point"
                },
                "id": 3,
                "lng": -76.611053,
                "name": "Science Center"
            },
            {
                "geometry": {
                    "coordinates": [
                        -76.603718,
                        39.283701
                    ],
                    "type": "Point"
                },
                "id": 4,
                "lng": -76.603718,
                "name": "Jones Falls Outlet"
            },
            {
                "geometry": {
                    "coordinates": [
                        -76.596738,
                        39.276857
                    ],
                    "type": "Point"
                },
                "id": 5,
                "lng": -76.596738,
                "name": "Northwest Branch A"
            }
        ],
        "immediate": {
            "east": {
                "geometry": {
                    "coordinates": [
                        -76.603718,
                        39.283701
                    ],
                    "type": "Point"
                },
                "id": 4,
                "name": "Jones Falls Outlet"
            },
            "west": {
                "geometry": {
                    "coordinates": [
                        -76.611053,
                        39.282422
                    ],
                    "type": "Point"
                },
                "id": 3,
                "name": "Science Center"
            }
        }
    },
    "organization_id": 1,
    "parameter_count": 10,
    "parameters": [
        "..."
    ],
    "raw_id": "Dragon Boats",
    "reading_count": 100,
    "sample_count": 10,
    "updated": "2021-01-01T00:00:00"
}
```

### Watersheds

#### Spatial search

`GET /watersheds/intersect`

Retrieve a single watershed using a coordinate pair (longitude, latitude).

**Parameters**

| Name | Type| Description |
| :--- | :--- | :--- |
| `access_token`<br /><sub>required</sub> | string |  Your Water Reporter access token. |
| `lng`<br /><sub>required</sub> | integer or float | A geographic coordinate that specifies the east–west position of a point on the Earth's surface. Must be in decimal degrees. |
| `lat`<br /><sub>required</sub> | integer or float | A geographic coordinate that specifies the north–south position of a point on the Earth's surface. Must be in decimal degrees. |

**Request**

```
GET https://api.waterreporter.org/watersheds?access_token={token}&lng=-77.113056&lat=38.984722
```

**Response**

```json
{
	"feature": {
		"huc_10": {
			"code": "0207001001",
			"name": "Rock Creek-Potomac River"
		},
		"huc_12": {
			"code": "020700100102",
			"name": "Lower Rock Creek"
		},
		"huc_6": {
			"code": "020700",
			"name": "Potomac"
		},
		"huc_8": {
			"code": "02070010",
			"name": "Middle Potomac-Anacostia-Occoquan"
		}
	}
}
```

#### Retrieve a single watershed

`GET /watersheds/:code`

Retrieve a watershed using a 6-, 8-, 10-, or 12-digit [hydrologic unit code](https://nas.er.usgs.gov/hucs.aspx).

**Parameters**

| Name | Type| Description |
| :--- | :--- | :--- |
| `access_token`<br /><sub>required</sub> | string |  Your Water Reporter access token. |

**Request**

```
GET https://api.waterreporter.org/watersheds/020700&access_token={token}
```

**Response**

```json
{
	"feature": {
		"huc_10": {
			"code": "0207001001",
			"name": "Rock Creek-Potomac River"
		},
		"huc_12": {
			"code": "020700100102",
			"name": "Lower Rock Creek"
		},
		"huc_6": {
			"code": "020700",
			"name": "Potomac"
		},
		"huc_8": {
			"code": "02070010",
			"name": "Middle Potomac-Anacostia-Occoquan"
		}
	}
}
```

