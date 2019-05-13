# Automower API Notes
	
Simple notes from poking around the Automower API as of 5/1/2019. A good portion of the endpoints and how to authenticate were found [here](https://github.com/chrisz/pyhusmow). There could be more undiscovered endpoints as well.

### URLs
  
- Authentication/Token URL: `https://iam-api.dss.husqvarnagroup.net/api/v3/`
- API URL: `https://amc-api.dss.husqvarnagroup.net/v1/`

### Authetication Headers

Token data can be retried from the `token` endpoint.
- Authorization: 'Bearer `token.data.id`'
- Authorization-Provider: '`token.attributes.provider`'

<br/><br/>
## token
  Uses the authentiation URL

* **URL**

	/token

* **Method:**
  
	`POST`
  
*  **URL Params**


* **Data Params**

	```json
  {
    "data": {
      "attributes": {
        "password": "password",
        "username": "username"
      },
      "type": "token"
    }
  }
	```

* **Success Response:**
  
	* **Code:** 200
	* **Content:**
	```json
  {
    "data": {
      "id": "00112233-4455-6677-8899-aabbccddeeff",
      "type": "token",
      "attributes": {
        "expires_in": 123456,
        "refresh_token": "00112233-4455-6677-8899-aabbccddeeff",
        "provider": "husqvarna",
        "user_id": "00112233-4455-6677-8899-aabbccddeeff",
        "scope": "iam:read iam:write"
      }
    }
  }
	```

* **Sample Call:**

	```
	POST /api/v3/token HTTP/1.1
	Host: iam-api.dss.husqvarnagroup.net
	Accept: application/json
	Content-Type: application/json

  {
    "data": {
        "attributes": {
        "password": "************",
        "username": "name@email.com"
      },
      "type": "token"
    }
  }
	```
<br/><br/>
## mowers
  Uses the API URL

* **URL**

	/mowers

* **Method:**
  
	`GET`
  
*  **URL Params**

* **Data Params**

* **Success Response:**
  
	* **Code:** 200 <br />
	* **Content:**
	```json
  [
    {
      "id": "123456789-123456789",
      "name": "NoMow",
      "model": "G",
      "valueFound": true,
      "status": {
        "batteryPercent": 70,
        "connected": true,
        "lastErrorCode": 80,
        "lastErrorCodeTimestamp": 0123456789,
        "mowerStatus": "OK_CUTTING_NOT_AUTO",
        "nextStartSource": "NO_SOURCE",
        "nextStartTimestamp": 0,
        "operatingMode": "AUTO",
        "storedTimestamp": 0123456789012,
        "showAsDisconnected": false,
        "valueFound": true
      }
    }
  ]
	```

* **Sample Call:**

	```
	GET /v1/mowers HTTP/1.1
	Host: amc-api.dss.husqvarnagroup.net
	Accept: application/json
	Authorization: Bearer 00112233-4455-6677-8899-aabbccddeeff
	Authorization-Provider: husqvarna
	```
<br/><br/>
## status
  Uses the API URL

* **URL**

	/mowers/`mowers.id`/status

* **Method:**
  
	`GET`
  
*  **URL Params**

* **Data Params**

* **Success Response:**
  
	* **Code:** 200 <br />
	* **Content:**
	```json
  {
    "batteryPercent": 36,
    "connected": true,
    "lastErrorCode": 0,
    "lastErrorCodeTimestamp": 0,
    "mowerStatus": "OK_CUTTING",
    "nextStartSource": "NO_SOURCE",
    "nextStartTimestamp": 0,
    "operatingMode": "AUTO",
    "storedTimestamp": 0123456789012,
    "showAsDisconnected": false,
    "valueFound": true,
    "cachedSettingsUUID": "00112233-4455-6677-8899-aabbccddeeff",
    "lastLocations": [
      {
        "latitude": 999.99999999999999,
        "longitude": -999.99999999999999,
        "gpsStatus": "USING_GPS_MAP"
      },...
    ]
  }	
	```

* **Sample Call:**

	```
	GET /v1/mowers/123456789-123456789/status HTTP/1.1
	Host: amc-api.dss.husqvarnagroup.net
	Accept: application/json
	Authorization: Bearer 00112233-4455-6677-8899-aabbccddeeff
	Authorization-Provider: husqvarna
	```
<br/><br/>	
## geofence
  Uses the API URL

* **URL**

	/mowers/`mowers.id`/geofence

* **Method:**
  
	`GET`
  
*  **URL Params**

* **Data Params**

* **Success Response:**
  
	* **Code:** 200 <br />
	* **Content:**
	```json
  {
    "centralPoint": {
      "location": {
        "latitude": 999.99999999999999,
        "longitude": -999.99999999999999,
        "gpsStatus": null
      },
      "sensitivity": {
        "level": 4,
        "radius": 500
      }
    },
    "lastLocations": [
      {
        "latitude": 999.99999999999999,
        "longitude": -999.99999999999999,
        "gpsStatus": "USING_GPS_MAP"
      },...
    ],
    "valueFound": true
  }	
	```

* **Sample Call:**

	```
	GET /v1/mowers/123456789-123456789/geofence HTTP/1.1
	Host: amc-api.dss.husqvarnagroup.net
	Accept: application/json
	Authorization: Bearer 00112233-4455-6677-8899-aabbccddeeff
	Authorization-Provider: husqvarna
	```
<br/><br/>
## control
  Uses the API URL

* **URL**

	/mowers/`mowers.id`/control

* **Method:**
  
	`POST`
  
*  **URL Params**

* **Data Params**

	This is only what I've found so far but there's probably more that we can provide in this body, like duration and overrides.

	```json
  {
    "action": "`PARK|STOP|START`"
  }
	```

* **Success Response:**
  
	* **Code:** 200 <br />
	**Content:**
	```json
  {
    "status": "OK",
    "commandId": "00112233-4455-6677-8899-aabbccddeeff",
    "errorCode": null
  }
	```

* **Sample Call:**

	```
	POST /v1/mowers/123456789-123456789/control HTTP/1.1
	Host: amc-api.dss.husqvarnagroup.net
	Accept: application/json
	Authorization: Bearer 00112233-4455-6677-8899-aabbccddeeff
	Authorization-Provider: husqvarna
	Content-Type: application/json

  {
    "action": "START"
  }
	```
<br/><br/>	
## settings
  Uses the API URL. I've not tried POSTing to this endpoint, but I assume it's possible to update settings using a different method like POST or PATCH.

* **URL**

	/mowers/`mowers.id`/settings

* **Method:**
  
	`GET`
  
*  **URL Params**

* **Data Params**

* **Success Response:**
  
	* **Code:** 200 <br />
	* **Content:**
	```json
  {
    "settings": [
      {
        "id": "installation.area2.proportion",
        "value": 20
      },
      {
        "id": "installation.area5.chargingStationRange",
        "value": "GUIDE_1"
      },
      {
        "id": "installation.guide3DelayTime",
        "value": 3
      },
      {
        "id": "accessories.avoidCollision",
        "value": 0
      },
      {
        "id": "installation.followGuide3Home",
        "value": false
      },
      {
        "id": "accessories.schedule",
        "value": "ALWAYS_OFF"
      },
      {
        "id": "installation.followGuide2Home",
        "value": true
      },
      {
        "id": "installation.area3.runningDistance",
        "value": 1
      },
      {
        "id": "installation.area2.enabled",
        "value": true
      },
      {
        "id": "installation.corridorWidthGuide3",
        "value": 9
      },
      {
        "id": "weatherTimer.cuttingTime",
        "value": 3
      },
      {
        "id": "installation.corridorWidthGuide2",
        "value": 9
      },
      {
        "id": "installation.area4.enabled",
        "value": false
      },
      {
        "id": "installation.corridorWidthGuide1",
        "value": 9
      },
      {
        "id": "installation.chargerStationRange",
        "value": 700
      },
      {
        "id": "installation.area5.proportion",
        "value": 0
      },
      {
        "id": "accessories.flashesWhenFault",
        "value": true
      },
      {
        "id": "cuttingHeight",
        "value": 4
      },
      {
        "id": "installation.followGuide1Home",
        "value": true
      },
      {
        "id": "installation.sector2ExitAngleMax",
        "value": 270
      },
      {
        "id": "general.runSpiralCutting",
        "value": true
      },
      {
        "id": "installation.corridorWidthBoundaryMin",
        "value": 1
      },
      {
        "id": "installation.area5.enabled",
        "value": false
      },
      {
        "id": "installation.sector1ExitAngleMax",
        "value": 270
      },
      {
        "id": "installation.sector1Proportion",
        "value": 100
      },
      {
        "id": "installation.area4.proportion",
        "value": 0
      },
      {
        "id": "installation.gpsAssistedNavigation",
        "value": 3
      },
      {
        "id": "installation.area3.proportion",
        "value": 0
      },
      {
        "id": "installation.area2.runningDistance",
        "value": 300
      },
      {
        "id": "installation.corridorWidthBoundaryMax",
        "value": 6
      },
      {
        "id": "installation.area2.chargingStationRange",
        "value": "GUIDE_2"
      },
      {
        "id": "installation.guide1DelayTime",
        "value": 3
      },
      {
        "id": "installation.area5.runningDistance",
        "value": 1
      },
      {
        "id": "installation.drivePastWire",
        "value": 350
      },
      {
        "id": "weatherTimer.runWeatherTimer",
        "value": false
      },
      {
        "id": "installation.boundaryDelay",
        "value": 11
      },
      {
        "id": "installation.area1.enabled",
        "value": true
      },
      {
        "id": "installation.reversingDistance",
        "value": 1210
      },
      {
        "id": "installation.sector2Proportion",
        "value": 100
      },
      {
        "id": "installation.sector1ExitAngleMin",
        "value": 90
      },
      {
        "id": "installation.sector2ExitAngleMin",
        "value": 90
      },
      {
        "id": "installation.area1.proportion",
        "value": 20
      },
      {
        "id": "installation.area3.enabled",
        "value": false
      },
      {
        "id": "installation.guide2DelayTime",
        "value": 3
      },
      {
        "id": "installation.area3.chargingStationRange",
        "value": "GUIDE_1"
      },
      {
        "id": "installation.area4.runningDistance",
        "value": 1
      },
      {
        "id": "installation.area1.chargingStationRange",
        "value": "GUIDE_1"
      },
      {
        "id": "general.spiralCuttingIntensity",
        "value": 2
      },
      {
        "id": "general.runEcoMode",
        "value": false
      },
      {
        "id": "installation.area1.runningDistance",
        "value": 300
      },
      {
        "id": "installation.followBoundaryHome",
        "value": true
      },
      {
        "id": "installation.area4.chargingStationRange",
        "value": "GUIDE_1"
      }
    ]
  }	
	```

* **Sample Call:**

	```
	GET /v1/mowers/123456789-123456789/settings HTTP/1.1
	Host: amc-api.dss.husqvarnagroup.net
	Accept: application/json
	Authorization: Bearer 00112233-4455-6677-8899-aabbccddeeff
	Authorization-Provider: husqvarna
