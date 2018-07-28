# Healthcare Market hApp (hm)
The _Healthcare Market hApp (hm)_ is one of a set of _holo-health_ hApp's designed to work together to support the emergence of _**person-centric healthcare ecosystems**_ on top of a _holochain_ based infrastructure. For an overview of the project, including its goals and guiding principles, refer to the [holo-health project README](../README.md). For an architectural overview of holo-health, refer to [Holo-Health App Architecture](../holo-health-app-architecture.md) .

# Overview
This hApp provides a _public marketplace_ where _Health Care Providers_ can _**offer**_ healthcare services. Each _offer_ specifies the _terms_ under which people can avail themselves of that service. Each Marketplace instance (DHT) can specify a set of principles that all providers in that marketplace are committed to following. Potential healthcare consumers can search for marketplaces whose principles align with their personal values and then search within that marketplace for services they feel may benefit them. If they find an _offer_ for a _service_ they want under _acceptable terms_, they can choose to _accept_ the _offer_. _Accepting_ an offer results in a new instance of the _Health Service Delivery hApp_ being created for which only the person who accepted the offer and the Healthcare Provider that posted the offer are added as members. Thus, although services are _offered_ publicly, the _service relationship_ between the _healthcare provider_ and the _consumer_ remains private.

# Proof of Concept (PoC) Simplistic Implementation 
The goal for the initial Proof-of-Concept (PoC) is to demonstrate the basic application architecture and hApp collaboration model. As such, a very simplistic concept of healthcare market will be implemented in the PoC. 


## Value Flow
Figure 1 illustrates the high-level value flows for the Health Market and shows how the Zome entries are partitioned across the _Health Market hApp (hm)_ and the _Health Service Delivery hApp (hsd)_.

![Health Market Value Flows](https://github.com/evomimic/holo-health/blob/master/images/holo-health-value-flow.png)

_Figure 1. Health Market Value Flows_

_HealthCare Providers_ create and publish _Offers_ to the _Health Market_ where they can be discovered by _Persons_. A Person can _accept_ an _offer_, creating a _Signed Agreement_ between them and the _Healthcare Provider_. 

## DNA
The DNA for the _Health Market hApp_ will include a single _Offer_ zome as shown in Figure 2.

![Figure 2. PoC Health Market hApp (hm) DNA](https://github.com/evomimic/holo-health/blob/master/images/hm-dna.png)

_Figure 2. PoC Health Market hApp (hm) DNA_

The following fields are provided for the _Offer_ Zome Entry in the PoC:
* `type: string` identifies the type of the offer. In the POC this is a simple string. Going forward a registry of _offer types_ can be included in the Health Market.
* `by: agentHash` refers to the _healthcare provider_ that is extending the _offer_
* `description: String` provides a brief description of the _offer_
* `isActive: boolean` indicates whether this _offer_ is currently _active_. Only _active offers_ can be _accepted_.
* `startDate: String` indicates the date when the _offer_ became or will become _active_.
* `endDate: String` indicates the date when the _offer_ will expire.
* `offerName: String` provides a short human-readable name for the _offer_.
* `dataRequested: Array<Strings>`provides a list of the Healthcare Observation types (e.g., _height_, _weight_, _age_, _bloodType_, _LDL_, _HDL_, etc.) that the _healthcare provider_ needs to receive from the consumer in order to delivery\ this service.
* `valueReturned: String` indicates the type of Healthcare Observation that the _healthcare provider_ will return to the consumer based on analysis of the data gathered from the consumer. 

Note that the PoC will (initially) support only free services, i.e., it does not allow _offers_ to define a _currency flow_.

The _Offer_ zome provides the following functions:
* `createOffer` -- allows _healthcare providers_ to create and elaborate new _offers_. _Offers_ do not become available in the marketplace until they are published.
* `publishOffer`-- makes an _offer_ visible in the market and available to be accepted. This function can optionally specify a `startDate` and/or an `endDate`. If no `startDate` is provided, the current date will be used. If no `endDate` is provided, the _offer_ is assumed to remain active until explcitly withdrawn.
* `withdrawOffer` -- removes an _offer_ from the market. Note that _offers_ are not deleted and withdrawing an _offer_ has no affect on any _agreements_ that have previously been linked to that _offer_. This function can optionally specify a an `endDate`. If no `endDate` is provided, the _offer_ is withdrawn immediately (i.e., it's `isActive` attribute is set to `false`).. 
* `myOffers: [Offer]` -- allows _healthcare providers_ to retrieve the list of all of their offers. This function relies on offers having been anchored with _provider_ tags and _offerType_ tags. 
* `availableOffers: [Offer]`-- allows _consumers_ to retrieve the list of all of the offers in the marketplace, filtered by _offerType_
* `acceptOffer`-- allows a _consumer_ to accept an offer. 

A `getOffer` _bridge function_ is provided so the _Health Service Delivery hApp_ can retrieve information about the _offer_ associated with an _agreement_. 

To provide more robust searching options (when the list of _Offers_ gets unwieldy), the [anchors mixin](https://github.com/holochain/mixins/tree/master/anchors) can be added to the _Offer_ zome. Anchors can be used to segregate _offers_ by _offerType_. This can be used to search and filter `myOffers` for _healthcare providers_ and `availableOffers` for consumers.

## Proof of Concept (PoC) Deployment Architecture Example

![Figure 3. PoC Health Market hApp (hm) Deployment Example](https://github.com/evomimic/holo-health/blob/master/images/hm-deployment-example.png)

_Figure 3. PoC Health Market hApp (hm) Deployment Example_

The example shown in _Figure 3_ shows a total of seven _nodes_ belonging to three different _healthcare providers_ (Good Health, Thrive, Inc. and Get Healthy, Inc.) and two different _consumers_ (Steve and Vidya) participating in the Health Market. (NOTE: In this figure, an hApp deployed on a node is depicted as a rounded rectangle (e.g., _hm_hApp_). The internal anatomy of the hApp on a node (i.e., its _DHT server_, _DHT shard_, _local source chain_, and _web server_) is not shown, but keep in mind that all of these components are included for each deployment of a hApp to a node.)

Notice that the _hm_hApp_ is sharded across all seven nodes. Also note that the _consumer nodes_ are hosting BOTH a their own _PersonalHealthVault_ hApps AND the _hm_hApp_). 

# Future Enhancements
