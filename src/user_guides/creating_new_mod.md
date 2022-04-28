# Creating and adding a new mod

First and foremost, during this early *pre-alpha* stage we do not intend to support mods (aka plugins) yet. Mainly because APIs will still change and we do not see why contributed and pushed code should be put into its own plugin at this in moment in time. All code contributions must be created inside the main module which is the `space` module.

This guide currently just serves to show and explain how mods and plugins will be used in the future.

To create your own mod, you will have to create a new folder inside `src/plugins/` and then create a [Bevy ECS plugin](https://bevyengine.org/learn/book/getting-started/plugins/) inside of it. Once that is done you could then add your own gridmap and entity initializer systems by copying what `space core` does at `src/entities/mod.rs` and `src/core/gridmap/mod.rs`. When creating a mod it is suggested to stick to the same code layout as the core. This way new entities and ship tiles are managed and added by managing plugins. Each Bevy ECS plugins allow you to add modular components, events, resources and systems that can work right next to the space core without requiring complex code integration.
