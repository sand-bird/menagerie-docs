# Conditions 

Many things in *Menagerie* only happen or become available under certain circumstances. A flower that only blooms when it's raining on a Tuesday, a collection of items that merchants will only sell when you've completed a certain number of requests for them, or a monster that evolves into one form when its intelligence is above a certain level, and a different form otherwise -- the possibilities are pretty much endless.

These **conditions** are a property of the [entity](/terminology/#entity-entity) they affect, so they're defined in the entity's [data file](/terminology/#data-file) using a special **condition syntax** specific to *Menagerie*. As a simple example, here's a condition requiring that the player have at least five thousand aster, the game's currency:

```json
{">=": ["$player.money", 5000]}
```

All conditions are *dictionaries* with one *key*, the **operator**, and one *value*, an **array of arguments**. (There are, however, a couple of shortcuts that break this convention, which we'll get to in a bit.)

Of special interest in the above example are `"$player.money"` and `">="` -- a **variable** and an **operator**, respectively. Together, they're the building blocks for a useful condition; we'll be discussing them, in that order, through the rest of this document.

## Variables

A condition wouldn't be very useful if it always had the same result. In fact, the whole point of a condition is that its value *depends* on *things that can change* -- **variables**.

Since *Menagerie*'s conditions live in data files, they must adhere to proper JSON syntax, just like the rest of the file. Of course, JSON only supprts a few types of values -- numbers, strings, booleans, arrays, and dictionaries -- and none of those are variables. So, what do we do...?

We use magic!

To denote a variable within a condition, we just use a plain ol' string representing the variable name. The magic happens when we *annotate* the string value with the proper **sigil**, such as `$` or `@`, which tells the game code that the string is representing a variable. Whenever the game comes across a condition, it looks for string values marked with sigils, and then *parses* the annotated string into the variable it represents.

!!! warning
     You can type a sigil in front of any string you want, of course, though it won't work as a variable unless it actually has something to reference. The game *should* be safe from crashing due to invalid variables, though you can never rule it out completely...

### Global Variables

Structurally, you can think of *Menagerie* as a thing of two complimentary halves. [Entities](/terminology/#entity-entity) -- anything defined using data files -- make up one half. The other half is "the system", or "the game", which handles the logic that makes things run. But it also contain quite bit of *state* -- that is, variables that describe things about it. Conditions will often depend on some aspect of the game's state, so they must be able to access it somehow. That's where **global variables** come in.

!!! trivia
    **State** is another programming term -- when we use it to describe software constructs, anyway. Really, though, it applies to anything: the state of a door (open or closed, locked or unlocked, oiled or creaky); the state of a relationship; the state of the union; a person's mental or emotional state. (Not the political concept of a state, though -- that one's different.)

Global variables are annotated with `$` and come in several delicious flavors, most of which correspond directly to *system classes* in the game's code, like `Player` and `Time`. For conditions, accessing these system classes themselves is neither useful nor a good idea. Instead, think of them as *categories*: we're really interested in their **properties**, which we can access with a period (`.`), like this:

```json
$player.money
```

Look familiar?

The following is a complete list of the global properties accessible through *Menagerie*'s condition syntax, along with some information about what they contain.

#### $player

Properties of the player character, accessed through the `Player` *singleton* (`res://system/player.gd`), and saved to the `player.save` file. 

!!! trivia
    A few other global variables, like `$encyclopedia` and `$inventory`, are technically also properties of `Player`, but they're important enough to have their own shortcuts in the syntax.

| property | type | description |
| --- | --- | --- |
| `playtime` | number | Total playtime in seconds |
| `money` | number | Player's wealth in Aster |
| `name` | string | Whatever the player entered as their name when they started the game |
| `experience` | number | Player's total experience points |
| `level` | number | Player's current level |


#### $garden

Properties of the garden, including collections of the entities -- monsters, items, and objects -- inside, which can be used to access *their* properties. This information is saved to the `garden.save` file. 

!!! trivia
    Unlike `Player`, there is no singleton class for the garden: since its purpose is to host *instanced nodes* for all the entities inside it, it exists as an instanced node itself, `root/game/garden` in the scene tree.

| property | type | description |
| --- | --- | --- |
| `monster_count` | number | Number of monsters in the garden |
| `item_count` | number | Number of items in the garden |
| `object_count` | number | Number of objects in the garden |
| `terrain` | TileMap | The terrain values for every tile in the garden  |
| `monsters` | Dictionary | A dictionary containing every monster resident in the garden. Its keys are the monsters' **unique IDs**, and its values are references to the monster's **game object**, from which its individual properties can be accessed. |
| `items` | Dictionary | A dictionary containing every item in the garden. Again, its keys are the items' **unique IDs**, and its values are references to the item's **game object**. |
| `objects` | Dictionary | A dictionary containing every object (that is, anything placeable on the grid) in the garden. Its keys are the objects' **unique IDs**, and its values are references to the object's **game object**. |
| `tier` | number | The tier of the garden, reflecting how much it's been upgraded; see the `GARDEN_TIER` constant. |

#### $time

References the game's clock, which lives in the `Time` singleton (`res://system/time.gd`).

| property | type | description |
| --- | --- | --- |
| `tick` | number | Current tick: the basic unit of game time, corresponding to 5 in-game minutes. A number between `0` and `11`. |
| `hour` | number | Current hour of the day. A number between `0` (midnight) and `23` (11pm).
| `day` | number | Current day of the week; see the `DAY` constant. |
| `date` | number | Current day of the month, corresponding with the displayed date: `1` for the first of the month, and so on. *(Note: In the game's code, the actual `Time.date` value is `0` for the 1st of the month; referencing it this way adds 1 to that.)* |
| `month` | number | Current month; see the `MONTH` constant. |
| `year` | number | Current year. Like `date`, starts at `1` for the first year, even though the actual `Time.year` value starts at `0`. |


#### $data

References the `Data` singleton, which is where the data in every data file ends up, indexed by its ID -- that is, the value of the `"id"` key within the file. Since conditions are evaluated during gameplay -- long after data files have already been loaded -- it's possible and totally safe to access the data containing a condition using `$data` inside that condition. You can even access the raw condition itself!


### Local Variables

A data file is like a blueprint. We can think of *Menagerie*'s garden as a planned neighborhood, where each individual monster is one house: the people in charge have selected a handful of blueprints (that is, monster species), and used one or another of them to build every house in the area.

!!! tip
    In programming terms, monster species -- and any other type of entity represented by a data file -- are analogous to classes, with the monsters themselves as instantiations of those classes.

Of course, a house is more than its blueprint -- the interior decorations, the landscaping, construction details like roof color and floor tiles, and of course the people living in it are all "house properties" that have nothing to do with the blueprint, and that make every home unique. In just the same way, an individual monster in *Menagerie* has many "monster properties" that aren't specified in the blueprint -- the data file -- and that help to make it distinct from other monsters of the same species. These properties are our **local variables**.

!!! note
    I'm using monsters as examples because -- as the most complicated of *Menagerie*'s entities -- they have by far the most local variables, so this section is most relevant to them. 
    
    A complete list of local variables for each entity type, and how to use them, can be found on that entity's page in the **Data Specifications** section of this guide.

A local variable is prefixed by the sigil `@`, like so:

``` json
{">": [ "@mood", 30 ]}
```

This condition expects somebody's `mood` to be greater than 30. But whose?

!!! note
    `mood` is a property of monsters, so this example would probably be found in the data definition for a Monster.

Well, it depends on the context. When used alone like this, it refers to the `mood` of any entity of the kind described by the data type -- any house matching the blueprint. For example, in the data file for a monster, we can use conditions to specify some *requirements* for that monster to drop certain resources, or to evolve in certain ways. Every monster of that species is subject to those requirements, and *fills local variables with its own individual properties* to evaluate whether that condition is `true` or `false` *for itself*.

Later on, we'll learn how to access local variables of other entities, using somewhat special kinds of operators called **data operators**. 

### Constants

What is a Tuesday? On that note, what is a day of the week? As far as real life is concerned, that may be one for the philosophers. In *Menagerie*, though, we can say with certainty that a Tuesday is a number: specifically, it's 2.

That's a silly way of saying that on in-game Tuesdays, the global variable `$time.day` -- which tracks the day of the week -- is equivalent to `2`. It's not just weekdays, either; a quick peek at the Global Variables section can tell us that a whole bunch of global variables are numbers, even when they represent *things*. Computers much prefer it that way, after all; if they had to deal with a string for every weekday, they'd start to get depressed.

"Wait a minute," you might be saying. "If we had to go around substituting `2` for Tuesday all over the place, we'd get depressed too -- uh, also!"

#### TYPE

```
MONSTER
ITEM
OBJECT
NPC
LOCATION
EVENT
REQUEST
CONTACT
```

#### CATEGORY

```
PLANT
DECORATION
RESOURCE
INGREDIENT
FOOD
```

#### DAY

```
SUNDAY
MONDAY
TUESDAY
WEDNESDAY
THURSDAY
FRIDAY
SATURDAY
```

#### MONTH
```
VERNE
TEMPEST
ZENITH
SOL
HEARTH
HALLOW
AURORA
RIME
```

#### INPUT_TYPE

```
TOUCH
MOUSE_AND_KEY
JOYPAD
KEY_ONLY
```

#### TERRAIN

```
DIRT
SAND
GRAVEL
GRASS
WATER
```


## Operators

The condition at the beginning of this page features a `">="` operator, which is an example of a **comparison operator** (or **comparator** for short). Here's slightly more complex condition showing off some other kinds of operators:

```
{"and": [
    {"==": ["$time.day", "#DAY.FRIDAY"]},
    {"in": ["bubbling_drink", "$inventory.items"]}
]}
```

Are you ready for a good time? This condition returns true if it's Friday and the player has an item with the id `bubbling_drink` in their inventory. Here we see the comparator `"!="`, which stands for "not equals", the **relational operator** `"in"`, and the **boolean operator** `"and"`. Let's take these categories one at a time.


### Relational Operators

Unsurprisingly, **relational operators** describe a *relationship* between their two arguments, and return a `true` or `false` value depending on whether that description is accurate. **Comparison operators** are relational operators where the relationship is, specifically, a *comparison* between the two.

Relational operators compare two things at a time, so they require **exactly two arguments** in their argument array. Comparison operators additionally require those two arguments to be of the **same type** -- generally, both numbers.

The relational operators used in *Menagerie*'s condition syntax are as follows:

| operator | returns true if... |
| --- | --- |
| `"=="` | the arguments are **equal** |
| `"!="` | the arguments are **not equal** |
| `">"` | the first argument is **greater than** than the second |
| `"<"` | the first argument is **less than** than the second |
| `">="` | the first argument is **greater than or equal to** the second |
| `"<="` | the first argument is **less than or equal to** the second
| `"in"` | the first argument is a **member of** the second |

The `"in"` operator is a little bit special, since it's the only relational operator in the list that isn't also a comparator. Instead, the `"in"` operator has a special requirement: its second argument must be a *collection* of values, either a dictionary or an array. 

(TODO: check if you can `"in"` a string)

### Boolean Operators

A condition is actually a *boolean value* -- something that is either `true` (when the condition is met), or `false` (when the condition is *not* met). We've seen that relational operators will return `true` or `false` depending on the relationship between the two values they're given, for example.

But what about conditions that depend on multiple relationships? Our last example was one such case: the player must have a `bubbling_drink` in their inventory, *and* it must be Friday. The key here is the `"and"` operator, which requires that *both those conditions be `true` at the same time*.

Along with its partner `"or"`, the `"and"` operator is known as a **boolean operator**, since it accepts *boolean values* -- in other words, *other conditions* -- as its arguments. Unlike relational operators, boolean operators can have an *unlimited number of arguments*. Here's how they work:

| operator | returns true if... |
| --- | --- |
| `"and"` | **all** of the arguments are `true` |
| `"or"` | **at least one** of the arguments is `true` |


#### Shortcut

Because the `"and"` operator is so important, it has a special shortcut. Remember how a condition must always be a *dictionary* with the operator as its only key? If *Menagerie* finds an *array* instead, it will treat it as though it were the *argument array* of an `"and"` operator, like so:

``` json
[
  {"==": ["$time.day", "#DAY.FRIDAY"]},
  {"in": ["bubbling_drink", "$player.inventory"]}
]
```

This is equivalent to the full `bubbling_drink` example from before.

!!! trivia
    The programmer jargon for this sort of thing is "syntactic sugar", which refers to any alternate, more convenient syntax for some functionality.

#### Thinking in boolean

Let's say we're creating an event, and we want it to happen on *Tuesdays and Thursdays*. We might first try something like this:

``` json
{"and": [
    {"==": ["$time.day", "#DAY.TUESDAY"]},
    {"==": ["$time.day", "#DAY.THURSDAY"]}
]}
```

This is a perfectly-written condition, except for one little problem... it will never return `true`! To understand why, you have to learn to **think in boolean**: even though `and` and `or` are English words, they don't mean the same thing as their natural-language counterparts, so you must take care when "translating" from one to the other. 

For example, if you ask your friend, "do you want to go to the movies **or** the park," she'd probably *choose one* (or she might respond with "neither, how about the beach?"). But what if you asked a computer that only understood boolean `or`? You might still get a "neither," or `false`. But if the computer liked one of your suggestions, it wouldn't tell you which one -- it would just say "sure!" (`true`).

So, let's try and consider our example from a computer's perspective. In natural English, you want the event to be available on Tuesday *and* Thursday. But remember, the `"and"` operator returns `true` if *all of* the arguments are `true`. What are the arguments?

```json
{"==": ["$time.day", "#DAY.TUESDAY"]}
```
"Today is Tuesday."

```json
{"==": ["$time.day", "#DAY.THURSDAY"]}
```
"Today is Thursday."

See the problem? Yep -- that `"and"` operator is telling the game that for the condition to be `true`, today needs to be *both* Tuesday *and* Thursday!

To avoid pitfalls like these, always try to phrase your intention so that it matches the structure of conditions as closely as possible. Start from the simplest parts -- relations, like "today is Tuesday," -- and build up from there. When you phrase expectations this way, the right boolean operator comes naturally:

> The event should be available if today is Tuesday, ***or*** if today is Thursday.

!!! tip
    Like just about any other kind of programming, there's often more than one way to write a condition. Instead of using an `"=="` condition for each valid day, you could also list them out with an as an array, and use the `"in"` operator to check if the current day is a member of the list:
    
    ```json
    {"in": [
      "$time.day", 
      ["#DAY.TUESDAY", "#DAY.THURSDAY"] 
    ]}
    ```

    We'll see more examples like this one in the section [Putting it All Together](#).
    
!!! tip
    If you enjoy writing conditions, you might appreciate **boolean logic**, a whole subfield of mathematics that deals with `true` and `false` values, in which the simple `and` & `or` that we use here are the building blocks for extraordinarily complex operations. (Raymond Smullyan's classic puzzle anthology *What is the Name of This Book?* is a fantastically fun introduction to the topic. Or, if you prefer something a little more practical, you might also enjoy *applied* boolean logic -- more commonly known as computer engineering.)


## Data Operators

There is a third-generation Pokémon called [Shedinja](https://bulbapedia.bulbagarden.net/wiki/Shedinja_(Pokémon)) with a rather unique evolutionary requirement: when your Nincada -- a cicada-inspired Pokémon -- evolves normally into a Ninjask, Shedinja -- the "shed Pokémon" -- will spontaneously appear in your party as long as you have an empty slot and a spare Pokéball.

One of the major goals of *Menagerie*'s data structure is to enable similarly complex behavior -- extensibly, modularly. We've already learned how to use global and local variables to access various bits of the game state. But we can't access *everything* that way -- so to make our own Shedinja, we're going to neet to manipulate the data we can get from those variables. **Data operators** let us do just that.

### get

We've already seen the `"get"` operator in action: it's the period (`.`) in `$player.money` and `#day.tuesday`. That's actually the short form: in its full glory, `"get"` looks a lot like any other condition operator:

```json
{"get": [
  "$player", "money"
]}
```

!!! trivia
    When *Menagerie* parses -- that is, reads and inteprets -- conditions, it actually converts the `.` form of `"get"` to its full form before resolving it. You can use the full form in your conditions if you want, though the `.` form is a lot more readable. 

The `"get"` operator takes two *arguments*, and they must both be *strings*. This is a given when using the `.` form, since it's already in the middle of a string, and the arguments are whatever's on either side. If you decide to use the full form, though, it's something to keep in mind.

The first argument (on the left side of the `.` in the short form) is the **parent**, and the second (on the right side) is the **key**. The key identifies a *property* of the parent. When the condition is evaluated, *Menagerie* checks if that property actually exists; if it does, the result of the `"get"` operation will be the value of that property.

You can use multiple `"get"` operators within a single string:

```
$data.monsters.pufig.description
```

This example grabs the `description` string -- the flavor text that shows up on encyclopedia pages, in the inventory (for Items and Objects), and in shops -- from the Monster with ID `pufig`.

The parent in a `"get"` operation can be *either a **dictionary** or a **game object*** -- it works the same either way. You can see this in our example above: `$data` is a game object (the `Data` system class) with a property called `monsters`, which contains a dictionary with a key called `pufig`, which contains yet another dictionary with all of Pufig's data inside (including its description text under the key `description`).

### map

The `"map"` operator is `"get"`'s big sister. Like `"get"`, it has a short form -- the colon (`:`) -- as well as a full form. Here it is in action:

```
$garden.monsters:given_name
```

or, in long form,

```json
{ "map": [
  "$garden.monsters",
  "given_name"
]}
```

This will return an array containing the `given_name` for every monster in your garden, like this:

```
[ "Fido", "Bumblebottom", "Hedwig", "Mr. Pie", ... ]
```

How does it work? Basically the same way as `"get"` -- `"map"` takes two string arguments, and like `"get"`, the *second* argument (the right side, in the `:` form) is a **key** that identifies the property we want. 

But `"map"` has one weird twist: the *first* argument (the left side in the `:` form). With `"get"`, the first argument is the *parent*, the dictionary or game object containing our key. With `"map"`, it's the **grandparent**: a *collection* -- either a dictionary or array -- of *parents*. When you use `"map"` on grandparent and a key, it tries to `"get"` that key from each of the grandparent's children, and returns all of the results together in an array.

!!! note
    For those from a programming background: `{"map":["grandparent","key"]}` is roughly equivalent to `grandparent.map{|p| p.key}` in Ruby, or `map(lambda p: p.key, grandparent)` in Python. If `grandparent` is a dictionary, it maps over `grandparent.keys` instead.

Don't worry if this is confusing at first! `"map"` one of the more advanced operators -- it requires advanced knowledge of *Menagerie*'s data structure to use effectively, and likely some practice too.

### filter

Our final and most complicated data operator (for now!) is `"filter"`. Unlike `"get"` and `"map"`, it has one form only:

```json
{ "filter": [
  "$garden.monsters",
  { ">": [
    "*.iq", 30
  ]}
]}
```

`filter` accepts two arguments: the first, as with `map`, is a string that must resolve to a **collection** (a dictionary or array, though not a game object). The second argument is a **condition**. 

As the name implies, `filter` will filter the collection according to the condition: it returns a collection just like the one it was given, but only the items that cause the condition to return `true` will be included.

Accordingly, the condition inside a filter should depend in some way on the items in a collection. We can accomplish this with two new, special sigils: `*` and `^`.

#### *

The `filter` and `map` operators -- those that require *collections* as their first argument -- work by going through every item in the collection, one at a time. For `filter`, this means that the second argument, the condition, is evaluated over and over again, once for every item in the collection. The `*` sigil connotes that item.

To illustrate, let's revisit our `filter` example above. This filter is operating on the collection `$garden.monsters`, and its condition is as follows:

```
{">": [ "*.iq", 30 ]}
```

First, let's consider the collection, `$garden.monsters`. As described in the [Global Variables](#) section of this guide, `$garden.monsters` is a dictionary. Its keys are the *unique IDs* of the monsters in the garden -- not Entity IDs, which are chosen by human authors, but long, random sequences generated inside the game by an algorithm whenever a new monster is born or received.

#### ^



### Advanced usage

Let's revisit our `map` example for a second:

```
$garden.monsters:given_name
```

is the same as

```
{ "map": [
  "$garden.monsters",
  "given_name"
]}
```

Notice that `"$garden.monsters"`? Yep, that's a `get` inside a `map`. This is why *Menagerie* converts the `.` and `:` forms of `get` and `map` to their long forms before it resolves them -- it needs to resolve `"$garden.monsters"` before it can do a `map` on it

## Putting it all together

----


.

.

.

.


`$garden.monsters`:

```
[ Monster#12345, Monster#5437, ... ]
```

`$garden.monsters:preferences`:

```
[
  {
    "monsters": {
      "3789": 5,
      "6732": 12,
      ...
    },
    "items": { ... },
    ...
  },
  {
    "monsters": { ... },
    "items": { ... },
    ...
  },
  ...
]
```

`$garden.monsters:preferences:monsters`:

```
[
  {
    "3789": 5,
    "6732": 12,
    ...
  },
  { 
    "4773": -2 
  },
  ...
]
```


`$garden.monsters:preferences:monsters:@id.max`

```
[ 5, 10, -8, 22, 16 ]
```


`{">=": [{"max": "$garden.monsters:preferences:@id" }, 30]}`


----------------------

`@preferences`

```
{
  "monsters": {
    "3789": 5,
    "6732": 12,
    ...
  },
  "items": { ... },
  ...
}
```


`@preferences.monsters`

```
{
  "3789": 5,
  "6732": 12,
  ...
}
```

-------------

`$garden.monsters`

```
{ "1234": Monster#1234, "5678": Monster#5678 }
```

`$garden.monsters:*`

```
[ Monster#1234, Monster#5678 ]
```

```
{ "filter": 
    [ 
        "$garden.monsters",
        { "==": ["^.*.id", "@favorite_monster.id"] }
    ]
}
```

*: 1234, 5678



.iq

```
{ "get":
    [
        "$garden.monsters",
        "@preferences.monsters"
    ]
}
```
       
        
`$garden.monsters.@preferences.monsters`





```
"map": [
  "$garden.monsters", { 
    "map": [
      "*",
      { 
        "map": [
          "preferences", 
          "monsters.@id"
        ]
      }
    ]
  }
]



{"map": [
  {"map": [
    {"map": [
      "$garden.monsters",
      "*"
    ]},
    "preferences"
  ]},
  "monsters.@id"
]}


{"get":[
  {"get":[
    "$garden",
    {"map":[
      {"map":[
        {"map":[
          "monsters",
          "*"
        ]},
        "preferences"
      ]},
      "monsters"
    ]}
  ]},
  "@id"
]}


{"map":[
  {"map":[
    {"map":[
      {"get":[
        "$garden",
        "monsters"
      ]},
      "*"
    ]},
    "preferences"
  ]},
  {"get":[
    "monsters",
    "@id"
  ]}
]}
```

```
{"get":[{"map":[{"map":[{"map":[{"get":["$garden","monsters"]},"*"]},"preferences"]},"monsters"]},"@id"]}
```

.

.

.

.


1. Basic comparison
-------------------

Type A:
```````````````````````````````````````````````````````````
{ ">": [ "$player.money", 5000 ] }
```````````````````````````````````````````````````````````
Type B:
```````````````````````````````````````````````````````````
[ ">", "$player.money", 5000 ]
```````````````````````````````````````````````````````````


2. Boolean AND on multiple comparisons
--------------------------------------

Type A:
```````````````````````````````````````````````````````````
{ "and": [
    { ">": [ "$player.money", 5000 ] },
    { "in": [ 
        "$time.day",  
        [ "#day.tuesday", "#day.wednesday" ]
    ] }
] }
```````````````````````````````````````````````````````````

Type A (shorthand):
```````````````````````````````````````````````````````````
[
  {">": [ "$player.money", 5000 ]},
  {"in": [
      "$time.day",  
      ["#day.tuesday", "#day.wednesday"]
  ]}
]
```````````````````````````````````````````````````````````

Type B:
```````````````````````````````````````````````````````````
[ "and", 
  [ ">", "$player.money", 5000 ],
  [ "in", 
    [ "$time.day", 
      [ "#day.tuesday", "#day.wednesday" ] ] ] ]
```````````````````````````````````````````````````````````

Type B (shorthand):
```````````````````````````````````````````````````````````
[ [ ">", "$player.money", 5000 ],
  [ "in", 
    [ "$time.day",  
      [ "#day.tuesday", "#day.wednesday" ] ] ] ]
```````````````````````````````````````````````````````````



3. Filtering data via boolean expression
----------------------------------------

A:
```````````````````````````````````````````````````````````
{ "filter": [ 
    "$garden.monsters",
    { "==": ["*.id", "@partner.id"] }
] }
```````````````````````````````````````````````````````````

B:
```````````````````````````````````````````````````````````
[ "filter",
  "$garden.monsters",
  [ ">=", "*.", 30 ] ]
```````````````````````````````````````````````````````````