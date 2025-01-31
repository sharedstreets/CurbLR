# About metadata (the manifest)

Agencies creating a CurbLR feed may wish to specify properties that apply across the entire feed. These are included in a `manifest` that appears at the beginning of the feed. The manifest is a JSON object, and below it is the GeoJSON object, which contains all of the features (geometries and associated regulations).

# Definition

The manifest JSON object may contain the following properties:

| Field name | Importance  | Type | Description | Example |
| :---: | :--- | :--- | :--- | :--- |
| createdDate | Required | `string` ([ISO 8601](https://en.wikipedia.org/wiki/ISO_8601)) | Timestamp when this CurbLR feed was created from asset data | `2019-08-15T21:17:41-05:00` |
| lastUpdatedDate | Optional | `string` ([ISO 8601](https://en.wikipedia.org/wiki/ISO_8601)) | Date when this CurbLR feed was updated from asset data. Does not apply if this is the initial CurbLR feed for an area; otherwise, this field is required. | `2019-08-20T15:33:02-05:00` |
| curblrVersion | Optional | `enum` (`string`) | Describes the version of the CurbLR data specification used to create this feed | `1.1.0` |
| timeZone | Required | `enum` (`string`) Possible values are listed in [tz database](https://www.iana.org/time-zones) | Defines the timezone for the local area covered by the CurbLR feed | `Europe/London`
| currency | Required | `string` Possible values are listed in [ISO 4217](https://www.iso.org/iso-4217-currency-codes.html) | Local currency, for interpreting payment information | `USD`
| unitHeightLength | Optional | `string` (for most countries, this will be `feet` or `metres`) | Defines the unit of measure used to describe maximum and minimum vehicle height and length restrictions in the [UserClasses](UserClasses.md). If a CurbLR feed includes any height- or length-related rules (e.g.`maxHeight` or `maxLength` is used), then this field is required. | `feet`, `metres` |
| unitWeight | Optional | `string` (well-known values: `tons`, `tonnes`, `pounds`, `kilograms`) | Defines the unit of measure used to describe maximum and minimum vehicle weight restrictions in the [UserClasses](UserClasses.md). If a CurbLR feed includes any weight-related rules (i.e.`maxWeight` or `minWeight` is used), then this field is required. | `tons` |
| priorityHierarchy | Required | `array` of `string`: a list of each unique value of `priorityCategory` (see [Rule](Rule.md)) ordered from highest to lowest priority, creating a hierarchy. | When regulations overlap in time and space, this list determines which regulation takes precedence. | priorityHierarchy = [`no standing`, `snow emergency zone`, `construction`, `game day`, `special event`, `street cleaning`, `temporary`, `standing`, `no loading`, `loading`, `no parking`, `paid parking`, `parking`]
| authority | Required | object | Defines which government authority regulates the curbspace described by the individual CurbLR feed. Many cities conducting data collection will only survey the areas that they regulate, so storing this as a universal property helps to reduce the size of the GeoJSON. If an override or exception is needed, this can be stored in the [rule](Rule.md) as a property that describes the activity being permitted or forbidden | |
| authority.name | Required | `string` | Name of agency | `City of London` |
| authority.url | Required | `string` (web link) | Link to the regulatory agency's domain | `https://vancouver.ca` |
| authority.phone | Optional | `string` (`E.164 format`: + `country code` + `local area code` + `phone number`) | The phone number,  including country and area code, for the regulatory agency that could be contacted about parking regulations | `+15551231234` |

# Sample manifest

  ```json
   "manifest": {
    "createdDate": "2019-12-30T11:40:45Z",
    "lastUpdatedDate": "2020-07-30T17:40:45Z",
    "priorityHierarchy": [
      "no standing",
      "construction",
      "temporary restriction",
      "restricted standing",
      "standing",
      "restricted loading",
      "loading",
      "no parking",
      "restricted parking",
      "paid parking",
      "free parking"
    ],
    "curblrVersion": "1.1.0",
    "timeZone": "America/Los_Angeles",
    "currency": "USD",
    "unitHeightLength": "feet",
    "unitWeight": "tons",
    "authority": {
      "name": "Portland Bureau of Transportation",
      "url": "https://www.portlandoregon.gov/transportation/",
      "phone": "+15551231234"
    }
  }
 ```

# Examples

The links below show real world curb regulations translated into CurbLR.

| Link | Description |
| :---- | :---- |
| [Examples of simple regulations](examples/simple_examples.md) | Simple regulatory scenarios typically involving one or two basic restrictions  |
| [Examples of complex regulations](examples/complex_examples.md) | Complex regulatory scenarios typically involving several restrictions  |
| Sample of [downtown Portland's parking regulations](/conversions/Portland/portland_2020-02-20.curblr.json) | Contains data for about 3 miles of parking regulations, surveyed in November 2019. This can also be viewed at [demo.curblr.org](https://demo.curblr.org)
