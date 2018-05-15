---
layout: post
title: "Ruby Regex Tips (Special global variables)"
date: 2018-05-15
categories: Ruby
---

## Special global variables

Pattern matching sets some global variables :

- `$~` is equivalent to ::last_match;
- `$&` contains the complete matched text;
- ``$``` contains string before match;
- `$'` contains string after match;
- `$1, $2` and so on contain text matching first, second, etc capture group;
- `$+` contains last capture group.

Example:

```ruby
m = /s(\w{2}).*(c)/.match('haystack') #=> #<MatchData "stac" 1:"ta" 2:"c">
$~                                    #=> #<MatchData "stac" 1:"ta" 2:"c">
Regexp.last_match                     #=> #<MatchData "stac" 1:"ta" 2:"c">

$&      #=> "stac"
        # same as m[0]
$`      #=> "hay"
        # same as m.pre_match
$'      #=> "k"
        # same as m.post_match
$1      #=> "ta"
        # same as m[1]
$2      #=> "c"
        # same as m[2]
$3      #=> nil
        # no third group in pattern
$+      #=> "c"
        # same as m[-1]
```

These global variables are thread-local and method-local variables.
