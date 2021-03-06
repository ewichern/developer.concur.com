---
title: Search 
layout: reference
---

# Search

Message to perform the initial search for hotels.

|  SOAPAction |	OTA name | Message structure | 
|----------|-----------|---------------------|
| search | HotelSearch | OTA_HotelSearchRQ |

---

## Request

**OTA_HotelSearchRQ**

|  Element |	Required | Data Type 	|  Description |
|----------|-----------|---------------------------|-|
| *MaxResponses* | Y | Int	| Concur currently supports 100 search results in one message. If more than 100 results are returned Concur drops all results after the 100th entry.|
| Criteria | Y | ComplexType	| Specified hotel search criteria. |

**Criteria**

|  Element |	Required | Data Type 	|  Description |
|----------|-----------|---------------------------|-|
| Criterion | Y | ComplexType	| Child elements that identify a single search criterion by criteria type. |

**Criterion**

The criterion is used to define the search criteria.  Currently we support only one Criterion.

|  Element |	Required | Data Type 	|  Description |
|----------|-----------|---------------------------|-|
| Position | Y | ComplexType	| Used to specify the geographic coordinates of a location, expressed in notation specified by ISO standard 6709. |
| HotelRef | N | ComplexType	| Indicates the detail of hotel reference information. |
| Radius | N | ComplexType	| Used to specify the extent of a search area. The extent is relative to an element (position, address, hotel reference, etc.) present in this ItemSearchCriterionType that specifies a location. |
| StayDateRange | Y | ComplexType	| Range of dates using ISO 8601, or fixed set of dates for Availability Request. |


**Position**

|  Element |	Required | Data Type 	|  Description |
|----------|-----------|---------------------------|-|
| *Latitude* | Y | StringLength1to16	| The measure of the angular distance on a meridian north or south of the equator. |
| *Longitude* | Y | StringLength1to16	| The measure of the angular distance on a meridian east or west of the prime meridian. |

**HotelRef**

|  Element |	Required | Data Type 	|  Description |
|----------|-----------|---------------------------|-|
| *HotelName* | N | StringLength1to128	| A text field used to communicate the proper name of the hotel. |
| *HotelCode* | N | StringLength1to16	| The code that uniquely identifies a single hotel property. The hotel code is decided between vendors. |


**Radius**

The radius element is used along with the Hotel Preference to categorise the search results. 

|  Element |	Required | Data Type 	|  Description |
|----------|-----------|---------------------------|-|
| *Distance* | Y | NumericStringLength1to16	| The distance from a reference point. |
| *DistanceMax* | N | NumericStringLength1to16 | Attribute indicating the distance from a reference point for Preferred (Corporate) hotels. |
| *UnitOfMeasureCode* | Y | NumericStringLength1to16 | The unit of measure in a code format. Refer to OpenTravel Code List Unit of Measure Code (UOM). Concur uses "1" for miles, "2" for kilometers. |

Move this to use cases and define a scenario where there is no DistaneMax specified

Given the following example: <RadiusDistance="5"DistanceMax="30"UnitOfMeasureCode="2"></Radius>Out of 100 returned hotels in response from Hotel Supplierfirst 10 hotels are Most Preferredhotels from 30 km radius. Next 10 hotels are Preferredhotels from 30kmradius.Other 80 hotels are hotels with no preference within the 5km radius. Note: The preference level is defined by the HotelPreference element in the TPA_Extensions, which is outlined below


**StayDateRange**

|  Element |	Required | Data Type 	|  Description |
|----------|-----------|---------------------------|-|
| *Start* | Y | DateOrTimeOrDateTimeType	| The starting value of the time span. |
| *End* | Y | DateOrTimeOrDateTimeType	| The ending value of the time span. |


---


## Response

**OTA_HotelSearchRS**

|  Element |	Required | Data Type 	|  Description |
|----------|-----------|---------------------------|-|
| Properties | Y | ComplexType	| Detailed property level information. |

**Properties**

|  Element |	Required | Data Type 	|  Description |
|----------|-----------|---------------------------|-|
| Property | Y | ComplexType	| A property that matches some or all of the search criteria. |


**Property**

|  Element |	Required | Data Type 	|  Description |
|----------|-----------|---------------------------|-|
| ChainCode | N | StringLength1to32	| 2 letter GDS chain code. The code that identifies a hotel chain or management group. Used for Chain filter in UI, and for Travel Rules based on GDS codes |
| ChainName | N | StringLength1to32	| Detailed property level information. |
| HotelCode | Y | StringLength1to32	| The code that uniquely identifies a single hotel property. Used in other OTA messages. |
| HotelName | Y | StringLength1to32	| 	A text field used to communicate the proper name of the hotel. |
| Position | Y | ComplexType	| Refer to Position in the Request |
| Address | Y | ComplexType	| something |
| ContactNumbers | N | ComplexType	| something Do we even care about this?  HRS does not seem to be returning this |
| Award | N | ComplexType	| An element that identifies the hotel ratings. |
| HotelAmenity | N | ComplexType | List of Hotel Amenities. |
| Policy | N | ComplexType	| **Not used now - need to confirm** |
| Amenities | N | ComplexType	| **Not used now - need to confirm** |
| TPA_Extensions | N | ComplexType | See TPA Extensions below |


**Address**

|  Element |	Required | Data Type 	|  Description |
|----------|-----------|---------------------------|-|
| AddressLine | Y | ComplexType	| something |
| CityName | Y | ComplexType	| something |
| PostalCode | Y | ComplexType	| something |
| StateProv | Y | ComplexType	| something |
| CountryName | Y | ComplexType	| Country name (e.g., Ireland) |


**CountryName**

|  Element |	Required | Data Type 	|  Description |
|----------|-----------|---------------------------|-|
| Code | Y | StringLength0to64 | The name or ISO 3166 code of a country (e.g. as used in an address or to specify citizenship of a traveller). |


**ContactNumbers**

|  Element |	Required | Data Type 	|  Description |
|----------|-----------|---------------------------|-|
| ContactNumber | Y | ComplexType | something |


**ContactNumber**

|  Element |	Required | Data Type 	|  Description |
|----------|-----------|---------------------------|-|
| CountryAccessCode | Y | StringLength1to32 | something |
| PhoneNumber | Y | StringLength1to32 | something |
| PhoneTechType | Y | StringLength1to32 | What types do we support? |


**Award**

|  Element |	Required | Data Type 	|  Description |
|----------|-----------|---------------------------|-|
| *Rating* | Y | Int | Hotel rating should be integer number from 0 to 5, represention it's star rating. |


**HotelAmenity**

|  Element |	Required | Data Type 	|  Description |
|----------|-----------|---------------------------|-|
| *Code* | Y | OTA_CodeType	| Refer to OpenTravel Code List Hotel Amenity Code (HAC) |


**Policy**

|  Element |	Required | Data Type 	|  Description |
|----------|-----------|---------------------------|-|
| *CheckInTime* | Y | StringLength1to32	| something |
| *CheckOutTime* | Y | StringLength1to32	| something |



### TPA Extensions

**TPA_Extensions**

|  Element |	Required | Data Type 	|  Description |
|----------|-----------|---------------------------|-|
| HotelPreference | Y | StringLength1to32	| A reference to identify the booking |
| TPA_HotelPreviewImageURI | Y | ComplexType | A reference to identify the booking |


**TPA_HotelPreviewImageURI**

|  Element |	Required | Data Type 	|  Description |
|----------|-----------|---------------------------|-|
| URL | Y | StringLength1to32	| Concur supports on one image URL in the Search Response.  For the ability t0 display more images refer to Decriptive Info.  The image will be used as a thumbnail and should be limited to 70x70 pixels to prevent image artifacts by scaling. |



