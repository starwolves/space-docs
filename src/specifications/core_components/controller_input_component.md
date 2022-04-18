# ControllerInput

This is the component that should be used to engage with pawns as an outside controller, ie a connected player or AI.
Unlike this component, components like `Pawn` and `Humanoid` hold some values for internal usage and editing those may result in bugs.

`movement_vector` is a 2d vector that should be limited to `(-1, 1)` ranges as `x` and `y` values.


