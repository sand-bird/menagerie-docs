# Data Files

*Menagerie* was designed from the beginning to be easily moddable, so just about all of the game's content is data-driven -- that is, it's stored in **data files**, which can be added, removed, or changed without having to poke around in the actual code.

If you haven't yet, it might be a good idea to consult the [Terminology](#) section of this guide.

## File Format

All of *Menagerie*'s data files are in the venerable JSON format, which is simple, easy to learn, and (fairly) human-readable. There are, of course, a zillion explanations of JSON on the web (eg. [this one](https://developers.squarespace.com/what-is-json/)), but to ensure consistency with the rest of the terminology in this document, here's a brief example.

``` json
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
    "arrays have no keys, only values!",
  	456,
  	{
  	  "inside a dictionary inside an array": 42,
  	  "^%$#@special characters are a-ok!*&(;": "...except backslash \\",
  	  "789": "numerical keys need quotes too"
  	}
  ]
}
```

That's valid JSON. As long as your files look less screwy than that, you should be good.

A few things to note:

- The entire contents of a JSON file **must always** be enclosed within a pair of curly braces `{ }` at the top level, as shown in the example.
- Keys **must always** be enclosed within double quotes `" "`, even if they're numbers. (Numeric identifiers are strongly discouraged in *Menagerie* anyway, though.)
- Any whitespace outside of a string doesn't matter -- eg. `{"fruits":{"apple":10,"pear":40}}` is valid.
- Every element -- a value if it's an array or a key/value pair if it's a dictionary -- must be separated from its siblings by a comma.

!!! warning 
    ***Data files must be valid JSON*, or else they will fail to load.** The format is notoriously unforgiving of typos and small mistakes; if you're not completely confident that your data file is valid, you can always run it through one of the many JSON checking tools on the web.

Although they're in JSON format, data files in *Menagerie* have the `.data` extension. 

## Common Properties

Every data specification has

## Resources


