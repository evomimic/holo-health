# Personal Health Vault Application
A _**Personal Health Vault**_ is a private container for all of my health information. It is built around the concept of a _**HealthCard**_. Briefly, a _HealthCard_...
is a private, but shareable, container of Health Information (any health request, event, condition).
* is owned by the subject of the card, but carries no Personally Identifying Information (PII). When shared it is only associated with a pseudonym.
* are stored in a Personal Health Journal that provides a complete, holistic view of my health events, conditions, and info.
* can be categorized into 1 or more categories
* is self-describing, i.e., is associated with a descriptor that specifies what types of data elements are stored in that card.
* is based on terminologies (code systems & value sets) and resources specified by the [FHIR standard](http://hl7.org/fhir/)
* constitutes a common form factor for requesting, sharing & presenting health information.
* may be shared under an Agreement that specifies the authorities and responsibilities of each Party to the agreement. The Agreement may also specify some reciprocal value flow (i.e., people may get paid for sharing HealthCards with others).
* Are protected such that only authorized users under a valid agreement have access to the HealthCard.
* Have an audit trail of all accesses to that health card.

Figure 1 provides examples of the types of HealthCards that may be defined.

![Figure 1. Examples of HealthCard Information](https://github.com/evomimic/holo-health/blob/master/images/healthcard-info-types.png)

For the initial Proof-of-Concept (PoC), a very simplistic concept of HealthCard will be used. The DNA for this Personal Health Vault app will offer a single _HealthObservation_ zome as shown in Figure 2.

![Figure 2. PoC Personal Health Vault DNA](https://github.com/evomimic/holo-health/blob/master/images/phv-dna.png)

Figure 2. Personal Health Vault DNA

Each observation records one fact. 
* `subjectHash` refers to the person that is the subject of the observation
* `observerHash` refers to the agent (person or organization) that made the observation
* `code` indicates the type of fact (e.g., _height_, _weight_)
* `number` is the value for this observation (in initial PoC, only numeric values are allowed for observations)
* `units` is the unit of measure associated with the value
* `date` is the observation date.

The _Health Observation_ zome provides only two functions:
* `create` for creating new _Health Observations_
* `myObservations` to return a list of all of my _HealthObservation_

To provide more robust searching options (when the list of _HealthObservation_ gets unwieldy), the [anchors mixin](https://github.com/holochain/mixins/tree/master/anchors) can be added to the _HealthObservation_ zome.
The following figure shows an example deployment architecture for an instance of the _Personal Health Vault_ (phv) app.
![Personal Health Vault Deployment Example](https://github.com/evomimic/holo-health/blob/master/images/phv-deployment-example.png)
