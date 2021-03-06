SPECIES INVADERS
================

An "invasive species" RESTful API for [Random Hacks of Kindness](http://www.rhok.org/problems/invasive-species-identification).

OVERVIEW
--------

Proposed use-cases for apps/sites/widgets which would use this API:

- Browse by "activity" (e.g. "camping", "hiking", "canoeing", "fishing", etc.)
- "I'm going from/to here, what should I watch out for?"
- "I'm here, what should I watch out for?"
- "I see something, what is it? Is it invasive?"
- "This place has something invasive, others should be aware!"
- The ability to administer the data sets

We've developed a RESTful API in PHP, initially responding w/JSON data, and a proof-of-concept JavaScript widget. View further [solution details at RHoK](http://www.rhok.org/solutions/species-invaders-api) and the [presentation at Speaker Deck](https://speakerdeck.com/u/morgant/p/species-invaders).

API SCHEMA
----------

Fetch data via GET & create/update data via POST. Querying specific data from an endpoint can be done by including appropriate URL parameters. Returns JSON, possibly JSON-P if a callback is specified in the query string. Uses appropriate HTTP response codes (200 for success, 400 for bad requests, 404 for request not found, 500 for internal server errors, etc.)

### GET Endpoints:

- /species (returns array of all species IDs)*
  - /species/id/{id} (returns a single species object by id)*
  - /species/name/{name} (returns a single species object by species)**
  - /species/common_name/{common_name} (returns a single species object by common name)**
  - /species/polygon/{polygon} (find species intersecting with polygon; returns array of species ids)**
  - /species/search/{search query} (find species by partial string match, specifically kindom..species & common names; returns an array of species ids)*
  - /species/native_locations/{id} (returns array of native location ids for a species by id)*
  - /species/invading_locations/{id} (returns array of invading location ids for a species by id)*
- /locations (returns array of all location IDs)*
  - /locations/id/{id} (returns a single location object by id)*
  - /locations/polygon/{polygon} (find locations intersecting with polygon; returns array of location ids)**
- /activity (return an array of all activity ids)**
  - /activity/id/{id} (return a single activity object by id)**
  - /activity/name/{name} (return a single activity object by name)**
  - /activity/search/{search query} (return an array of activity ids by partial string match against name & extra_info)**

### POST Endpoints:

- /species (add a new species; parameters 'kingdom'..'species', 'extra_notes'])* 
  - /species/id (update species by id; parameters: 'id' [required], 'kingdom'..'species', 'extra_notes')**
  - /species/name{name} (update species by name; parameters: 'id' [required], 'kingdom'..'species', 'extra_notes')**
  - /species/common_name (add common name; parameters: 'speciesid' [required], 'common_name' [required])**
  - /species/native_location/ (associate/disassociate a native location; parameters: 'speciesid' [required], 'locationid' [required], 'action' [required; values: 'add' or 'delete'])**†
  - /species/invading_location/ (associate/disassociate an invading location; parameters: 'speciesid' [required], 'locationid' [required], 'action' [required; values: 'add' or 'delete'])**†
- /locations (add a new location; parameters: 'name' [optional] & 'polygon' [required])*
  - /locations/id (update/remove a location by id; parameters: 'locationid' [required], 'name' [optional], 'polygon' [required])*
- /activity (add a new activity; parameters: 'name' [required], 'extra_info' [optional])**
  - /activity/id (update/remove an activity by id; parameters: 'activityid' [required], 'name' [required], 'extra_info' [optional])**

<sup>\*</sup> - Completed
<sup>**</sup> - Unimplemented or Incomplete
<sup>†</sup> - 'add/associate' support only.

IMPLEMENTATION CONSIDERATIONS
-----------------------------

We're developing in PHP and intend to use the [Wikipedia API](http://www.mediawiki.org/wiki/API) & [Google Maps API](https://developers.google.com/maps/documentation/), fortunately MySQL supports [geometry functions](http://dev.mysql.com/doc/refman/4.1/en/geometry-property-functions.html) so we can store/process our polygons natively there. We started with Scott Markoski's [Silent Running](https://github.com/smarkoski/sr-framework) framework due to familiarity for implementation speed.

### Example Invasive Species

* [Ash Borer](http://en.wikipedia.org/wiki/Ash_Borer)
* [Watermilfoil](http://en.wikipedia.org/wiki/Watermilfoil)
* [Japanese Knotweed](http://en.wikipedia.org/wiki/Japanese_knot_weed)

ROADMAP
-------

### Alpha Release

* We are here. Core API mostly implemented, plus an example widget that talks to the API.

### Beta Release

#### API:

* Feature Complete GET & POST endpoints
* Better common name management
* Return Wikipedia data/links
* Support for polygon merging
* Pending/Approved status for species, location, and activity data
* Require specific auth privileges to edit species, location, and activity data
* OAUTH for authentication for POST

#### Widget:

* Icons for kingdom/phylum of species
* Query species by viewport
* Support for activities

### v1.0

#### API:

Any additional features?

#### Web Admin Interface:

A web administration interface for managing the data, API authentication, etc.

#### Widget:

Developed into something easily embedded in an environmental action group or outdoor activities website w/basic config & theming options

#### Mobile Web App:

A mobile web app, built on the full feature set of the API that allows individuals (amateur or professionals) to search for invasive species information by:

  * Activity that they'd like to partake in (e.g. camping, fishing, swimming, gardening, etc.)
  * Traveling from point A to point B
  * A current location
  * A plant/animal that they've spotted

Also, the ability to post locations (both native & invading) and species from anywhere.

#### Other Partners:

Any other partners: web, desktop, and mobile app developers; professionals; scientific researchers; environmental activism organizations.
