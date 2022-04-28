# Adding a new entity

The best way to understand how new entities are created is by looking at the currently existing entities under `src/entities/`. Also by checking out the content related to entities inside the client content folder located under `content/entities/`.

Creating and adding a new entity essentially boils down to the following:
1. Come with a unique *lowerCamelCase* name for your entity.
2. If applicable, strip the Godot scene that represents the entity and copy the stripped scene and stripped resources into a their own folder named after the entity name. More information on how to exactly do this will be added to the docs upon request.
3. Create a new *snake_case* named folder in `src/entities/`, named after your entity. In this folder create a `spawn.rs` file. Create a spawn function that has the exact same input and output as the other entity spawn functions. There are several `core components` combinations that should be added to the entity upon spawning it. You can read more about these important components in this documentation. To see which components must be included for the entity, take a look at other spawning functions of existing entities and read through the `core components` section further down this documentation.
4. Register the new entity you have created as a plugin inside its `mod.rs`, reference the spawn function you have just created there. Also ensure that the entity name is *lowerCamelCase* and matches the name of the entity content folder so the client knows which resources belong to this specific entity.
5. Test out your newly added entity in-game with the `spawn_entity` command, use the name *lowerCamelCase* of the entity to spawn it in and to test it.

There are several `core components` which should be used for almost all entities, such as `Senser`, `RigidBodyBundle`, `ColliderBundle`, `EntityData`, `EntityUpdates`, `Examinable` and `Health`. 

The selection of core components your entity will require in its `spawn.rs` will mainly depend on which one of these three common types describes your entity the most:
1. Static entities that don't move.
2. Dynamic entities that do move and are physics based.
3. Dynamic entities that can be picked up by other entities with inventories.

In case of the first entity type ensure your entity has component `StaticTransform` and the Rapier physics `RigidBodyBundle` contains `RigidBodyType::Static`. For an example of this entity type and its components check out `src/entities/air_locks/spawn.rs`.

In case of the second entity type you will require component `CachedBroadcastTransform` and the Rapier physics `RigidBodyBundle` contains `RigidBodyType::Dynamic`. Also component `WorldMode` must be added and contain `WorldModes::Physics`. For an example of this entity type and its components please check out `src/entities/air_locks/spawn.rs`.

The last described entity type will require the same components as the second entity type. On-top of that it also requires an `InventoryItem` component which holds several data points, including 3D attachment positions and rotations on humanoid slots. `src/entities/air_locks/spawn.rs` should be checked out for more information.

Don't forget to add the new systems of this new entity to the main server loop. Also use the correct labels and execution orders for special systems like but not limited to `entity_update` systems! You can learn more about this by inspecting the `space core` Bevy loop builder.

### Content folder, Godot assets

Not only do we identify different entity types simply by a uniquely identifying name, we also are able to set properties of individual nodes of each (custom) entity by being able to provide paths to the node, making full use of Godot's scene tree and specifically the scene trees of the stripped .tscn files each entity consists off.

Stripped tscn files are simply godot scenes which contian nodes that have been stripped of their content, as it is not possible to load in tscn files with resources linked. The resources must be stripped from the nodes, exported to the content folder separately together with the stripped scene, then the client loads in the stripped scenes and seperated resources seperately and links them together by itself when it spawns in  the entities. 
As you can see in the content folder, each resource that has been seperated and stripped from the tscn file from its node will be named after its node, this naming is critical for the client as this is the way it will be able to know to which node this stripped resource belongs to. Stripped resources contain a wide variety of file types such material, mesh, animation and more. Whereas stripped Godot scenes are always just tscn files, each entity must have a tscn file.
