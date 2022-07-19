# Using OKAPI for Opencaching websites
[OKAPI](https://www.opencaching.de/okapi/introduction.html) is a publically available API for "National Opencaching" sites. This includes most websites, with the most notable exception being [Geocaching.com](Geocaching.com.md).

-   It provides OC site with a set of useful well-documented API methods,
-   Allows external developers to easily **read public** Opencaching data,
-   Allows **read and write private** (user-related) data with OAuth Authentication.

The project has grown to become a standard and common API for all National Opencaching.xx sites.

## Examples
The below examples use Opencaching.de (note that KEY should be replaced with a consumer key).

- https://www.opencaching.de/okapi/services/users/by_username?consumer_key=KEY&username=jelle619&fields=uuid
	Request the UUID of username jelle619, returns e.g. `{"uuid":"403e1df6-f7d4-11ec-bc12-5959b47b4c0a"}`.
- https://www.opencaching.de/okapi/services/caches/search/all?consumer_key=KEY&found_by=403e1df6-f7d4-11ec-bc12-5959b47b4c0a
	Returns all cache codes found by a user with UUID 403e1df6-f7d4-11ec-bc12-5959b47b4c0a, returns e.g. `{"results":["OC31AB","OC3ECA","OC5DA8"],"more":false}`

## Leaflet map
It is possible to embed a map using Leaflet that displays information from an Opencaching website using OKAPI. For this example, we will consider a map showing markers on the locations where a particular user found a cache.

### Initialising the initial map
First of all, load the Leaflet CSS and JavaScript code (in that order).

```html
<!-- Note: To retrieve the most recent version, please consult the Leaflet website at https://leafletjs.com. -->

<!-- CSS -->
<link rel="stylesheet" href="https://unpkg.com/leaflet@1.8.0/dist/leaflet.css" integrity="sha512-hoalWLoI8r4UszCkZ5kL8vayOGVae1oxXe/2A4AO6J9+580uKHDO3JdHb7NzwwzK5xr/Fs0W40kiNHxM9vyTtQ==" crossorigin="" />

<!-- JavaScript -->
<script src="https://unpkg.com/leaflet@1.8.0/dist/leaflet.js" integrity="sha512-BB3hKbKWOc9Ez/TAwyWxNXeoV9c1v6FIeYiBieIWkpLjauysF18NzgR1MBNBXf8/KABdlkX68nAhlwcDFLGPCQ==" crossorigin=""></script>
```

Then, initialise a map in JavaScript. We will also need to define a tile provider. In this instance, we will use OpenStreetMap for this, as it can be freely used as long as you adhere to the [Tile Usage Policy](https://operations.osmfoundation.org/policies/tiles/). Alternatively, the OpenStreetMap Wiki has a [list containing also other tile providers](https://wiki.openstreetmap.org/wiki/Tile_servers).

```javascript
var map = L.map('map', {
  attributionControl: false,
  center: [51.690486599927794, 5.296194152088851],
  zoom: 19,
});

// Use OpenStreetMap as a tile provider
L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
    maxZoom: 19,
}).addTo(map);

```
And put an empty `div` element into the HTML code with an ID of `map`.
```html
<div id="map">
<!-- Put this in the position where you would like the map to load. -->
</div>
```

### Retrieving data through JavaScript
Before we can enrich the map with data from OKAPI, we first need to fetch the data. We will use XMLHttpRequest and Promises to achieve this. We will start off writing a function that will fetch the data, `getData()`.

```javascript
function getData() {
  return new Promise(function (resolve, reject) {
    let xhr = new XMLHttpRequest();
    xhr.open("GET", "URL");
    xhr.onload = function () {
      if (this.status >= 200 && this.status < 300) {
        resolve(xhr.response);
        return this.responseText;
      } else {
        reject({
          status: this.status,
          statusText: xhr.statusText
        });
      }
    };
    xhr.onerror = function () {
      reject({
        status: this.status,
        statusText: xhr.statusText
      });
    };
    xhr.send();
  });
}
```

URL should be replaced with an OKAPI GET request. In this case, we want to create a map of caches found by a certain user. We will use the [search_and_retrieve](https://www.opencaching.de/okapi/services/caches/shortcuts/search_and_retrieve.html) OKAPI server for this. An example URL would be (note that KEY should be replaced with a valid consumer key):

```
https://www.opencaching.de/okapi/services/caches/shortcuts/search_and_retrieve?consumer_key=KEY&search_method=services%2fcaches%2fsearch%2fall&search_params=%7B%22found_by%22%3A%228d641d70-ba05-11e9-9ecd-0aaa1aab5a07%22%2C%22limit%22%3A%22500%22%7D&retr_method=services%2fcaches%2fgeocaches&retr_params=%7B%22fields%22%3A%22location%22%2C%22limit%22%3A%22500%22%7D&wrap=true&format=json
```

Which would return a JSON result like this:

```json
{"results":{"OC31AB":{"location":"50.6262|7.169583"},"OC3ECA":{"location":"50.84015|6.05495"},"OC5DA8":{"location":"50.867|6.09"},"OC5DA9":{"location":"50.8585|6.090433"}},"more":false}
```

Now, lets write some JavaScript code that parses these results. This is why we use a Promise: XMLHttpRequest is an asynchronous fuction, yet we want to be sure the data has been fetched before the parsing of the results starts.

``` javascript
async function addMarkers() {
  response = JSON.parse(await getData());
  results = response["results"];
  for (var cache in results) {
    // Add marker for every cache
    var marker = L.marker(results[cache]["location"].split("|")).addTo(map);
  }
}

addMarkers();
```

The final result is a Leaflet map with markers for found caches rendered in the browser.

![Leaflet map with markers for found caches](assets/opencaching_map.webp)