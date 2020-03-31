# Introduction

Hi there! If you're interested in modding *Menagerie*, you're in the right place. This is the **Menagerie Guidebook**, a *luxuriously detailed* reference document for *Menagerie* content creation. 

I put this book together as a reference for myself over the course of developing the game. It's split into two main parts: the **[General Information](#)** section details the nuts and bolts of *Menagerie*'s data files, while the **[Data Specifications](#)** section details the *schemas* -- lists of all required *and* optional properties, and the types of information they should contain -- for every kind of Entity in the game, with plenty of explanations and examples for each.

(organization note: separate reference and guide sections and links to both here, framed as "learn by doing" or "understand first" approaches)

### How to Use This Guide

The Guidebook is equal parts reference and tutorial. 


### Getting Started

Mods in *Menagerie* consist of `.json` (JSON data), `.png` (image), `.tres` (Godot resource) and/or `.tscn` (Godot scene) files. These are the same types of files the actual game is built from, and for a mod to work, they must be organized in roughly the same way. When *Menagerie* launches, it looks in a few special directories for mod files, and then supplements or replaces the base game files accordingly.

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





