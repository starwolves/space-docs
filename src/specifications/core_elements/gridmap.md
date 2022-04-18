# Gridmap

The world consists of a gridmap. As you can see from media of Space Frontiers in action, each spaceship (aka map) consists of tiles. The dimensions of those tiles are 2x2x2m. The map size is 500 by 500 tiles at this moment, but expanding this to 2500 by 2500 tiles would be great. However such an expansion would require optimization of atmospherics and field of view algorithms.

The map also consists of more than one gridmap overlays. Currently there is a `main` gridmap overlay for real ship structure such as floors and walls and there is a `details` gridmap overlay that is more for overlaying repetitive `details` over the existing `main` tiles.

The gridmap is loaded up from one of the maps inside `data/maps/`. The json files found there are exported from the unreleased map and content editor. 

The live gridmap data are stored in resources `GridmapMain`, `GridmapDetails1` and `GridmapData` found inside `src/space/core/gridmap/resources.rs`.
Each gridmap cell has its own coordinated. Since gridmap data is stored in a vector and we know the min and max coordinates we can easily turn the coordinates into a usize. This way we iterate much faster over the gridmap. You can find functions that obtain the indexes in `src/space/core/gridmap/functions/gridmap_functions.rs`.
