# Personal Health Vault Application
A _**Personal Health Vault**_ is a private container for all of my health information. It is built around the concept of a _**HealthCard**_. Briefly, a _HealthCard_...
is a private, but shareable, container of Health Information (any health request, event, condition).
* is owned by the subject of the card, but carries no _Personally Identifying Information_ (_PII_). When shared it is only associated with a pseudonym.
* are stored in a _Personal Health Journal_ that provides a complete, holistic view of my health events, conditions, and info.
* can be categorized into 1 or more _categories_
* is _self-describing_, i.e., is associated with a descriptor that specifies what types of data elements are stored in that card.
* is based on _terminologies_ (code systems & value sets) and resources specified by the [FHIR standard](http://hl7.org/fhir/)
* constitutes a _common form factor_ for requesting, sharing & presenting health information.
* may be shared under an _Agreement_ that specifies the authorities and responsibilities of each Party to the agreement. The Agreement may also specify some reciprocal value flow (i.e., people may get paid for sharing HealthCards with others).
* Are protected such that _only authorized users under a valid agreement_ have access to the HealthCard.
* Have an _audit trail_ of all accesses to that health card.

Figure 1 provides examples of the types of HealthCards that may be defined.

![Figure 1. Examples of HealthCard Information](https://github.com/evomimic/holo-health/blob/master/images/healthcard-info-types.png)
_Figure 1. Examples of HealthCard Information_

## Proof of Concept (PoC) Simplistic HealthCard Implementation 
For the initial Proof-of-Concept (PoC), a very simplistic concept of HealthCard will be used. The DNA for this Personal Health Vault app will offer a single _HealthObservation_ zome as shown in Figure 2.

![Figure 2. PoC Personal Health Vault DNA](https://github.com/evomimic/holo-health/blob/master/images/phv-dna.png)

_Figure 2. Personal Health Vault DNA_

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

## Proof of Concept (PoC) Deployment Architecture Example
The following figure shows an example deployment architecture for an instance of the _Personal Health Vault_ (phv) app.
![Personal Health Vault Deployment Example](https://github.com/evomimic/holo-health/blob/master/images/phv-deployment-example.png)

_Figure 3. Personal Health Vault Deployment Example_

In this example, new instance of the _phv_ app has been created, via

`hcdev init --clone <path-to-original-phv-app-dir> steves-phv-app`

The clone operation copies the DNA file from the original _phv_ app (so it shares the same code as the _phv_ app), but gives it a different UUID (so it will have its own DHT). 

NOTE: Currently, once cloned, any linkage to the original app is lost. So if new versions of the _phv_ app's DNA are accepted, the cloned app will have to be manually updated. I have filed [Issue #154 DNA as 1st Class Object?](https://github.com/holochain/holochain-rust/issues/154) on the holochain-rust project to enable cloned projects to be notified when new versions of the original app have been accepted, with the option to `pull` the new version into the cloned app.

In the example, four _nodes_ have joined steves-phv-app.

**_TODO: insert discussion of membrane functions and restrictions on what nodes can join an app_**

_Figure 4. Anatomy of a Single PHV Node_

