# taxiinet
TAXII 2 Server Written in C#

<details>
<summary>Introduction to TAXII</summary>


### Overview
Trusted Automated Exchange of Intelligence Information (TAXII) is an application layer protocol used to exchange cyber threat intelligence (CTI) over HTTPS. TAXII enables organizations to share CTI by defining an API that aligns with common sharing models. Specifically, TAXII defines two primary services, Collections and Channels, to support a variety of commonly-used sharing models. Collections allow a producer to host a set of CTI data that can be requested by consumers. Channels allow producers to push data to many consumers; and allow consumers to receive data from many producers. Collections and Channels can be organized by grouping them into an API Root to support the needs of a particular trust group or to organize them in some other way. Note: This version of the TAXII specification reserves the keywords required for Channels but does not specify Channel services. Channels and their services will be defined in a subsequent version of this specification.

TAXII is specifically designed to support the exchange of CTI represented in STIX. As such, the examples and some features in the specification are intended to align with STIX. This does not mean TAXII cannot be used to share data in other formats; it is designed for STIX but is not limited to STIX.

### Discovery
This specification defines two discovery methods. The first is a network level discovery that uses a DNS SRV record [RFC2782]. This DNS SRV record can be used to advertise the location of a TAXII Server within a network (e.g., so that TAXII-enabled security infrastructure can automatically locate an organization's internal TAXII Server) or to the general Internet. See section 3.9 for complete information on advertising TAXII Servers in DNS.

The second discovery method is a Discovery Endpoint (this specification uses the term Endpoint to identify a URL and an HTTP method with a defined request and response) that enables authorized clients to obtain information about a TAXII Server and get a list of API Roots. See section 4.1 for complete information on the Discovery Endpoint.

### API Roots
API Roots are logical groupings of TAXII Collections, Channels, and related functionality. A TAXII server instance can support one or more API Roots. API Roots can be thought of as instances of the TAXII API available at different URLs, where each API Root is the "root" URL of that particular instance of the TAXII API. Organizing the Collections and Channels into API Roots allows for a division of content and access control by trust group or any other logical grouping. For example, a single TAXII Server could host multiple API Roots - one API Root for Collections and Channels used by Sharing Group A and another API Root for Collections and Channels used by Sharing Group B.

Each API Root contains a set of Endpoints that a TAXII Client contacts in order to interact with the TAXII Server. This interaction can take several forms:

- Server Discovery, as described above, can be used to learn about the API Roots hosted by a TAXII Server.
- Each API Root might support zero or more Collections. Interactions with Collections include discovering the type of CTI contained in that Collection, pushing new CTI to that Collection, and/or retrieving CTI from that Collection. Each piece of CTI content in a Collection is referred to as an Object.
- Each API Root might host zero or more Channels.
- Each API Root also allows TAXII Clients to check on the Status of certain types of requests to the TAXII Server. For example, if a TAXII Client submitted new CTI, a Status request can allow the Client to check on whether the new CTI was accepted.



![summarizes the relationships between the components of an API Root.](https://docs.oasis-open.org/cti/taxii/v2.1/os/taxii-v2.1-os_files/image002.png)

> Example API Root URLs
>
> ```
> https://example.com/
> https://api1.example.com/
> https://example.com/api1/
> https://example.com/api2/
> https://example.org/trustgroup1/
> https://example.org/taxii2/api1/
> ```
</details>

<details>
<summary>Data Types</summary>|

This section defines the names and permitted values of common types used throughout this specification. These types are referenced by the “Type” column in other sections. This table does not, however, define the meaning of any properties using these types. These types may be further restricted elsewhere in the document.

| Type        | Description                                        |
|-------------|----------------------------------------------------|
| `api-root`    | An API Root Resource, see section 4.2.1.           |
| `boolean`     | A boolean is a value of either true or false. Properties with this type MUST have a literal (unquoted) value of true or false. |
| `collection`  | A Collection Resource, see section 5.2.1.          |
| `collections` | A Collections Resource, see section 5.1.1.         |
| `dictionary`  | A dictionary is a JSON object that captures an arbitrary set of key/value pairs. |
| `discovery`   | A Discovery Resource, see section 4.1.1.           |
| `envelope`    | A TAXII Envelope, see section 3.7.                 |
| `error`       | An Error Message, see section 3.6.1.               |
| `identifier  | An identifier is an RFC 4122-compliant Version 4 UUID. The UUID MUST be generated according to the algorithm(s) defined in RFC 4122, section 4.4 (Version 4 UUID) [RFC4122]. |
| `integer`     | <p>The integer data type represents a whole number.</p> <p>Unless otherwise specified, all integers MUST be capable of being represented as a signed 54-bit value  ([-(2**53)+1, (2**53)-1]) as defined in [RFC7493]. Additional restrictions MAY be placed on the type where it is used.</p> |
| `list`        | <p>The list type defines a sequence of values ordered based on how they appear in the list.</p><p> The phrasing "list of type &lt;type&gt;" is used to indicate that all values within the list MUST conform to the specified type.</p><p> For instance, list of type integer means that all values of the list must be of the integer type. This specification does not specify the maximum number of allowed values in a list, however every instance of a list MUST have at least one value. Specific TAXII resource properties may define more restrictive upper and/or lower bounds for the length of the list.</p><p> Empty lists are prohibited in TAXII and MUST NOT be used as a substitute for omitting optional properties. If the property is required, the list MUST be present and MUST have at least one value.</p> |
| `manifest`    | A Manifest Resource, see section 5.3.1.            |
| `object`      | An Object Resource, see section 3.7.               |
| `status`      | A Status Resource, see section 4.3.1.              |
| `string`      | The string data type represents a finite-length string of valid characters from the Unicode coded character set [ISO10646] that are encoded in UTF-8. Unicode incorporates ASCII [RFC0020] and the characters of many other international character sets. |
| `timestamp   | <p>The timestamp type defines how timestamps are represented in TAXII and is represented in serialization as a string.</p> <p>The timestamp type MUST be a valid RFC 3339-formatted timestamp [RFC3339] using the format YYYY-MM-DDTHH:MM:SS.ssssssZ Unlike the STIX timestamp type, the TAXII timestamp MUST have microsecond precision. The timestamp MUST be represented in the UTC timezone and MUST use the “Z” designation to indicate this.</p> |
| `versions`    | A Versions Resource, see section 5.8.1.            |

</details>

<details>
	<summary>Specifications</summary>

- [TAXII - Core Concepts](https://docs.oasis-open.org/cti/taxii/v2.1/os/taxii-v2.1-os.html#_Toc31107513)
	- [Endpoints](https://docs.oasis-open.org/cti/taxii/v2.1/os/taxii-v2.1-os.html#_Toc31107514)
	- [HTTP Headers](https://docs.oasis-open.org/cti/taxii/v2.1/os/taxii-v2.1-os.html#_Toc31107515)
	- [Sorting](https://docs.oasis-open.org/cti/taxii/v2.1/os/taxii-v2.1-os.html#_Toc31107516)
	- [Filtering](https://docs.oasis-open.org/cti/taxii/v2.1/os/taxii-v2.1-os.html#_Toc31107517)
		- [Supported Fields for Match](https://docs.oasis-open.org/cti/taxii/v2.1/os/taxii-v2.1-os.html#_Toc31107518)
	- [Pagination](https://docs.oasis-open.org/cti/taxii/v2.1/os/taxii-v2.1-os.html#_Toc31107519)
	- [Errors](https://docs.oasis-open.org/cti/taxii/v2.1/os/taxii-v2.1-os.html#_Toc31107520)
		- [Error Message](https://docs.oasis-open.org/cti/taxii/v2.1/os/taxii-v2.1-os.html#_Toc31107521)
	- [Envelope Resource](https://docs.oasis-open.org/cti/taxii/v2.1/os/taxii-v2.1-os.html#_Toc31107522)
	- [Property Names](https://docs.oasis-open.org/cti/taxii/v2.1/os/taxii-v2.1-os.html#_Toc31107523)
	- [DNS SRV Names](https://docs.oasis-open.org/cti/taxii/v2.1/os/taxii-v2.1-os.html#_Toc31107524)
- [TAXII API - Server Information](https://docs.oasis-open.org/cti/taxii/v2.1/os/taxii-v2.1-os.html#_Toc31107525)
	- [Server Discovery](https://docs.oasis-open.org/cti/taxii/v2.1/os/taxii-v2.1-os.html#_Toc31107526)
		- [Discovery Resource](https://docs.oasis-open.org/cti/taxii/v2.1/os/taxii-v2.1-os.html#_Toc31107527)
	- [Get API Root Information](https://docs.oasis-open.org/cti/taxii/v2.1/os/taxii-v2.1-os.html#_Toc31107528)
		- [API Root Resource](https://docs.oasis-open.org/cti/taxii/v2.1/os/taxii-v2.1-os.html#_Toc31107529)
	- [Get Status](https://docs.oasis-open.org/cti/taxii/v2.1/os/taxii-v2.1-os.html#_Toc31107530)
		- [Status Resource](https://docs.oasis-open.org/cti/taxii/v2.1/os/taxii-v2.1-os.html#_Toc31107531)
- [TAXII API - Collections](https://docs.oasis-open.org/cti/taxii/v2.1/os/taxii-v2.1-os.html#_Toc31107532)
	- [Get Collections](https://docs.oasis-open.org/cti/taxii/v2.1/os/taxii-v2.1-os.html#_Toc31107533)
		- [Collections Resource](https://docs.oasis-open.org/cti/taxii/v2.1/os/taxii-v2.1-os.html#_Toc31107534)
	- [Get a Collection](https://docs.oasis-open.org/cti/taxii/v2.1/os/taxii-v2.1-os.html#_Toc31107535)
		- [Collection Resource](https://docs.oasis-open.org/cti/taxii/v2.1/os/taxii-v2.1-os.html#_Toc31107536)
	- [Get Object Manifests](https://docs.oasis-open.org/cti/taxii/v2.1/os/taxii-v2.1-os.html#_Toc31107537)
		- [Manifest Resource](https://docs.oasis-open.org/cti/taxii/v2.1/os/taxii-v2.1-os.html#_Toc31107538)
	- [Get Objects](https://docs.oasis-open.org/cti/taxii/v2.1/os/taxii-v2.1-os.html#_Toc31107539)
	- [Add Objects](https://docs.oasis-open.org/cti/taxii/v2.1/os/taxii-v2.1-os.html#_Toc31107540)
	- [Get an Object](https://docs.oasis-open.org/cti/taxii/v2.1/os/taxii-v2.1-os.html#_Toc31107541)
	- [Delete an Object](https://docs.oasis-open.org/cti/taxii/v2.1/os/taxii-v2.1-os.html#_Toc31107542)
	- [Get Object Versions](https://docs.oasis-open.org/cti/taxii/v2.1/os/taxii-v2.1-os.html#_Toc31107543)
		- [Versions Resource](https://docs.oasis-open.org/cti/taxii/v2.1/os/taxii-v2.1-os.html#_Toc31107544)
- [TAXII API - Channels](https://docs.oasis-open.org/cti/taxii/v2.1/os/taxii-v2.1-os.html#_Toc31107545)
</details>