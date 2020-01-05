# Terminology

Like *Menagerie* itself, this document is meant for everyone. I won't deny that it helps to know a programming language or two if you want to mod a game -- even this one -- but I've done my best to explain things in plain English, to use terms that you don't need a computer science degree to understand, and to maintain consistent language throughout.

### A note on definitions vs. instances

At various places in the Guidebook, I invoke the metaphor of *blueprints and houses* to describe the relationship between the (capital-E) [**Entities**](#entity--entity) defined by data files and the (lowercase-e) **entities** inside the game. In real life, multiple houses can be built from the same blueprint, which means they'll always share certain properties; they'll probably also have the same name in the builders' catalog. But once the houses are built, they become more than just a blueprint -- in addition to houses, they become *homes*, with non-blueprint properties like family members and furniture.

Our data [**definitions**](#data-definition) are a lot like blueprints. The game's `pufig.data` file is the blueprint for Pufig, which is a Monster, which is an Entity. The (lowercase-p) pufigs walking around in your garden are [**instantiations**](#instanceinstantiation) of (capital-P) Pufig, according to its definition -- in other words, houses built on the Pufig blueprint. They'll always have certain Pufig traits in common, but it's their not-explicitly-Pufig traits like personality and preferences that differentiate them from each other.

**Definitions** and **instances** always share the same name (eg, Pufig and pufig), because they are two halves of a whole -- an instance cannot exist without a definition, and a definition is meaningless if there are no instances of it. Throughout this guide, I will always differentiate them using capitalization: definitions are always capitalized, and instances are always lowercase.


## Basic Terms

#### Mod

A collection of files that modify the game in some way. Mods should always live inside their own folders, which can be placed inside the `res://mods/` or `user://mods/` directories (see [Getting Started](#) for more info).

#### Data file

The subject of this guide. A **data file** is a file *you* create as part of a mod; it defines an [**Entity**](#entity--entity). Data files should have a filename matching their [**Entity ID**](#entity-id), have extension `.data`, and be located inside a mod folder (again see [Getting Started](#)).

The contents of a data file are referred to as a [**data definition**](#data-definition) or **Entity definition**, and must be in valid JSON format.

#### Data definition

The contents of a [**data file**](#data-file) are referred to as a **data definition**, or (capital-E) **Entity definition**. You can think of a data definition as the "blueprint" for an [**Entity**](#entity--entity) -- see the section on [definitions vs. instances](#a-note-on-definitions-vs-instances) for more details.

#### Entity & entity

An **Entity** (capitalized) is anything defined by a [**data file**](#data-file). There are several [**types**](#entity-type) of Entity, including but not limited to Monsters, Items, Objects, NPCs, and Events.

An **entity** (lowercase) is an [**instantiation**](#instanceinstantiation) of an **Entity**; the house to its blueprint (see the section on [definitions vs. instances](#a-note-on-definitions-vs-instances) above). This capital/lowercase convention applies to **Entity types** (eg. Monster/monster) and specific Entities (eg. Pufig/pufig) as well.

#### Entity type

Every [**Entity**](#entity--entity) belongs to a specific **type** or *category* -- Monsters, Items, Objects, NPCs, and Events are just a few. An **Entity**'s type *must* be specified in its [**data definition**](#data-definition). Each type of **Entity** has its own properties, behavior, game logic, and [**schema**](#schema), which is documented in the [Data Specifications](#) section of this Guidebook.

#### Instance/instantiation

A programming term discussed in the section on [definitions vs. instances](#a-note-on-definitions-vs-instances) above. It's not the most intuitive term, but its usage is highly conventional, so I will be relying on it to some extent. Throughout the Guidebook, I refer to **instances** interchangeably as **game objects**.

#### Entity ID

An **Entity ID** is a unique identifier for a (capital-E) [**Entity**](#entity--entity). Every **Entity** needs its own identifier, which means that every [**Entity definition**](#data-definition) *must* contain contain an **Entity ID**.

**Entity IDs** should be *human-readable strings*, like `pufig` or `fluffy_tuft`. (Conventionally, we use lowercase for **Entity IDs**, with underscores to represent spaces.) If multiple **data definitions** are found with the same **Entity ID** when *Menagerie* loads, it will try to merge their content; see the chapter on [Data Loading](#) for more info.

#### Unique ID

A **unique ID**, also known as a (lowercase-e) *entity ID*, is a long, random sequence that uniquely identifies an [**entity**](#entity--entity). 

Unlike [**Entity IDs**](#entity-id), which are chosen by human authors, **unique IDs** are generated inside the game by an algorithm when an **entity** is created. As the name implies, no two entities should ever have the same **unique ID**.

Since they are randomly generated and not meant to be human-readable, we seldom (if ever!) deal with individual **unique IDs** *directly*. Instead, we generally obtain the **unique ID** for an **entity** from a reference to the **entity**'s [**game object**](#instanceinstantiation).

#### Schema

A **schema** describes the correct structure for a [**data file**](#data-file): you can think of it as a set of rules that the data file must follow in order for it to be valid. Each [**Entity type**](#entity-type) has its own **schema** -- eg, there is a schema for Monsters, another for Items, and so on -- that all **data files** for Entities of that type must follow. All schemas are described in the [Data Specifications](#) section of the Guidebook.

## Data Syntax

#### JSON

The data format used for *Menagerie*'s [**data files**](#data-file). JSON files must adhere to [specific formatting rules](https://www.digitalocean.com/community/tutorials/an-introduction-to-json) which make them (relatively) easy for both humans and computers to read and write.

#### Collection

A **collection** is a type of data that contains multiple other pieces of data (including, potentially, other collections!). A piece of data inside a collection is referred to as an [**element**](#element). Most of the data we care about in *Menagerie* is contained inside collections -- in fact, [**data definitions**](#data-definition) are collections themselves -- so it's important to be aware of how they work and how they're represented. *Menagerie* uses two kinds of collections: [**arrays**](#array) and [**dictionaries**](#dictionary).

#### Element

A single item in a [**collection**](#collection).

#### Array

One of the two types of [**collection**](#collection) in *Menagerie* (with the other being the [**dictionary**](#dictionary)). An **array** is simply a list of *values*. It is represented in [**JSON**](#json) with a set of square brackets (`[]`) surrounding the [**elements**](#element), and commas (`,`) between them:

```
["first element", "second element", "third element"]
```

#### Dictionary

One of the two types of [**collection**](#collection) in *Menagerie* (with the other being the [**array**](#array)). In contrast to **arrays**, where each [**element**](#element) is simply a *value*, each **element** in a **dictionary** has both a *value* and a *key*. 

In [**JSON**](#json), **dictionaries** are surrounded with a set of curly braces (`{}`) surrounding the [**elements**](#element), and commas (`,`) between them. Each **element** is represented with the *key* first, followed by a colon (`:`), and then the *value*:

```
{
  "one": "first element",
  "two": "second element"
}
```

#### Argument

#### String

### Game Internals

#### Game object

#### Scene

#### Singleton
