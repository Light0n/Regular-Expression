# Regular Expressions

>Regex is eager, greedy

## Regex mode
Type|Example| Text | Match
---|---|---|---
**Standard**         | `/abc/`  | "abc abc ABC" | "**abc** abc ABC"
**Global**           | `/abc/g` | "abc abc ABC" | "**abc** **abc** ABC" 
**Case-insensitive** | `/abc/i` | "AbC" | "**AbC**"
**Multiline**        | `/abc/m` |
**Dot-matches-all**  | `/a.c/s` | "a\nc"| "**a\nc**"

---

## Metacharacters
`\ . * + - {} [] ^ $ | ? () : ! =`  
They can have more than one meanings

---

## Single Character
Metacharacter|Name|Meaning
---|---|---
`.`|Wildcard|any character except newline
`\`|Escaping|escape the next character **only for metacharacters**

Example|Match|Not match
---|---|---
`/h.t/`|**"hat" "hot" "h\tt" "h t" "h$t"**|"h\nt"
`/9.00/`|**"9.00" "9500" "9a00" "9:00"**
`/9\.00/`|**"9.00"**|"9500" "9a00" "9:00"

### Special characters
Name|Code
---|---
Tabs| `\t`
Line returns| `\r`,`\n`, `\r\n`
Non-printable characters| bell (`\a`), escape (`\e`), form feed (`\f`), vertical tab(`\v`)
ASCII or ANSI codes| `\xA9` 

---

## Character Set
Metacharacter|Name|Meaning
---|---|---
`[]`|Character Set|match **one** character defined between character set
`-`||range of characters **only** when inside `[]`
`^`||negate a character set when it is the **first** character in side `[]`
`\d`||Digit or `[0-9]`
`\w`||Word character or `[a-zA-Z0-9_]`
`\s`||Whitespace or `[ \t\r\n]`
`\D`||Not digit or `[^0-9]`
`\W`||Not word character or `[^a-zA-Z0-9_]`
`\S`||Not whitespace or `[ \t\r\n]`
>These metacharacters `] - ^ \` need escaping inside `[]`

Example|Match|Not match
---|---|---
`/[ueoai]/`|any one vowel|
`/gr[ea]y/`|**"grey" "gray"**| "greay"
`/gr[ea]t/`|**"gret" "grat"**| "great"
`/gr[ea][ae]t/`|**"greet" "graat" "great" "graet"**|
`[0-9]`|a number from 0 to 9
`[A-Za-z]`| a normal or capitalized character 
`[50-99]`|a number from 0 to 9| any number from 50 to 99
`/[^ueoai]/`|any one is not a vowel|
`/see[^mn]/`|**"see#" "see&" "sees" "see "**| "seem" "seen" "see"
`/var[[(][0-9][\])]/`|**"var(0)" "var[1]" "var(9]" "var[5)"**
`/file[0\-\\_]1/`|**"file-1" "file_1" "file01" "file\1"**
`/\d\d\d\d/`|**"1989" "7865"**|"187a" "1sd9"
`/\w\w\w/`|**"___" "a9c" "1_a" "Ax3"**| "a-9"
`/\w\s\w\w/`|**"9 ds" "A 98" "7\t89" "_\nA9"**|
`/\w\-/`|any word character or hyphen
`/\d\s/`|any digit or whitespace character
`/^\d/`|`/\D/` or `/^0-9/` any non-digit character
`/[^\d\s]/`|**NOT** digit **OR** whitespace character **"a" "b"  "Z" "-" "_"**|"1" " " "\t" "\n"
`/[\D\S]/`| **EITHER NOT** digit (can be space or word) **OR NOT** whitespace (can be digit or word) character 

---
## Repetition

>Regex repetition is greedy, match as much as possible before giving control to the next regex 

>Speed = efficient matching + less backtracking
> * `/.+/` is faster than `/.*/`  
> * `/.{5}/` and `/.{3,7}/` are even faster
> * narrow the scope: `/.+/` => `/[A-Za-z]+/`
> * clearer starting and ending point: `/<.+>/` => `/<[^>]+>/` 

Metacharacter|Name|Meaning
---|---|---
`*`||Preceding item >= 0
`+`||Preceding item >= 1
`?`|option|Preceding item 0 or 1
`{}`|quantified repetition|Preceding item is repeated with specific times `{min,max}` **min>=0 is mandatory, max>0 is optional**  
`?`|lazy|Make preceding quantifier lazy: `*?`, `+?`, `??`, `{min,max}?`

Example|Match|Not match
---|---|---
`/apps*/`|**"app" "apps" "appssssssss"**|
`/apps+/`|**"apps" "appssssssss"**|"app"
`/apps?/`|**"app" "apps"**|"appsssssss"
`/\d\d\d\d*/`|three digits or more
`/\d\d\d+/`|three digits or more
`/\d{4,8}/`|number has 4 to 8 digits
`/\d{4,}/`|number has at least 4 digits or more
`/\d{4}/`|number has 4 digits
`/\d{0,}/`| or `/\d*/`
`/\d{1,}/`| or `/\d+/`
`/\d{3}-\d{3}-\d{4}/`|**"786-787-3378"**
`/.+\.jpg/`|**"image.jpg" "filename.jpg"**
`/.*[0-9]+/`|**"Page 789"** but the first part of regex `/.*/` matches `"Page 78"` and the second part `/[0-9]+/` matches `"9"`
`/.*?[0-9]+/`|**"Page 789"** but the first part of regex `/.*/` matches `"Page "` and the second part `/[0-9]+/` matches `"789"`
`/.*?[0-9]*?//`|match nothing because of two parts of regex are lazy
`/'.+', '.+'/`|**"'Abc', 'Xyz', 'Inc.com'"** the first `/'.+', /` matches `"'Abc', 'Xyz', "` and the second part `/'.+'/` matches `"'Inc.com'"`  
`/'.+?', '.+?'/`|**"'Abc', 'Xyz'"** the first `/'.+?', /` matches `"'Abc', "` and the second part `/'.+?'/` matches `"'Xyz'"`|", 'Inc.com'"
`/\d+\w+\d+/`|**"07_FY_08_report_99"**, `/\w+/` matches `"_FY_08_report_"`
`/\d+\w+?\d+/`|**"07_FY_08"**, `/\w+/` matches `"_FY_"`|"_report_99"
