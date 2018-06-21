# Item Definitions

In Menagerie, an **Item** is anything that can be stored in the player's inventory and does *not* align to the tile grid when placed in the garden. There are several subcategories of Item, each with its own required properties in addition to the standard required properties for all Items.

## Item

The following properties are required for all Items. A generic `ITEM` subtype does exist, though it's intended for testing and experimentation, and really isn't recommended for use in released mods.

### Properties

| property | description |
|---|---|
| `id` | An identifier for the item. Strings in snake_case are standard, eg. `fluffy_tuft`. ***Must*** match the name used for the item's directory and datafile, eg. `data/items/fluffy_tuft/fluffy_tuft.data`.
| `name` | The item's name |

### Example

`/data/items/fluffy_tuft/fluffy_tuft.data`:

```
{
  "id": "fluffy_tuft",
  "name": "Fluffy Tuft",
  "icon": "!fluffy_tuft.png",
  "description": "Soft, springy fur from a Pufig's fluffy tail. It's rumored that stuffing a pillow with it will bring good dreams.",
  "value": 45,
  "subtype": "items.subtypes.resource"
}

```

### Assets

#### Icon

Represents the item in menus, including the inventory and store screens. Should be **16 by 16 pixels or less**. If it's larger than 16x16, it will be resized (poorly) in-game.

#### Sprite(s)

Represents the item when it's physically present in the garden. *By default, an item's **icon** is used as its sprite.*

For most items, a single static sprite will do, but if your item uses multiple sprites, their can be specified in the data file.