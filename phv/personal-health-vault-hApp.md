# Personal Health Vault Application (phv)

The _Personal Health Vault_ is one of a set of hApp's designed to work together to support the emergence of _**person-centric healthcare ecosystems**_ on top of a _holochain_ based infrastructure. Refer to the [holo-health project README](../README.md) for an overview of the project, including its goals and guiding principles. Refer to [Holo-Health App Architecture](../holo-health-app-architecture.md) for an architectural overview.

A _**Personal Health Vault**_ is a private container for all of my health information. This hApp is inspired by and loosely patterned after the [holo-vault](https://github.com/holochain/holo-vault) application. _Holo-vault_ is intended as a general identity manager application that provides storage of _personal profile attributes_ and allows different subsets of those profile attributes to be mapped to _personas_ that can be shared with other applications (although the exact method for sharing is not clear to me). 

_Personal Health Vault_ is built around the concept of a _**HealthCard**_. 

Briefly, a _HealthCard_...
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
* `provenance` is a reference to the transaction that resulted in this observation being recorded. 

The _Health Observation_ zome provides the following functions:
* `recordSelfObservation` for creating new _Health Observations_ for which I am both the _subject_ and _observer_.
* `myObservations` to return a list of all of my _HealthObservations_

A `recordObservation` _bridge function_ is included that allows other hApps (e.g., the _Health Service Delivery hApp_) to record observations where I am the _subject_, but another agent (e.g., a physician, medical lab, etc.) is the _observer_.

To provide more robust searching options (when the list of _HealthObservation_ gets unwieldy), the [anchors mixin](https://github.com/holochain/mixins/tree/master/anchors) can be added to the _HealthObservation_ zome.

## Proof of Concept (PoC) Deployment Architecture Example

The better I understand holochain's notion of _agent-centricity_, it seems _node-centric_ may be a more accurate label. A key principle of holochain is **_all state is local state_** and _nodes_ are the holders of local state. Each of my devices is a different _node_. According to my definition of agent, "any entity with the capacity to sense and respond to its environment", holochain _nodes_ do, indeed, qualify as _agents_. But they are distinct from _human agents_. In essence the "vault" pattern is a way of creating a _human-agent_ abstraction on top of a set of _node-agents_.

To understand a possible deployment model for the _PoC Personal Health Vault hApp_, let's start with the anatomy of a single node in a holochain app.  

![Anatomy of a Holochain Node](https://github.com/evomimic/holo-health/blob/master/images/anatomy-of-a-node.png)

_Figure 4. Anatomy of a Single PHV Node_

All _holochain nodes_ maintain two persistent data stores for each _hApp_ the _node_ has joined: (1) a **_local source chain_** and (2) a **_shard_** of the _DHT_ for that _hApp_. The _local source chain_ is a hashChain that links a series of _entries_ (NOT _blocks_!). The initial entry (_H0_, referred to as the _genesis entry_ contains a reference to (hash of) the _DNA file_ for the _hApp_. This ensures that all nodes that join this _hApp_ are running the same application code (or can be detected if they attempt to run different code). The next entry (_H1_)contains a reference to the _user_key_ for this specific node. Subsequent entries (_H2_ .. _Hn_) each reference a _data record_ that encodes a change to the application state. All of the headers are _hashchained_ to their predecessor, thus enforcing a local ordering of state changes (on the _local source chain_) while also ensuring the integrity of the entire chain.

Additionally, the _node_ runs a webserver that allows data to be retrieved from either the _local source chain_ or the _DHT_ and presented in a web browser (using the UI code defined in the _hApp's DNA folder_. 

Finally, the _node_ also runs a _DHT server_ that is responsible for helping to maintain a [World Model](https://developer.holochain.org/World_Model) by communicating with other nodes that have joined that _hApp_.

The following figure shows an example deployment architecture for an instance of the _Personal Health Vault_ (phv) hApp.
![Personal Health Vault Deployment Example](https://github.com/evomimic/holo-health/blob/master/images/phv-deployment-example.png)

_Figure 3. Personal Health Vault Deployment Example_

In this example, a new instance of the _phv_ app has been created, via

`hcdev init --clone <path-to-original-phv-app-dir> steves-phv-app`

The clone operation copies the DNA file from the original _phv_ app (so it shares the same code as the _phv_ app), but gives it a different UUID (so it will have its own DHT). 

NOTE: Currently, once cloned, any linkage to the original app is lost. So if new versions of the _phv_ app's DNA are published, the cloned app will have to be manually updated. I have filed [Issue #154 DNA as 1st Class Object?](https://github.com/holochain/holochain-rust/issues/154) on the _holochain-rust_ project to enable cloned projects to be notified when new versions of the original app have been accepted, with the option to `pull` the new version into the cloned app.

In the example, four _nodes_ have joined _steves-phv-hApp_: _Steve's Phone_, _Steve's LapTop_, _Steve's HoloPort_, and _Terri's Phone_. Each has its own _local source chain_, _DHT shard_, _WebServer_ and _DHT Server_.

**_TODO: insert discussion of membrane functions and restrictions on what nodes can join an app_**. Where should such restrictions be specified? Decisions on whether to allow another hApp to _**bridge**_ this app can be encoded in the _bridgeGenesis callback functions_ of zomes. If any zome's _bridgeGenesis_ function returns `false` the bridge is not allowed to be established. I'm assuming decisions on whether to allow a _node_ to _**join**_ an hApp could similarly be encoded in the _genesis callback function_ of zomes. Is this a sound assumption??? see **[Issue #1](https://github.com/evomimic/holo-health/issues/1)** 

# Future Enhancements
As noted above, the initial PoC implementation will include a very simplistic concept of _HealthObservation_. Going forward, the intent is to support the full [FHIR Observation Resource](https://www.hl7.org/fhir/observation.html):

![FHIR Observation Resouce UML](https://github.com/evomimic/holo-health/blob/master/images/fhir-observation-resource.png)

_Figure 4. UML Representation of the FHIR Observation Resource_

Also, the concept of a HealthCard will be implemented so that different types of FHIR resources beyond _Observations_ (e.g., [DiagnosticReports](https://www.hl7.org/fhir/diagnosticreport.html), [Medications](https://www.hl7.org/fhir/medication.html), [Immunizations](https://www.hl7.org/fhir/immunization.html), etc.)
