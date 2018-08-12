# Anatomy of a Holochain App

This document provides a quick look at the components making up a Holochain Application from both development and deployment viewpoints. Refer to the [Holochain Developers Portal](https://developer.holochain.org) for a much more complete set of documentation about developing holochain applications.

## Development ViewHolochain

The following figure depicts the components comprising the development view of a holochain app.

![Figure 1. Development View](https://github.com/evomimic/holo-health/blob/master/holochain-aids/images/Holochain%20App%20Anatomy.png)

_Figure 1. Development View of a Holochain App_

The components are all stored within a _dna_ folder. The structural elements of the app are stored in a _dna.[json,yaml,toml]_ file. As the different suffixes suggect, it can be written in JSON, YAML, or TOML. This file consists of...
  1. **_application meta-data_**: name, UUID, version, description, a reference to the version of holochain on which this app depends and some configuration attributes for the DHT associated with this app.     
  2. **_zero or more Properties_**: application-specific strings available as _properties_ of the application.   
  3. **_zero or more Zomes_**: You can think of a Zome as a module within the app.
    
Each Zome consists of:
  1. **_zome metadata_**: name, description, type, configuration properties, the error handling style and the list of functions the zome exposes to other apps via bridges (discussed below). The `type` attribute specifie the type of language used to express the zome's behavior. Currently, this can be either js (for javascript), or zygo (Go's scripting language zygomys). With the Rust re-write of holochain, additional languages will be supported, basically anything that can be compiled or transpiled to _web assembly_).    
  2. **_zero or more functions_**: these implement the behavior of the zome. Each _function_ definition includes a code file and some _function metadata_ (name, callingType, and exposure). The `calling type` indicates whether the parameters passed to the function are simple strings or more complex JSON structures. The `exposure` specifies the visibility of the function in different scopes. There are three possibilities:    
    a. only callable from within the zome itself, signified by omitting this function entirely from the Zome Function list          
    b. callable from other zomes within the same app, signified by setting exposure to `zome` or 
    c. callable from any zome in any app, signified by setting exposure to `public`. Exposure defaults to `zome` if a function is included in the list of zome functions, but does not explicitly specify a value for its `exposure` attribute. 
  3. **_zero or more Zome Entry Types_**: these specify the data structures offered by the zome. _Entry types_ can either be simple strings, json structures, or _links_ (which is a special data structure used for creating semantic links between entries in the DHT). JSON data types require an accompanying Schema file specifying the json schema for the structure.
    
    

    
