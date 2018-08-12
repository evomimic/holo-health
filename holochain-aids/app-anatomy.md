# Anatomy of a Holochain App

This document provides a quick look at the components making up a Holochain Application from both development and deployment viewpoints. Refer to the [Holochain Developers Portal](https://developer.holochain.org) for a much more complete set of documentation about developing holochain applications.

## Development View

The following figure depicts the components comprising the development view of a holochain app.

![Figure 1. Development View](images/Holochain%20App%20Anatomy.png)

_Figure 1. Development View of a Holochain App_

The components are all stored within a _dna_ folder. The structural elements of the app are stored in a _dna[.json, .yaml, .toml]_ file. As the different suffixes suggect, it can be written in JSON, YAML, or TOML. This file consists of...
  1. **_application meta-data_**: name, UUID, version, description, a reference to the version of holochain on which this app depends and some configuration attributes for the DHT associated with this app.     
  2. **_zero or more Properties_**: application-specific strings available as _properties_ of the application.   
  3. **_zero or more Zomes_**: You can think of a Zome as a module within the app.
    
Each Zome consists of:
  1. **_zome metadata_**: name, description, type, configuration properties, the error handling style and the list of functions the zome exposes to other apps via bridges (discussed below). The `type` attribute specifies the type of language used to express the zome's behavior. Currently, this can be either js (for javascript), or zygo (Go's scripting language zygomys). With the Rust re-write of holochain, additional languages will be supported, basically anything that can be compiled or transpiled to _web assembly_).    
  2. **_zero or more functions_**: these implement the behavior of the zome. Each _function_ definition includes a code file and some _function metadata_ (name, callingType, and exposure). The `calling type` indicates whether the parameters passed to the function are simple strings or more complex JSON structures. The `exposure` specifies the visibility of the function in different scopes. There are three possibilities:    
    1. _only callable from within the zome itself_, signified by omitting this function entirely from the Zome Function list          
    2. _callable from other zomes within the same app_, signified by setting exposure to `zome` or     
    3. _callable from any zome in any app_, signified by setting exposure to `public`. Exposure defaults to `zome` if a function is included in the list of zome functions, but does not explicitly specify a value for its `exposure` attribute. 
  3. **_zero or more Zome Entry Types_**: these specify the data structures offered by the zome. _Entry types_ can either be simple strings, json structures, or _links_ (which is a special data structure used for creating semantic links between entries in the DHT). JSON data types require an accompanying Schema file specifying the json schema for the structure.
  
## Deployment View
The following figure shows a the components of a single HApp (hApp1) deployed to a single node (MyPhone node).

![Figure 2. Deployment View](images/Holochain%201Node%201App.png)

_Figure 2. Deployment View of a Single App Deployed to a Single Node_

All _holochain nodes_ maintain two persistent data stores for each _hApp_ the _node_ has joined: (1) a [**_local source chain_**](https://developer.holochain.org/Technical_Architecture) and (2) a **_shard_** of the _DHT_ for that _hApp_. The _local source chain_ is a hashChain that links a series of _entries_ (NOT _blocks_!). The initial entry (_H0_, referred to as the _genesis entry_ contains a reference to (hash of) the _DNA file_ for the _hApp_. This ensures that all nodes that join this _hApp_ are running the same application code (or can be detected if they attempt to run different code). The next entry (_H1_)contains a reference to the _user_key_ for this specific node. Subsequent entries (_H2_ .. _Hn_) each reference a _data record_ that encodes a change to the application state. All of the headers are _hashchained_ to their predecessor, thus enforcing a local ordering of state changes (on the _local source chain_) while also ensuring the integrity of the entire chain.

Additionally, the _node_ runs a webserver that allows data to be retrieved from either the _local source chain_ or the _DHT_ and presented in a web browser (using the UI code defined in the _hApp's DNA folder_). 

Finally, the _node_ also runs a _DHT server_ that is responsible for helping to maintain a _World Model_. (https://developer.holochain.org/World_Model) by communicating with other nodes that have joined that _hApp_.

In the following figure, we have extended the view to include a second node (_My Holoport Node_) for _hApp1_. 

![Figure 3. Deployment View](images/Holochain%202Node%201App.png)

_Figure 3. Deployment View of a Single App Deployed to a Two Nodes_

This shows the _gossip_ communication between the nodes. This channel is part of the holochain's _immune system_. As updates are propagated from _local source chains_ to the DHT sharded across multiple nodes. Each node validates the updates and, if invalid nodes are detected, the node originating the update can be flagged. Flagged nodes can then be excluded from further DHT updates, effectively banishing them from the application. The _gossip_ channel can also be used to detect which nodes are offline.

The folloiwng figure shows the configuration of MyPhone node having _joined_ a second app (_hApp2_). 

![Figure 4. Deployment View](images/Holochain%202Node%202App.png)

_Figure 4. Deployment View of a Two Apps Deployed to Two Nodes_

Notice that _hApp2_ has its own set of components: _local source chain_, _dht shard_ for hApp2, _DHT server_ and _web server_. The figure also illustrates _bridging_ between apps. In this case, _hApp2_ has created a _bridge_ to _hApp1_ (which _hApp1_ has accepted). The presence of the bridge allows _hApp2_ to invoke any of the functions designated by _hApp1's_ DNA as being _bridge functions_. Note that bridges are directional. This means that _hApp1_ cannot invoked _hApp2's_ bridge functions unless it opens (and _hApp2_ accepts) a second bridge from _hApp1_ to _hApp2_. Also note that _**all bridges are local**_ to a node. 



    
    

    
