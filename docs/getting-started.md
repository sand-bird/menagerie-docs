# Getting Started

Mods in *Menagerie* consist of `.data` (JSON data), `.png` (image), `.tres` (Godot resource) and/or `.tscn` (Godot scene) files. These are the same types of files the actual game is built from, and for a mod to work, they must be organized in roughly the same way. When *Menagerie* launches, it looks in a few special directories for mod files, and then supplements or replaces the base game files accordingly.

*Menagerie* loads mods from the `user://mods/` and `res://mods/` directories, and prioritizes them in that order. In other words, if any files in `user://mods/` conflict with files in `res://mods/`, the former will take precedence, just as mod files take precedence over any conflicting files in the main game.

In Godot, `user://` is an alias for the "user directory," which `res://` is an alias for the "resource directory." The actual location of `user://` will depend on your operating system; in Linux, for example, it is:
```
/home/{yourname}/.local/share/godot/app_userdata/Menagerie/
``` 
The actual location of `res://` is simply the base directory where the game is installed. This will depend on your game distribution platform; for Steam on Linux, for example, the game is located at:
```
home/{yourname}/.steam/steamapps/common/Menagerie/
``` 
More info on data paths in Godot is available [in the Godot docs](https://godot.readthedocs.io/en/3.0/tutorials/io/data_paths.html).

# File Structure

Mods for *Menagerie* must mimic the file structure of the game, which is as follows:

```
game/
|-- assets/
|   |-- garden/
|   |-- items/
|   |-- objects/
|   |-- monsters/
|   |-- music/
|   |-- npcs/
|   |-- locations/
|   `-- ui/
|-- data/
|   |-- items/
|   |-- objects/
|   |-- monsters/
|   |-- npcs/
|   `-- locations/
|-- garden/
|-- item/
|-- object/
|-- monster/
|-- npc/
|-- system/
|-- town/
`-- ui/
```

(To view the full file structure, check out the game's source code [here]().)

The `garden/`, `item/`, `object/`, `monster/`, `npc/`, `system/`, `town/`, and `ui/` folders contain game logic in the form of Godot scenes (`.tscn`) and scripts (`.gd`). In Godot, scenes and scripts are generally paired, though they can function independently as well.

The `assets/` and `data/` folders contain graphical & sound assets and data files, respectively. The subfolders within `data/` organize data files by the type of entity they describe. Meanwhile, the assets for a specific entity are *conventionally* grouped into a folder named after that entity within the corresponding subdirectory of the `assets/` folder. For example, Pufig's data is contained in the file `data/monsters/pufig.data`, while its sprites, sound files, and any other assets it needs are inside the folder `assets/monsters/pufig/`. 

Putting this all together, a mod to add a monster called Pikablu would include the following files and folders:

```
my_mod/
|-- assets/
|   `-- monsters/
|       `-- pikablu/
|           |-- spritesheet_normal.png
|           |-- spritesheet_shiny.png
|           ...
`-- data/
    `-- monsters/
        `-- pikablu.data
```

# Data Files

Of course, you'll still have to fill those folders with **data files** that describe your creation in a way *Menagerie* can understand. Every type of entity in the game -- monsters, items, furniture, NPCs, and even requests, just to name a few -- has its own set of necessary properties, which data files describing that type of entity must contain.
