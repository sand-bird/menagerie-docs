# Terminology

Like *Menagerie* itself, this document is meant for everyone. I won't deny it helps to know a programming language or two if you want to mod a game -- even this one -- but I've done my best throughout to explain things in plain English, to use terms that you don't need a computer science degree to understand, and to maintain consistent language throughout.

### A note on definitions vs. instances

At various places in the Guidebook, I invoke the metaphor of **blueprints and houses** to describe the relationship between the (capital-E) *Entities* defined by data files and the (lowercase-e) *entities* inside the game. In real life, multiple houses can be built from the same blueprint, which means they'll always share certain properties; they'll probably also have the same name in the builders' catalog. But once the houses are built, they become more than just a blueprint -- in addition to houses, they become *homes*, with non-blueprint properties like family members and furniture.

Our data **definitions** are a lot like blueprints. The game's `pufig.data` file is the blueprint for Pufig, which is a Monster, which is an Entity. The (lowercase-p) pufigs walking around in your garden are **instantiations** of (capital-P) Pufig, according to its definition -- in other words, houses built on the Pufig blueprint. They'll always have certain Pufig traits in common, but it's their not-explicitly-Pufig traits like personality and preferences that differentiate them from each other (and from other monsters).

Definitions and instances always share the same name (eg, Pufig and pufig), because they are two halves of a whole -- an instance cannot exist without a definition, and a definition is meaningless if there are no instances of it. Throughout this guide, I will always differentiate them using capitalization: definitions are always capitalized, and instances are always lowercase.


## Basic Terms

#### Mod

A collection of files that modify the game in some way. Mods should always live inside their own folders, which can be placed inside the `res://mods/` or `user://mods/` directories (see [Getting Started](#) for more info).

#### Data file

The subject of this guide. A **data file** is a file *you* create as part of a mod; it defines an *Entity*. Data files should have a filename matching their Entity ID, have extension `.data`, and be located inside a mod folder (again see [Getting Started](#)).

The contents of a data file can be referred to as a *data definition* or an *Entity definition*, and must be in valid JSON format.

#### Data definition

The contents of a *data file* are referred to as a **data definition**, or **Entity definition**. You can think of a data definition as the "blueprint" for an *Entity* -- see the section on [definitions vs. instances](#a-note-on-definitions-vs-instances) for more details.

#### Entity & entity

An **Entity** (capitalized) is anything defined by a *data file*. There are several *types* or *categories* of Entity, including but not limited to Monsters, Items, Objects, NPCs, and Events.

An **entity** (lowercase) is an *instantiation* of an Entity; the house to its blueprint (see the section on [definitions vs. instances](#a-note-on-definitions-vs-instances) above). This capital/lowercase convention applies to Entity subtypes and individual Entity definitions as well.

#### Instance/instantiation

A programming term discussed in the section on [definitions vs. instances](#a-note-on-definitions-vs-instances) above. It's not the most intuitive term, but its usage is highly conventional, so I will be relying on it to some extent.

#### Entity ID

Part of the *schema* for every kind of data file. A unique identifier for the Entity you are defining in the data file.

An **Entity ID** should be a *human-readable string*, like `pufig` or `fluffy_tuft`. If multiple data files are found with the same Entity ID when *Menagerie* loads, it will try to merge their content; see the chapter on [Data Loading](#) for more info.

#### Unique ID

A **unique ID**, also known as a (lowercase-e) *entity ID*, is a long, random sequence that uniquely identifies a

Unlike *Entity IDs*, which are chosen by human authors, **unique IDs** are long, random sequences generated inside the game by an algorithm whenever a new monster is born or received.

#### Schema

A **schema** describes the correct structure for a *data file* -- you can think of it as a set of rules that the data file must follow in order for it to be valid. Each *Entity type* has its own schema -- eg, there is a schema for Monsters, another for Items, and so on -- that all data files for that type of Entity must adhere to. All schemas are described in the [Data Specifications](#) section of the Guidebook.

## Data Syntax

#### Collection

A **collection** is a category of data

#### Array

One of the two types of *collection*. A data type characterized by a list of *elements*. In practice, these elements are generally all the same type, although they don't have to be.

#### Dictionary

Another

#### Argument

#### String

#### Integer

### Game Internals

#### Game object

#### Scene

#### Singleton
