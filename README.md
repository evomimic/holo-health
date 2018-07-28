# holo-health
_**Person-Centric Healthcare Ecosystems**_ on top of a _holochain_ based infrastructure.

Code Status: **Pre-alpha Design**. Not for production use. This application has not been audited for any security validation.

## Goals
   1. Create _**empowered health marketplaces**_ that enable _HealthCare Providers_ to offer health services (both online and in-person) that can be easily discovered and privately subscribed to by individuals.
   1. Put _**individuals in charge**_ of their own **_healthcare data_** and their _**healthcare decisions**_.
   1. Enable _**better health outcomes**_ by unifying all of an individual's health observations into a single _**Personal Health Record**_ (PHR). Observations added by _**any healthcare provider**_ in my ecosystem are added to my _PHR_.
   1. _Design in_ direct support for compliance with relevant healthcare (e.g., HIPAA/HITECH) and personal information regulations (e.g., GDPR). 
   1. Support traditional _**currency-for-service**_ flows while also allowing an open-ended set of _agreement types_ to be used to support _**alternative reciprocal value flows**_ (e.g., _alternative currencies_, _barter_, or _gift economy_). 


## Guiding Principles
_**P1 Primacy-of-the-Individual**_ -- The preponderance of applications on the internet (Facebook, Amazon, Netflix, Google, Uber, Twitter, etc.) are designed around the primacy of the application over the individual. They serve the companies that have created them. Each application offers individual user accounts and maintains its own information about that individual. Increasingly, they are gathering information about the people who use those applications with an eye towards then pushing recommendations, products, or services at them. If an individual interacts with multiple such applications information about them is fragmented into each of these databases. Individuals have limited control over what information is gathered, how it is used and/or who it is shared with. The burden of keeping such information consistent, accurate and up-to-date falls to the individual (i.e., by updating each profile individually). MyOneId reverses this picture by first and foremost serving the individual. 
_**P2 Personal-Information-Stewardship**_ -- individuals are the stewards of their own information and it is they who make the decisions about whether, how and with whom their information is shared.
_**P3 Transparency**_ -- The more accurate, comprehensive and up-to-date the information that a _healthcare provider_ has about the totality of an individual's medical history, conditions and life events, the better they are able to diagnose accurately and coordinate care plans across a team of _healthcare providers_ collaborating with the individual. There is a natural tension between these last two principles. Per _P1 Primacy of the Individual_, each individual is free and empowered by _holo-health_ to find their own balance point between privacy and transparency and they can adjust this balance point over time. 

## Architecture
Refer to [Holo-Health Application Architecture](holo-health-app-architecture.md) for an overview of the various apps comprising the _Holo-Health_ architecture.
