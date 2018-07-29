# Health Service Delivery hApp
The _Health Service Delivery hApp_ is one of a set of hApp's designed to work together to support the emergence of _**person-centric healthcare ecosystems**_ on top of a _holochain_ based infrastructure. Refer to the [holo-health project README](../README.md) for an overview of the project including its goals and guiding principles. Refer to [Holo-Health App Architecture](../holo-health-app-architecture.md) for an architectural overview.

# Overview
An instance of this _hsd_ hApp is created when a person _accepts_ a _Healthcare Service offer_ placed in the _Healthcare Market_ by a _Healthcare Provider_. The _hsd_ serves as a private channel between the two parties. Both the _agreement_ (which includes a reference to the _offer_ on which the _agreement_ was based) and the _requests_ and _responses_ made by either party, are recorded in the DHT for this hApp instance. This provides a secure, non-forgeable, immutable, non-repudiable audit trail that is necessary for both invoicing and regulatory compliance. 

The _Healthcare Service Delivery hApp_ is intended to support a very broad range of services (including both electronic and in-person) offered under a broad range of _agreement types_. 

# Proof of Concept (PoC) Simplistic Implementation 
The goal for the initial Proof-of-Concept (PoC) is to demonstrate the basic application architecture and hApp collaboration model. As such, a very simplistic concept of _healthcare service delivery_ will be implemented in the PoC.

ges to the _Personal Health Vault (phv)_ hApp to retrieve requested personal health information from the person's vault and also to record observations made by the _Healthcare Provider_ back into the persons's vault.
* No support for _agreement types_, the PoC will only a support a single, implicitly defined agreement with the following _service flow_:
   1. _Healthcare provider_ issues _HealthInfoRequest_ for specified _observation codes_ from _consumer_
   1. The _hsd hApp_ will automatically check all such requests to ensure they comply with the terms specified in the _agreement_. If not, the request is  rejected.
   1. If the _request_ conforms to the _agreement_, the _hsd_ will build a _HealthInfoResponse_ to return to the _healthcare provider_. For each requested _observation code_ for which the consumer's _PersonalHealthRecord_ includes an _observation_, the _observation_ will be included in the response. 
   1. Upon receiving the _HealthInfoResponse_, the _hsd_ will invoke _handleResponse_ to trigger the _healthcare provider's_ analysis logic to analyze the personal health observations returned by the _consumer_ to arrive at a _health recommendation_. The    response is NOT included in the list of _observation codes_ defined in the _offer_, the HealthInfoRequest is rejected.
   1. All HealthInfoRequests, HealthInfoResponses, and Observations recorded are persisted to the DHT.

## DNA
![Figure 1. Health Service Delivery DNA](../images/hsd-dna.png)

## Proof of Concept (PoC) Deployment Architecture Example
![Figure 2. Example hsd hApp Deployment](../images/hsd-deployment-example.png)

# Future Enhancements
1. Handle an extended range of _agreement types_ that enable more complex _service flows_.
   * Pay-per-service transactions using a _**pluggable currency**_
   * More complex _**service choreography**_.
1. Enhance privacy and security by supporting:
   * pairwise pseudonymous identities
   * encrypted payloads (using receiving agent's public key, so that only that agent can decrypt the payload).


