# Usage

## Basic Parsing

If you want to create your own internal parsing framework for CST files, you can use the ``get_entry()`` function.

```text
1 ^The quick brown fox jumps over the lazy dog.^
```

```rust
use cst::get_entry;
use std::fs::File;
use std::io::prelude::*;
use std::path::Path;

fn main() {
    let path = Path::new("example.cst");
    let display = path.display();
    let mut file = match File::open(&path) {
        Err(why) => panic!("couldn't open {}: {}", display, why),
        Ok(file) => file,
    };
    let mut s = String::new();
    match file.read_to_string(&mut s) {
        Err(why) => panic!("couldn't read {}: {}", display, why),
        Ok(_) => println!("{}", get_entry(s, 1)),
    }
}
```

A working example that parses multiple entries at once can be found in the [examples](./examples) directory.

<!-- 
## In Production

Based on FreeSO's public APIs, the UIText class provides a basic production infrastructure to be used in production. Starting with 0.1.400, the UIText class provides methods that directory map to these directories relative to the application's.

```csharp
#r "nuget:CSTNet,1.0.300"
using CSTNet;

var english = new UIText(); // UIText assumes English
var swedish = new UIText("swedish");
var engExample = english.GetText(152, 1); // english.dir/_154_miscstrings.cst
var sweExample = swedish.GetText(152, 1); // swedish.dir/_154_miscstrings.cst

Console.WriteLine(engExample);
Console.WriteLine(sweExample);
```

In The Sims Online, it was required translation were prefixed with numbers enclosed in underscores, known as the ID. The IDs were used to locate the right file without having to remember it's name. Meanwhile, each translation was split into their respective ``uitext/<languae>.dir`` directories:

- ``uitext/english.dir/_154_miscstrings.cst``
- ``uitext/swedish.dir/_154_miscstrings.cst``

Note that that ``UIText`` class uses the above mentioned ``CST.Parse()`` method to parse the files.

### Changing base directories

If you want to change the base directory, you can. Though, this is still a work in progress and not recommended.

```csharp
#r "nuget:CSTNet,1.0.300"
using CSTNet;

var example = new UIText()
{
    BasePath = new[] { "gamedata", "uitext" },
};
```
 -->