# Healthcare Market hApp

## Proof of Concept (PoC) Simplistic Implementation 
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
* `provenance` is a reference to the transaction that resulted in this observation being recorded. 

The _Health Observation_ zome provides the following functions:
* `recordSelfObservation` for creating new _Health Observations_ for which I am both the _subject_ and _observer_.
* `myObservations` to return a list of all of my _HealthObservations_

A `recordObservation` _bridge function_ is included that allows other hApps (e.g., the _Health Service Delivery hApp_) to record observations where I am the _subject_, but another agent (e.g., a physician, medical lab, etc.) is the _observer_.

To provide more robust searching options (when the list of _HealthObservation_ gets unwieldy), the [anchors mixin](https://github.com/holochain/mixins/tree/master/anchors) can be added to the _HealthObservation_ zome.

## Proof of Concept (PoC) Deployment Architecture Example

# Future Enhancements
