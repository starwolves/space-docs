# Pawns

Pawns and players are seperated into several different components. Each pawn has the components:
1. `Pawn` which holds some simple pawn properties.
2. `ControllerInput`  which is used to actually control the pawn, either from a player connection as controller or arteficial intelligence.
3. `pawn type`, currently `Humanoid` is the only existing pawn type.

Most pawn components also hold internally required values that you probably shouldn't change, `ControllerInput` is the exception. 


