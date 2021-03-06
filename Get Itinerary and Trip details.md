## API Recipes: API Tasks
This series of API Recipes describes API tasks associated with developing apps,  apps for Travel Management Companies, (TMC), Travel Suppliers and businesses who need to get data about their employees' travel related booking.  This recipe assumes you are a current Concur customer.
### Before you begin
Review the following checklist to ensure you are able to perform the task in this recipe. To see reference information, click the link in the bullet point
Understand the [Oath 2.0 process](https://developer.concur.com/api-reference/authentication/authentication.html)
- Be able to access the [Get Trips API](https://developer.concur.com/api-reference/travel/itinerary/trip/trip-resource.html#postnt)
- Be able to access the [Itinerary Web Service (TMC & Third Party)](https://developer.concur.com/api-reference/travel/itinerary-tmc-thirdparty/index.html)
- Ensure you have a definition [XSD](https://developer.concur.com/api-reference/travel/itinerary/ItinServices_Public_0.xsd), usable for architecting your solution

### Get Itinerary and Trip Details: Pulling Itinerary Trip data from Concur

Developers, travel suppliers and travel management companies will want to use the processes described in this recipe.

Developers, travel suppliers and travel management companies will want to use the processes described in this recipe.

- With an authorized Oauth token, Developers can retrieve detailed itineraries for users or user groups. Using this data allows developers to create applications that match opportunities for users in a given locale, region or country specific to user interests.  For example, TV Food Maps matches destinations on a user’s itinerary to notify them of a featured restaurant from a TV show like Diners, DriveIns and Dives near the location of their itinerary
- Travel Suppliers can use the processes to analyze itineraries/trips for gaps in such services as car rental and hotel accommodations, and use the information to offer these services. This is particularly useful if you want to provide an enhanced travel experience for your users
- Travel Management Companies can use these processes to analyze individual or company travel data, offer traveler support services, Triplink support or business intelligence and analytics

The order in which most Concur partners obtain travel data from Concur is:

1. Manually add a new itinerary. This creates a sample trip that allows the developer to retrieve their first itinerary via the API and with the response and the XSD, build a schema or data structure.
2. Populate internal systems by building a scheduled process to poll for itineraries based on specific function parameters.
3. Use the list of itineraries to obtain greater trip details. Most travel suppliers and TMC's will retrieve trip details based on day, time period or users.

Each of these processes can be used by itself. For example if a travel supplier only wanted to obtain itinerary details, they can follow the Get Itinerary Details process described below.


Each of these processes can be used by itself. For example if a travel supplier only wanted to obtain itinerary details, they can follow the Get Itinerary Details process described below.

#### Get List of Itineraries:
The GET List of Itineraries required for the Get Itinerary endpoint is located in the official Concur developer page [here](https://developer.concur.com/api-reference/travel/itinerary-tmc-thirdparty/index.html#getlist).

This task retrieves trip summaries for the traveler specified in the Oauth token. This endpoint can also be used to get details for trips for a different user, organization or for the entire company. Get List of Itineraries is used most often when a Travel management Company (TMC) needs to retrieve a list of trips on behalf of a user or company.

#### Examples:
  Entire Company (Oauth consumer must have Admin user role)

  https://www.concursolutions.com/api/travel/trip/v1.1/?startDate={startdate}&endDate={enddate}&_createdAfterDate={_date}&createdBeforeDate={date}&lastModifiedDate={date}&bookingType={type}&userid_type=login&userid_value=ALL

Individual user (Oauth consumer)

https://www.concursolutions.com/api/travel/trip/v1.1/?startDate={startdate}&endDate={enddate}&_createdAfterDate={_date}&createdBeforeDate={date}&lastModifiedDate={date}&bookingType={type}

Individual user (other than the Oauth consumer)

https://www.concursolutions.com/api/travel/trip/v1.1/?startDate={startdate}&endDate={enddate}&_createdAfterDate={_date}&createdBeforeDate={date}&lastModifiedDate={date}&bookingType={type}&userid_type=login_id&userid_value={loginID}

Additional examples can be viewed [here](https://developer.concur.com/api-reference/travel/itinerary-tmc-thirdparty/index.html#getlist).

##### Function Parameters
Name | Description
--------------- | ---------------
tripId	| The trip ID
startDate={_date_} |	The URL-encoded UTC start date for the trip. Format: YYYY-MM-DD. If no query parameters are provided, the start date is set to today's date - 30 days.
endDate****={_date_} |	The URL-encoded UTC end date for the trip. Format: YYYY-MM-DD. If no query parameters are provided, the end date is set to today's date + 12 months.
createdAfterDate****={_date_} |	The URL-encoded UTC date for when the trip was created. The query string will return trips created on or after this date. Used with the createdafterdate for finding trips created during a date range. Format: YYYY-MM-DD
createdBeforeDate****={_date_} | The last modified UTC date of the trips and any their associated bookings. This query string will return only the trips where the trip or any of its associated bookings have a last modified date that is greater or equal to the supplied time
lastModifiedDate****={_date_} |	The trip includes at least one booking of this type. Format: Air, Car, Dining, Hotel, Parking, Rail, or Ride
bookingType={_type_} |	The trip includes at least one booking of this type. Format: Air, Car, Dining, Hotel, Parking, Rail, or Ride
userid_type=login&userid_value=_{loginID}_ |	The loginID is the user's Concur login ID. The userid_value of ALL can be sent to get trip summaries for all users at the company. The userid_type and userid_value parameters can only be used if the OAuth consumer has one of the user roles listed above.
includeMetadata=true&ItemsPerPage={_number_}&Page={_number_} |	The includeMetadata query parameter combined with the ItemsPerPage and Page query parameters will cause the response to be divided into pages.
includeVirtualTrip=_1_ |	Virtual trips are segments booked offline through the Travel Request product. Set the includeVirtualTrip query parameter to 1 to include those trips in the list
includeCanceledTrips=_{true/false}_ |	The includeCanceledTrips query parameter will cause the request to also return trips with a status of Canceled.

Refer to the sample [XML GET Itinerary Details Get request](https://developer.concur.com/api-reference/travel/itinerary-tmc-thirdparty/index.html#getdetails) for guidance.

### Get Itinerary details
This process allows developers, travel suppliers and travel management companies to obtain details on itineraries for specific trips. By default, the Oauth consumer should be the owner of the trip. The Get Itinerary endpoint can also be used to get details for trips that the Oauth consumer does not own. Travel management companies use this process to get trip details on behalf of a user. This assumes the TMC is registered with Concur and possesses a valid Concur account with one the following user roles: Web Services Administrator for Professional, or Can Administer for Standard.

##### Function Parameters
Name | 	Description
--------------- | ---------------
{_tripId_} | Required. The identifier for the desired trip
userid_type=login&userid_value=_{loginID}_ |	The loginID is the user's Concur login ID. The userid_value of ALL can be sent to get trip summaries for all users at the company.
systemFormat=_{format}_	|The systemFormat query parameter can be used to specify that the response is formatted for a different system. The supported value is Tripit.

The request returns an itinerary parent element with a subset of child elements. A partial list appears below. The complete list is located [here]( https://developer.concur.com/api-reference/travel/itinerary-tmc-thirdparty/index.html#getdetails).

##### Parent element parameters
Name |	Description
-------------- | ---------------
BookedByFirstName	| The first name of the person who booked the trip.
BookedByLastName | The last name of the person who booked the trip.
BookedVia	| The booking method for the trip.
CancelComments |	The comments provided if the itinerary is cancelled. Maximum length: 256 characters.
Comments | Optional comments. Maximum length: 512 characters.
DateBookedLocal | The date the trip was booked, in the local time of the booking location. Format: YYYY-MM-DDThh:mm:ss
DateCreatedUtc | The date that this trip was created, in UTC. Format: YYYY-MM-DDThh:mm:ss

##### Booking Child elements
Name |Description
--------------- | ---------------
BookingSource	| The name of the booking source for this booking. A booking source is a textual name the system uses to track where a booking took place.
DateBookedLocal	| The name of the booking source for this booking. A booking source is a textual name the system uses to track where a booking took place.
DateCreatedUtc	| The date the booking was created, in UTC. Format: YYYY-MM-DDThh:mm:ss
DateModifiedUtc	| The date the booking was last modified, in UTC. Format: YYYY-MM-DDThh:mm:ss
FormOfPaymentName	| The name of the form of payment for the booking.
FormOfPaymentType	| The type of the form of payment.
PassengerCount | The number of passengers included in the booking.


### Get Trip details
Developers should build a scheduled process to poll for itineraries or get trips on demand when a user logs onto your website to make a booking. Using the Oauth token of the user for whom you want details as the authorization, perform this two-step process:
1. GET a list of trips for the user.
2. GET the details of each trip one at a time.

For more information on how to GET a trip list for a user by using their Oauth token, go [here](https://developer.concur.com/itinerary-tmc-and-third-party-developers/itinerary-resource/itinerary-resource-get#listofitins).

#### Example Get List of Trips request:
```
GET https://{InstanceURL}/api/travel/trip/v1.1 HTTP/1.1
Authorization: OAuth {user's access token}
Accept: application/xml
```
You can also get the details of each trip by using the TripID.
The Get Trip List response includes the unique TripID for each trip.

#### Example trip details request:
```
GET https://{InstanceURL}/api/travel/trip/v1.1/{TripID} HTTP/1.1
Authorization: OAuth {user's access token}
Accept: application/xml
```
Refer to the sample [Get trip details for a trip ID]( https://developer.concur.com/api-reference/travel/itinerary/trip/trip-resource.html#gettd) for guidance.

##### Important
No itinerary can contain all of the possible elements, because the response will always be a subset of the possible returned values. For full response details, view the [Public Itinerary XSD](https://developer.concur.com/sites/default/files/ItinServices_Public_0.xsd )

Once you have the trip details, look for trips that lack your segment type.

-  For example, if you are a TripLink Car Supplier, look for trips that may include a flight but which lack a rental car reservation.

- If you are a TripLink Hotel Supplier, look for trips that may include a flight but which lack a hotel reservation.  If you are polling for trips at an interval, you may choose to email a traveler to offer them your service to add to their upcoming trip.  For example, if you are a TripLink Hotel partner, you may choose to email the traveler: “If you would like to reserve a hotel for your upcoming trip to Boston on 13 December, we are offering a special rate at our property in city center.  Log on here to add a hotel reservation to your trip.”

- If you are checking for upcoming trips when the traveler logs in to your mobile app or website to make a booking, you may choose to present to the traveler a list of their upcoming trips that lack your segment type.  For example, if you are a TripLink Car Supplier, you may choose to present a list of upcoming trips that lack a rental car reservation and allow the traveler to easily add a car rental to that trip, prepopulating the pickup and drop-off locations and dates/times using the information from the trip details.

#### Make us better at making your experience easier.

Share a Concur API process issue we can do better. Provide us with an explanation, screen shots and your recommendation [here](http://forum.developer.concur.com/).
