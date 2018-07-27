# Healthcare Market hApp (hm)
The _Healthcare Market hApp (hm)_ is one of a set of _holo-health_ hApp's designed to work together to support the emergence of _**person-centric healthcare ecosystems**_ on top of a _holochain_ based infrastructure. For an overview of the project, including its goals and guiding principles, refer to the [holo-health project README](https://github.com/evomimic/holo-health/blob/master/README.md). For an architectural overview of holo-health, refer to [Holo-Health App Architecture](https://github.com/evomimic/holo-health/blob/master/docs/holo-health-app-architecture.md) .

# Overview
This hApp provides a _public marketplace_ where _Health Care Providers_ can _**offer**_ healthcare services. Each _offer_ specifies the _terms_ under which people can avail themselves of that service. Each Marketplace instance (DHT) can specify a set of principles that all providers in that marketplace are committed to following. Potential healthcare consumers can search for marketplaces whose principles align with their personal values and then search within that marketplace for services they feel may benefit them. If they find an _offer_ for a _service_ they want under _acceptable terms_, they can choose to _accept_ the _offer_. _Accepting_ an offer results in a new instance of the _Health Service Delivery hApp_ being created for which only the person who accepted the offer and the Healthcare Provider that posted the offer are added as members. Thus, although services are _offered_ publicly, the _service relationship_ between the _healthcare provider_ and the _consumer_ remains private.

# Proof of Concept (PoC) Simplistic Implementation 
The goal for the initial Proof-of-Concept (PoC) is to demonstrate the basic application architecture and hApp collaboration model. As such, a very simplistic concept of healthcare market will be implemented in the PoC. 


## Value Flow
Figure 1 illustrates the high-level value flows for the Health Market and shows how the Zome entries are partitioned across the _Health Market hApp (hm)_ and the _Health Service Delivery hApp (hsd)_.

![Health Market Value Flows](https://github.com/evomimic/holo-health/blob/master/images/holo-health-value-flow.png)

_Figure 1. Health Market Value Flows_

_HealthCare Providers_ create and publish _Offers_ to the _Health Market_ where they can be discovered by _Persons_. A Person can _accept_ an _offer_, creating a _Signed Agreement_ between them and the _Healthcare Provider_. 

The DNA for the _Health Market hApp_ will include a single _Offer_ zome as shown in Figure 2.

![Figure 2. PoC Health Market hApp (hm) DNA](https://github.com/evomimic/holo-health/blob/master/images/hm-dna.png)

_Figure 2. PoC Health Market hApp (hm) DNA_

Each observation records one fact. 
* `subjectHash` refers to the person that is the subject of the observation
* `observerHash` refers to the agent (person or organization) that made the observation
* `code` indicates the type of fact (e.g., _height_, _weight_)
* `number` is the value for this observation (in initial PoC, only numeric values are allowed for observations)
* `units` is the unit of measure associated with the value
* `date` is the observation date.
* `provenance` is a reference to the transaction that resulted in this observation being recorded. 

The _Health Observation_ zome provides the following functions:
* `recordSelfObservation` for creating new _Health Observations_ for which I am both the _subject_ and _observer_.
* `myObservations` to return a list of all of my _HealthObservations_

A `recordObservation` _bridge function_ is included that allows other hApps (e.g., the _Health Service Delivery hApp_) to record observations where I am the _subject_, but another agent (e.g., a physician, medical lab, etc.) is the _observer_.

To provide more robust searching options (when the list of _HealthObservation_ gets unwieldy), the [anchors mixin](https://github.com/holochain/mixins/tree/master/anchors) can be added to the _HealthObservation_ zome.

## Proof of Concept (PoC) Deployment Architecture Example

# Future Enhancements
