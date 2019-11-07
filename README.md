# Any Swap

A plugin of `Sublime Text 3` that helps user to swap parameters, lines, and expressions from current cursor.

Behaves just like `Move-Element-Left/Right` in IntelliJ's IDEs, but more intelligently.
It is based on a general abstract-syntax-tree analyzer that enables user to swap complicated expressions recursively while maintaining a correct operator precedence.

## Usage

First of all, install package `AnySwap` via `Package Control` (not published yet) or clone this repo into your `Sublime Text 3/Packages/` folder. 

Bind command `any_swap` with parameter `forward` equals `true` or `false` to keys that you prefer. The command is set to `alt+[` and `alt+]` by default.
```
{ "keys": ["alt+["], "command": "any_swap", "args": {"forward": false} },
{ "keys": ["alt+]"], "command": "any_swap", "args": {"forward": true} },
```
Place your cursor (|) on the begin/end of a word/paren, then trigger the command.

## Features

**Parameters:**
```
func(a|, b)         => func(b, a|)         => func(|a, b)
func(int a|, int b) => func(int b, int a|) => func(|int a, int b)
```

**Lines:**
```
a|, b   =>  b, a|   =>  c, d
c, d        c, d        b, a|
```

**Math expressions:**
```
a| * b + c   => b * a| + c   => c + b * a|
(a| + b) * c => (b + a|) * c => c * (b + a)|
```

**Expressions with functions:**
```
func(a| + b, c) * d => func(b + a|, c) * d  => func(c, b + a|) * d   => d * func(c, b + a)|
```

**Expressions with nested functions:**
```
func(a().b(c| + d), f)  => func(a().b(d + c|), f)   => func(f, a().b(d + c)|)
```

**Logic expressions:**
```
a| and b or c   => b and a| or c    => c or b and a|
if a| and b 	=> if b and a|
```

**Array expressions:**
```
a[0|][1]        => a[1][0|]
a[b[c| + 1]][0] => a[b[1 + c|]][0] => a[0][b[1 + c]|]
```

**Statements:**
```
a = 1|; b = 2; 	=> b = 2; a = 1;|
return a|, b 	=> return b, a|
```

**Cross-line expressions:**
```
func(a|, => func(b,
    b)          a|)
```
