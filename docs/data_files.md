# Data Files

*Menagerie* was designed from the beginning to be easily moddable, so just about all of the game's content is data-driven -- that is, it's stored in [data files](/terminology/#data-file), which can be added, removed, or changed without having to poke around in the actual code.

If you haven't yet, it might be a good idea to consult the [Terminology](/terminology) section of this guide.

## File Format

All of *Menagerie*'s data files are in the venerable JSON format, which is simple, easy to learn, and (fairly) human-readable.

Although they're in JSON format, data files in *Menagerie* have the `.data` extension.

!!! warning 
    ***Data files must be valid JSON*, or else they will fail to load.** The format is notoriously unforgiving of typos and small mistakes; if you're not completely confident that your data file is valid, you can always run it through one of the many JSON checking tools on the web.

    A few things to note:

    - The entire contents of a JSON file **must always** be enclosed within a pair of curly braces (`{}`) at the top level.
    - Keys **must always** be enclosed within double quotes (`"`).
    - Any whitespace outside of a [string](/terminology/#string) doesn't matter -- eg. `{"fruits":{"apple":10,"pear":40}}` and `{ "fruits" : { "apple" : 10, "pear" : 40 } }` are both valid JSON.
    - Every element of a [collection](/terminology/#collection) -- a *value* if it's an array or a *property* if it's a dictionary -- must be separated from its siblings by a comma.

!!! note
    Save files for *Menagerie* are also in JSON format, although they have the `.save` extension. Yes, this makes it quite easy to modify your save -- and yes, this is by design. 😉

There are, of course, a zillion explanations of JSON on the web (eg. [this one](https://developers.squarespace.com/what-is-json/)), but to ensure consistency with the rest of the terminology in this document, I'll discuss it briefly here.

<!--
This is an example of valid JSON:

```json
{
  "key": "value",
  "types_of_values": {
    "string": "hello",
    "number": 789,
    "boolean": false,
    "array": [],
    "dictionary": {}
  },
  "types_of_keys": [ 
    "just string!"
  ],
  "dictionary": {
    "key to a numeric value": 123,
    "key to a boolean": true,
    "nested dictionary": { "hello": "world" }
  },
  "array": [
    "arrays don't have keys inside -- they are collections of values!",
  	456,
  	{
  	  "inside a dictionary inside an array": 42,
  	  "^%$#@special characters are a-ok!*&(;": "...except backslash \\",
  	  "789": "numerical keys need quotes too"
  	}
  ]
}
```
-->

### Keys and values

The basic building blocks of JSON are **keys** and **values**. *Keys* are always enclosed in double-quotes (`"`), and followed by a colon (`:`) and that key's *value*.

Technically, a key can be almost anything, as long as it's unique. However, it's usually a good idea to follow these common conventions when naming keys:

- Use only lowercase letters.
- Don't use spaces -- use underscores instead.
- Keep keys short and descriptive. Keys should almost always be nouns.
- Use only letters and underscores; no numbers or special characters.

Together, a key and its value are referred to as a **property**. The key *identifies* the property -- it must be unique so that a computer can find it in the data file without getting confused. The value *describes* that property, and can take one of five different forms, known as **data types**.

### Data types

#### String

A **string** is just programming jargon for a bit of text. Strings can be made up of a single word, many words, or no words at all -- as long as it is enclosed in double-quotes (`"`), it counts as a string:

```json
{ "string_value": "I am a string value!" }
```

!!! trivia
    As we already know, keys are also enclosed in double-quotes. If you were wondering whether that means keys are strings too... the answer is yes! Using strings for keys is one reason JSON is considered to be a human-friendly data format.

#### Number

There are actually two kinds of numbers

#### Dictionary

Also known as an *object*, a **dictionary** is a set of **keys** and **values**, wrapped in a pair of curly braces (`{}`). Sounds familiar, doesn't it? You guessed it -- a JSON file is *itself* a dictionary! This is exactly why JSON files must always start with `{` and end with `}`.


## Common Properties

Every data specification has

## Resources


