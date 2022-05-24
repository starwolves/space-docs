# Collider

This is a [Collider](https://rapier.rs/docs/user_guides/bevy_plugin/colliders) from the `bevy_rapier` physics plugin.

In this project a `Collider` component **should not** be added to an entity that already has a `RigidBody` component. Instead add colliders as a child of that `RigidBody`.

Please take a look at similar entities and their spawn functions to find out how to set this one up.
