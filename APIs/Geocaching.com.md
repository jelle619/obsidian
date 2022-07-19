# Geocaching.com
Geocaching.com, owned by Groundspeak Inc., began operating in late 2000. With a worldwide membership and a freemium business model, the website claims millions of caches and members in over 200 countries. Hides and events are reviewed by volunteer regional cache reviewers before publication. Free membership allows users access to coordinates, descriptions, and logs for some caches; for a fee, users are allowed additional search tools, the ability to download large amounts of cache information onto their gps at once, instant email notifications about new caches, and access to premium-member-only caches. Geocaching Headquarters are located in Seattle, Washington, United States.

## Scrapable web pages
The following web pages can potentially be scraped. These require a valid session cookie (the user must be logged in with any Geocaching.com account). Some examples are:

- https://www.geocaching.com/play/results/?fb=example&asc=false&sort=founddate
	Show caches found by user example sorted by date in descending order.
- https://www.geocaching.com/play/results/?ho=0&hb=example&asc=false&sort=founddate
	Show caches that were hidden by user example sorted by date in descending order.
- https://www.geocaching.com/play/results/?st=51.690484642318644%2C+5.296198098134734&oid=-1&ot=query&asc=false&sort=founddate
	Show caches within 10 miles of the coordinates 51.690484642318644, 5.296198098134734 sorted by date in descending order.

## Web API
*Note: This method can only be used if [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) is not enforced.*

Geocaching.com has server addresses available that respond with JSON in case a valid session cookie is present (the user is logged in with any Geocaching.com account). C:GEO, a Geocaching app for Android, makes use of this to allow users to interact with Geocaching.com caches, and the code responsible for the interaction can be found on [GitHub](https://github.com/cgeo/cgeo/tree/master/main/src/cgeo/geocaching/connector/gc). The base URL of the Web API is as follows:

	https://www.geocaching.com/api/proxy/web/search/v2

By default, it returns with a list of 50 caches, and the total amount of results, for example:

```json
{
  "results": [
    {
      "id": 560116,
      "name": "Geist des Hagen",
      "code": "GC11JM6",
      "premiumOnly": false,
      "favoritePoints": 9372,
      "geocacheType": 2,
      "containerType": 3,
      "difficulty": 1.5,
      "terrain": 2,
      "userFound": false,
      "userDidNotFind": false,
      "cacheStatus": 0,
      "postedCoordinates": {
        "latitude": 49.64995,
        "longitude": 8.50425
      },
      "detailsUrl": "/geocache/GC11JM6",
      "hasGeotour": false,
      "hasLogDraft": false,
      "placedDate": "2007-03-20T00:00:00",
      "owner": {
        "code": "PR114N8",
        "username": "die Helden"
      },
      "lastFoundDate": "2022-07-18T12:00:00",
      "trackableCount": 3,
      "region": "Hessen",
      "country": "Germany",
      "attributes": [
        {
          "id": 6,
          "name": "Recommended for kids",
          "isApplicable": true
        },
        {
          "id": 13,
          "name": "Available 24/7",
          "isApplicable": true
        },
        {
          "id": 7,
          "name": "Takes less than one hour",
          "isApplicable": true
        },
        {
          "id": 25,
          "name": "Parking nearby",
          "isApplicable": true
        },
        {
          "id": 55,
          "name": "Short hike (<1 km)",
          "isApplicable": true
        },
        {
          "id": 19,
          "name": "Ticks",
          "isApplicable": true
        }
      ]
    },
    
    ...

  ],
  "total": 3362794
}
```

The query parameters follow the same convention as the scrapable web pages mentioned earlier. So for example, one of such URLs like, for example:

	https://www.geocaching.com/play/results/?fb=example&asc=false&sort=founddate
	Show caches found by user example sorted by date in descending order.

would become

	https://www.geocaching.com/api/proxy/web/search/v2?fb=jelle619&asc=false&sort=founddate
	Show caches found by user example sorted by date in descending order (using Web API).

and give the same results, but in JSON format.