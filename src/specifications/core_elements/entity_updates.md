# Entity Updates

Entity updates is basically Godot asset and scene tree related netcode.

**entity_update** systems are exactly here for netcode related to Godot tree and node calls. A good way to see the difference between normal **systems** and **entity_update** systems is by seeing **systems** as systems that just involve gameplay functions, algorithms and events which end up modifying the components of specific entities. Whereas **entity_update** systems are hooked to components, being triggered when the properties of specified components have been changed so they can put together dynamic netcode messages that will be sent to clients when applicable.
For example, `pawn` movement and input would be registered and processed in a **system** whereas the Godot related animation updates that should follow would be triggered and constructed in the **entity_update** logic of the `pawn` entity. Effectively separating the actual gameplay code and algorithms from the netcode constructors.

Netcode related to Godot scene trees and nodes is dynamic, meaning that it involves a lot of string identifiers and variant values. It also involves dynamically sized data sets like HashMaps.
Each Godot client has a dynamically linked closed-source Rust library named `networkhound` which is capable of turning this Rust-based netcode into Godot variants for the client and execute logic based on that.
