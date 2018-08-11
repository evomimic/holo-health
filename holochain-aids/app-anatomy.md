# Anatomy of a Holochain App

This document provides a quick look at the components making up a Holochain Application from both development and deployment viewpoints. Refer to the [Holochain Developers Portal](https://developer.holochain.org) for a much more complete set of documentation about developing holochain applications.

## Development View

The following figure depicts the components comprising the development view of a holochain app.

![Figure 1. Development View](../images/Holochain%20App%20Anatomy.png)

_Figure 1. Development View of a Holochain App_

The components are all stored within a _dna_ folder. The structural elements of the app are stored in a _dna.json_ file. This file consists of:
    1. **_some meta-data_** (name, UUID, version, description, a reference to the version of holochain on which this app depends and some configuration attributes for the DHT associated with this app. 
    2. **_Zero or more Properties_**
    3. **_Zero or more Zomes_**. You can think of a Zome as a module within the app.
