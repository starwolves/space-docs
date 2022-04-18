# Field Of View

There are two types of sensing calculations made, one for visible entities and one for audible entities. Sound effects currently have a simple fixed distance check to see if it is audible, meaning sounds are heard through walls. Whereas visible, `Sensable` entities are bound to a *recursive shadow casting* gridmap [field of view algorithm](https://github.com/starwolfy/doryen-fov). 

All entities with the `Sensable` component are bound to these FOV checks performed by entities with `Senser` components. Usually `Senser` entities also have a `Sensable` component for themselves.

Netcode updates about entities are only sent to clients when the possessed pawn actually senses those entities. This field of view system is heavily integrated into the game and also serves as anti-cheat protection as it is performed server-side.
