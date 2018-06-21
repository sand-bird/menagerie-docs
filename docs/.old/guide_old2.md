The `garden`, `monster`, `npc`, `system`, `town`, and `ui` folders contain game logic in the form of Godot scenes (`.tscn`) and scripts (`.gd`). In Godot, scenes and scripts are generally paired, though they can function independently as well.

The `assets` and `data` folders are special: they contain sprites and data files, respectively. All of the data files relied upon by scenes in the `monster` folder can be found within `data/monster`, and so on.

Resources for specific monsters, objects, items, or characters is organized into subfolders, one for each entity. For example, Pufig's data is inside `data/monster/pufig`, while its sprites are inside `assets/monster/pufig`. 
