# Basics & Introduction 🌌

Before we get started, the documentation assumes that you can do the following things:
1. Write simple applications with the Rust programming language.
2. Work with the ECS system from Bevy ECS, no rendering know-abouts required.
3. Work with the scene tree and various types of nodes of the Godot engine.

The aforementioned technologies and engines offer community hubs on Discord and lots of user guides on their official websites. A lot of information is available on the internet, accessible by entering the right queries on search engines such as duckduckgo.com or google.com .

Remember, everyone should be able to learn basics of the technology used here if given enough time. You just have to be patient and find the right educational resources, we can help you along the way on [our Discord](discord.gg/yypmun9ctt) too.

Always keep in mind that everything is integrated with Bevy ECS, always keep in mind that we are strictly working with an Entity Component System architecture when reading through these documentations.

**Please take some time to learn the basics of Bevy ECS, strictly the ECS part. Learn about Bevy events too.**

It is highly recommended to get an editor that specifically has integrated support for Rust. Visual Studio Code in combination with the rust-analyzer extension is a great option!

The server also uses the [Rapier Bevy plugin](https://rapier.rs/docs/user_guides/bevy_plugin/rigid_bodies) to simulate physics with. It may be useful to understand how it works too, specifically when trying to create new entities. 

You can clone or fork the open-source server application via [the official repository](https://github.com/starwolves/space).

## Application layout

The server is written in pure Rust and uses json files exported by the unreleased Space Frontiers GUI map & content editor.

These json files are to be found in `data/maps/`.

Game mods that can have their own additional sets of content, entities, systems and components would be dragged and dropped into `src/plugins/`.

The main server loop builder can be found in `src/space/mod.rs`.

The logic is split up in two categories `core` and `entities`. `entities` contains all exclusive logic related to the officially existing entities that are available in the game. Many of which can be spawned from the in-game console. In here you will find spawn functions, components and systems specific to individual entities; logic that isn't shared with other entities.

In terms of code related to specific entities it is also important to know about `src/space/core/entity/mod.rs` as that is the initializer for all entities belonging to the official core of the server.

`core` covers logic that spans across multiple entity types or that covers exclusive gameplay features not related to any specific entity. Here you will find the core gameplay elements, also the main world and gridmap initialization based on the json files found in the map data folder we have touched on. To understand how the gridmap and tiles get initialized please take a look at `src/space/core/gridmap/`. The `functions` folder contains the logic that reads map data at run-time from the map data json files, whereas inside `src/space/core/gridmap/mod.rs` each individual tile of the gridmap gets initialized much like how we did with entities in `src/space/entities/mod.rs`!

As you can see both logic related to `core` and `entities` are methodically organized and split up into the following named files and/or folders:
1. **functions** where all helpful code functions are found.
2. **systems** contains most logic. Systems usually are the ones to call **functions**. These are ECS systems directly added to the server loop we have touched on earlier.
3. **components** has all components that are usually added to entities upon spawning them or after gameplay triggers occur.
4. **entity_update** similar to **systems**, because this also contains Bevy ECS systems that are inserted into the game loop, but these ones are seperated as special entity_update systems which contain logic that has to do with constructing Godot specific net code to be sent to clients which have the entity in their field of view. These systems contain logic to alter specific Godot nodes of specific entities in the view of clients. Think animation player updates, position updates of nodes, texture swaps and `SpatialMaterial` property adjustments.
5. **events** will have the Bevy ECS events layed out, to communicate between systems and to prepare and send net code messages to the client.
6. **mod** where startup systems are located, systems that run once at the very beginning when the server starts to initialize things.
7. **resources** contains the Bevy ECS resources used, also initialized in the server loop we have touched on before.
8. **spawn** only exists in `entities` and here you find the main spawn function for each individual entity, the one that gets called when this entity is supposed to spawn in either on server launch from `entities.json` or through spawn commands or other in-game events.

## Godot assets, content folders and netcode

Let's explain how it is possible for Bevy ECS to send dynamic netcode to the client, allowing each server to have its own collection of custom content additions which involves custom Godot scenes, custom textures, custom materials, custom meshes, custom animations, custom sounds and more. All the while the client automatically loads these in and automatically accepts new incoming netcode calls for those custom entities with requiring the client to be modified, only the server has to get modified for this!

First and foremost, make sure you have obtained a prototype client from [the Discord](discord.gg/yypmun9ctt). Because with this client a folder named "content" is shipped, which includes either Godot engine resources or raw files such as png's which the client is able to load in at run-time.

As you can see, the content files and folders are strictly named. The naming is a critical aspect to being able to properly load in entities for the client and then further make netcode calls on those newly added entities through dynamic netcode, netcode which takes uses strings to identify different entity types. In fact the identifying strings for entities are the names of the folders you see located in `content/entities/`. Go ahead and take a look at `src/space/entities/mod.rs` and the **spawn** functions of entities and find that we use the same entity names on the server as is reflected in the content folder of the client.

Right now the content folder is already supplied with the client upon download, but in the future these content folders will be downloaded from either the server itself or an HTML download server, much like how previous classic games like Garry's Mod have done it.

The way these Godot scene files are created, stripped and named involves methodical naming and stripping which will be documented later on. It also involves usage of the yet to be released Space Frontiers GUI map & content editor.

**entity_update** systems are exactly here for netcode related to Godot tree and node calls. A good way to see the difference between normal **systems** and **entity_update** systems is by seeing **systems** as systems that just involve gameplay functions, algorithms and events which end up modifying the components of specific entities. Whereas **entity_update** systems are hooked to components, being triggered when the properties of specified components have been changed so they can put together dynamic netcode messages that will be sent to clients when applicable.
For example, `pawn` movement and input would be registered and processed in a **system** whereas the Godot related animation updates that should follow would be triggered and constructed in the **entity_update** logic of the `pawn` entity. Effectively separating the actual gameplay code and algorithms from the netcode constructors.

Netcode related to Godot scene trees and nodes is dynamic, meaning that it involves a lot of string identifiers and variant values. It also involves dynamically sized data sets like HashMaps.
Each Godot client has a dynamically linked closed-source Rust library named `networkhound` which is capable of turning this Rust-based netcode into Godot variants for the client and execute logic based on that.

You can find all the netcode messages at `src/space/core/networking/resources.rs`.
