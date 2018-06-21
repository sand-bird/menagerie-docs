# Operators

The condition at the beginning of this page features a greater-than-or-equal-to operator, which is an example of a **comparison operator** (or **comparator** for short). Here's slightly more complex condition showing off some other kinds of operators:

```
{
  "and": [
    {
      "==": ["$time.day_of_week", $DAY.FRIDAY"]
    },
    { 
      "in": ["bubbling_drink", "$player.inventory"] 
    }
  ]
}
```

Are you ready for a good time? This condition returns true if it's Friday and the player has an item with the id `bubbling_drink` in their inventory. Here we see the comparator `"!="`, which stands for "not equals", the **relational operator** `"in"`, and the **boolean operator** `"and"`. Let's take these categories one at a time.


## Relational (and Comparison) Operators

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

The `"in"` operator is a little bit special, since it's the only relational operator in the list that isn't also a comparator. Instead, the `"in"` operator has a special requirement: its second argument must be a **collection** of values, either a dictionary or an array. 

(TODO: check if you can `in` a string)

## Boolean Operators

A condition is actually a **boolean value** -- something that is either `true` (when the condition is met), or `false` (when the condition is not met). We've seen that relational operators will return `true` or `false` depending on the relationship between the two values they're given, for example.

But what about conditions that depend on multiple relationships? Our last example was one such case: the player must have a `bubbling_drink` in their inventory, *and* it must be Friday. The key here is the `"and"` operator, which requires that *both those conditions be `true` at the same time*.

Along with its partner `"or"`, the `"and"` operator is known as a **boolean operator**, since it accepts *boolean values* -- in other words, *other conditions* -- as its arguments. Unlike relational operators, boolean operators can have an *unlimited number of arguments*. Here's how they work:

| operator | returns true if... |
| --- | --- |
| `"and"` | **all** of the arguments are `true` |
| `"or"` | **at least one** of the arguments is `true` |

!!! note 
    If you enjoy writing conditions, you might appreciate **boolean logic**, a whole subfield of mathematics that deals with `true` and `false` values, in which the simple `and` and `or` that we use here are the building blocks for many other, more complex operations. Or, if you prefer something a little less abstract, you might also enjoy *applied* boolean logic -- more colloquially known as computer engineering.

### Shortcut

Because the `"and"` operator is so important, it has a special shortcut. Remember how a condition must always be a **dictionary** with the condition's operator as its only key? If *Menagerie* finds an **array** instead, it will treat it as though it were the *argument array of an `"and"` condition*. The shortcut, then, is to omit the dictionary and the `"and"` operator, like so:

```
[
  {
    "==": ["$time.day_of_week", $DAY.FRIDAY"]
  },
  { 
    "in": ["bubbling_drink", "$player.inventory"] 
  }
]
```

This will be resolved the same way as the full `bubbling_drink` example from before.

!!! note
The programmer jargon for this sort of thing is "syntactic sugar", which refers to any alternate, more convenient syntax for some functionality.

### Thinking in boolean

Let's say we're creating an event, and we want it to happen on *Tuesdays and Thursdays*. We might first try something like this:

```
{
  "and": [
    {
      "==": ["$time.day_of_week", "$DAY.TUESDAY"]
    },
    {
      "==": ["$time.day_of_week", "$DAY.THURSDAY"]
    }
  ]
}
```

This is a perfectly-written condition, except for one little problem... it will never return `true`! To understand why, you have to learn to **think in boolean**: even though `and` and `or` are English words, they don't mean the same thing as their natural-language counterparts, so you must take care when "translating" from one to the other. 

For example, if you ask your friend, "do you want to go to the movies **or** the park," she'd probably *choose one* (or she might respond with "neither, how about the beach?"). But what if you asked a computer that only understood boolean `or`? You might still get a "neither," or `false`. But if the computer liked one of your suggestions, it wouldn't tell you which one -- it would just say "sure!" (`true`).

So, let's try and consider our example from a computer's perspective. In natural English, you want the event to be available on Tuesday **and** Thursday. But remember, the `"and"` operator returns `true` if **all of** the arguments are `true`. What are the arguments?

```
{ "==": ["$time.day_of_week", "$DAY.TUESDAY"] }
```
"Today is Tuesday."

```
{ "==": ["$time.day_of_week", "$DAY.THURSDAY"] }
```
"Today is Thursday."

See the problem? Yep -- that `"and"` operator is telling the game that for the condition to be `true`, today needs to be *both* Tuesday *and* Thursday!

To avoid pitfalls like these, it often helps to rephrase your intention to match the logical structure of conditions as closely as possible. Start from the simplest parts -- relations, like "today is Tuesday," -- and build up from there. When you phrase expectations this way, the right boolean operator comes naturally:

> The event should be available if today is Tuesday, *or* if today is Thursday.

!!! note
Like just about any other kind of programming, there's often more than one way to write a condition. Instead of using an `"=="` condition for each valid day, you could also list them out with an as an array, and use the `"in"` operator to check if the current day is a member of the list:

 ```
{
  "in": [
    "$time.day_of_week",
    [ "$DAY.TUESDAY", "$DAY.THURSDAY" ]
  ]
}
```

We'll see more examples of 
    

## Data Operators

Using the `"in"` operator
