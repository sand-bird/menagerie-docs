A condition wouldn't be very useful if it always had the same result. In fact, the whole point of a conditional is that its value *depends* on *things that can change* -- **variables**.

Since *Menagerie*'s conditions live in data files, they must adhere to proper JSON syntax, just like the rest of the file. Of course, JSON only supprts a few types of values -- numbers, strings, booleans, arrays, and dictionaries -- and none of those are variables. So, what do we do...?

We use magic!

To denote a variable within a condition, we just use a plain ol' string representing the variable name. The magic happens when we *annotate* the string value with the proper **sigil**, such as `$` or `@`, which tells the game code that the string is representing a variable. Whenever the game comes across a condition, it looks for string values marked with sigils, and then *parses* the annotated string into the variable it represents.


## Global Variables

Using a **global variable** in a condition lets you access aspects of the game itself. Global variables are annotated with `$`, and they come in several flavors -- many of which correspond more or less directly to system classes in the game's code, like `Player` and `Time`. You can think of these flavors as *categories*: we're really interested in the **properties** of the classes they represent, which we can access by joining them with a period (`.`), like this:

```
$player.money
```

Look familiar?

All the global variables are listed below, along with some information about what they contain. All numbers are integers -- whole numbers, without fractions or decimal places -- unless otherwise specified.

### $player

Properties of the player character, accessed through the `Player` singleton (`res://system/player.gd`), and saved to the `player.save` file. (Technically, a few other global variables, like `$encyclopedia` and `$inventory`, are also properties of `Player`, but they're important enough to have top-level shortcuts.)

| property | type | description |
| --- | --- | --- |
| `playtime` | number | Total playtime in seconds |
| `money` | number | Player's wealth in Aster |
| `name` | string | Whatever the player entered as their name when they started the game. |
| `experience` | number | Player's total experience points |
| `level` | number | Player's current level |


### $garden

Properties of the garden, including collections of the entities -- monsters, items, and objects -- inside, which can be used to access their properties. This information is saved to the `garden.save` file. 

!!! trivia
    Unlike `Player`, there is no singleton class for the garden: since its purpose is to host instanced nodes for all the entities inside it, it exists as an instanced node itself, `root/game/garden` in the scene tree.

| property | type | description |
| --- | --- | --- |
| `monster_count` | number | Number of monsters in the garden |
| `item_count` | number | Number of items in the garden |
| `object_count` | number | Number of objects in the garden |
| `terrain` | TileMap | The terrain values for every tile in the garden  |
| `monsters` | Dictionary | A dictionary containing every monster resident in the garden. Its keys are the monsters' **unique IDs**, and its values are references to the **actual monster entity**, from which the monster's individual properties can be accessed. |
| `items` | Dictionary | A dictionary containing every item in the garden. Again, its keys are the items' **unique IDs**, and its values are references to the **actual item entity**. |
| `objects` | Dictionary | A dictionary containing every object (that is, anything placeable on the grid) in the garden. Its keys are the objects' **unique IDs**, and its values are references to the **actual object entity**. |
| `tier` | number | The tier of the garden, reflecting how much it's been upgraded; see the `GARDEN_TIER` constant. |

### $time

References the game's clock, which lives in the `Time` singleton (`res://system/time.gd`).

| property | type | description |
| --- | --- | --- |
| `tick` | number | Current tick: the basic unit of game time, corresponding to 5 in-game minutes. A number between `0` and `11`. |
| `hour` | number | Current hour of the day. A number between `0` (midnight) and `23` (11pm).
| `day` | number | Current day of the week; see the `DAY` constant. |
| `date` | number | Current day of the month, corresponding with the displayed date: `1` for the first of the month, and so on. *(Note: In the game's code, the actual `Time.date` value is `0` for the 1st of the month; referencing it this way adds 1 to that.)* |
| `month` | number | Current month; see the `MONTH` constant. |
| `year` | number | Current year; starts at `1` for the first year. *(Note: as with `date`, the actual `Time.year` value starts at `0`.)* |


### $data

References the `Data` singleton, which is where the data in every data file ends up, indexed by its ID -- that is, the value of the `"id"` key within the file. Since conditions are evaluated during gameplay -- long after data files have already been loaded -- it's possible and totally safe to access the data containing a condition using `$data` inside that condition. You can even access the raw condition itself!

### $weather


### Constants



## Local Variables

A data file is like a blueprint. We can think of *Menagerie*'s garden as a planned neighborhood, where each individual monster is one house: the people in charge have selected a handful of blueprints (that is, monster species), and used one or another of them to build every house in the area.

!!! analogy
    In programming terms, monster species -- and any other type of entity represented by a data file -- are analogous to classes, with the monsters themselves as instantiations of those classes.

Of course, a house is more than its blueprint -- the interior decorations, the landscaping, construction details like roof color and floor tiles, and of course the people living in it are all "house properties" that have nothing to do with the blueprint, and that make every home unique. In just the same way, an individual monster in *Menagerie* has many "monster properties" that aren't specified in the blueprint -- the data file -- and that help to make it distinct from other monsters of the same species. These properties are our **local variables**.

!!! note
    Since monsters are by far the most complicated of *Menagerie*'s entities, they have the most local variables, and this discussion is most relevant to them. Other entities also have local variables, though; a complete list of local variables for each entity type, and how to use them, can be found on that entity's page in the schema section of this guide.

A local variable is prefixed by the sigil `@`, like so:

```
{ ">": [ "@mood", 30 ] }
```

!!! note
    `mood` is a property of monsters, so this example would probably be found in a monster's data file. You can type an `@` in front of any string you want, of course, but it won't work as a variable unless it actually has something to reference.

We want somebody's `mood` to be greater than 30, but whose?

It depends on the context. When used alone like this, it refers to the `mood` of any entity of the kind described by the data type -- any house matching the blueprint. For example, in the data file for a monster, we can use conditions to specify some *requirements* for that monster to drop certain resources, or to evolve in certain ways. Every monster of that species is subject to those requirements, and *fills local variables with its own individual properties* to evaluate whether that condition is `true` or `false` for *itself*.

Later on, we'll learn how to access local variables of other entities, using special kinds of operator we'll call **data operators**. 
