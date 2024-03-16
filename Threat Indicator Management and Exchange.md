
Managing threat information at any scale *requires standardisation and tooling* to allow the [[Threat Intelligence|threat information]] to be processed and used in automated ways. Indicator management can be much easier with a defined set of terms. That's where structured markup languages like
**STIX** and **OpenIOC** come in.

### Structured Threat Information eXpression (STIX)
STIX is an *XML language* originally sponsored by the U.S. Department of Homeland Security. In its current version, **STIX 2.1** defines *18 STIX domain* objects, including things like attack patterns, identities, malware, threat actors, and tools.

These objects are then related to each other by one of two STIX relationship object models: either as a relationship or a sighting. A STIX 2.1 JSON description of a threat actor might read as follows:

```JSON
{
	"type": "threat-actor",
	"created": "2019-10-20T19:17:05.000Z",
	"modified": "2019-10-21T12:22:20.000Z",
	"labels": [ "crime-syndicate"],
	"name": "Evil Maid, Inc",
	"description": "Threat actors with access to hotel rooms",
	"aliases": ["Local USB threats"],
	"goals": ["Gain physical access to devices", "Acquire data"],
	"sophistication": "intermediate",
	"resource:level": "government",
	"primary_motivation": "organizational-gain"
}
```

Fields like `sophistication` and `resource level` use defined vocabulary options to allow STIX 2.1 users to consistently use the data as part of automated and manual systems.

### Trusted Automated eXchange of Indicator Information (TAXII)
A companion to STIX is the **TAXII** protocol. TAXII is intended to allow cyber threat information to be *communicated at the application layer via HTTPS*. TAXII is specifically designed to support STIX data exchange.