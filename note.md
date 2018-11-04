**Note: Raw md file using `\|` inside regex by to escape | inside md cell table. Regex to find these regexs <code>`/[^`]*?\|[^`]*?/`</code>**

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

>Regex repetition is greedy, match as much as possible before giving control to the next regex 

>Speed = efficient matching + less backtracking
> * `/.+/` is faster than `/.*/`  
> * `/.{5}/` and `/.{3,7}/` are even faster
> * narrow the scope: `/.+/` => `/[A-Za-z]+/`
> * clearer starting and ending point: `/<.+>/` => `/<[^>]+>/` 

## Grouping
>Cannot be use inside character set `[]`

Metacharacter|Name|Meaning
---|---|---
`()`||Content inside as a group (may have many characters)
`\|`|alternation, OR|Match previous or next expression

  
Example|Match|Not match
---|---|---
`/(abc)+/`|**"abc" "abcabc" "abcabcabc"**
`/(in)?complete/`|**"complete" "incomplete"**|"ncomplete" "icomplete"
`/apple(s)?/`| same as `/apples?/` but may look clearer
`/(apple\|banana)/`|**"apple" "banana"**
`/(apple (juice\|cake))/`|**"apple juice" "apple cake"**|"apple " "cake" "juice"
`/(peanut\|peanutbutter)/`|**only "peanut" in "peanutbutter", the first part in regex**
`/peanut(butter)?/`|**"peanut" "peatnutbutter "**
`/(\w+\|FY\d{4})_report\.xls/`|use the first part `/\w+/` to match **"FY2003_report" and "xls" of "FY2003_report.xls"**, the second part `/FY\d{4})_report\.xls/` is never used

>Simplest expression first  
>`/\w+_\d{2,4}|\d{4}_\d{2}_\w+|export\d{2}/` => `/export\d{2}|\d{4}_\d{2}_\w+|\w+_\d{2,4}/` 

## Anchors

>Reference a **position**, has **zero-length** and **not a character**

Metacharacter|Name|Meaning
---|---|---
`^`||Start of string/line
`$`||End of string/line
`\A`||Start of string, never end of line
`\Z`||End of string, never end of line
`\b`|Word Boundary| start/end of word
`\B`|Not Word Boundary| 

>Word boundary:
>* Before and after the first and the last word character in the string (**Word characters: `[A-Za-z0-9_]`**)  
>* Between a word character and a non-word character
>* Space is not a word boundary

>* Single-line mode  
>   `^ $ \A \Z` do not match at line breaks
>* Multiline mode  
>  `^ $` match the start and the end of lines
>   `\A \Z` do not match at line breaks


Example|Match|Not match
---|---|---
`/^\w+@\w+\.[a-z]{3}$/`|**"nobody@nowhere.com" "abc@def.net"**|"nobody@nowhere.ca"
`/^[\t ]+/`|all heading whitespaces
`/[\t ]+$/`|all trailing whitespaces
`/^[a-z ]+/`|**only "apple" in "apple\norange"**
`/[a-z ]+$/`|**only "orange" in "apple\norange"**
`/^[a-z ]+/m`|**"apple\norange"**
`/[a-z ]+$/m`|**"apple\norange"**
`/^[a-z ]+$/m`|**"apple\norange"**
`/\b\w+\b/`|**"abc_123", "This" "is" "a" "boundary" in "This is a boundary.", "top" "notch" in "top-notch"**
`/\B\w+\B/`|**"hi" "oundar" in "This is a boundary."**
`/apples\band\boranges/`||"apples and oranges"
`/apples\b \band\b \boranges/`|"apples and oranges"
`/\b[\w']+\b/`|**"a" "summer's" "day" in "a summer's day"**
`/\b[\w']+?\b/`|**"a" "summer" "'" "s" "day" in "a summer's day"** there are boundary after "summer" and before "s"

>Find word ending with "s": `/\b\w+s\b/` is faster than `/\w+s/` 

## Capturing Groups and Backreferences

Metacharacter|Name|Meaning
---|---|---
`\1` through `\9`||Backreference for position 1 to 9. Some regex engines support `\1` to `\99` and use `$` instead of `\`
`?:`||Specify a non-capturing group, place at the beginning of group expression `/(?:regex)/`, `?` = "give this group a different meaning" and `:` = "non-capturing group"

>Group expression `()` are automatically captured or stored the matched position for later use by default  
>Example: `/a(p{2}l)e/` matches **"apple"** and store position of **"ppl"**  
>Backreferences allow access to these captured data
>* can be used in the same regex as the group
>* can be accessed after the match is complete
>* cannot be used inside character classes

Example|Match|Not match
---|---|---
`/(apples) to \1/`|**"apples to apples"**|"apples to \1"
`/(ab)(cd)(ef)\3\2\1/`|**"abcdefefcdab"**|"abcdedabcdef"
`/<(i\|em)>.+?</\1>/`|**"\<i>Hello\</i>" "\<em>Hello\</em>"**|"\<i>Hello\</em>" "\<em>Hello\</i>"
`/\b([A-Z][a-z]+)\b\s\b\1\w+\b/`|**"John Johnson" "Evan Evanson"**
`/\b(\w+)\s+\1\b/`|find duplicate words, **"is is"** in **"This is is an apple"**
`/(A?)B/`|**"AB" and captures "A" or "B" and captures "", element is optional but the group is not so it captures something**
`/(A?)B\1/`|**"ABA" and "B"**
`/(A?)B\1C/`|**"ABAC" and "BC"**
`/(A)?B/`|**"AB" and captures "A" or "B" and captures nothing because element is not optional, the whole group `(A)` is optional**
`/(A)?B\1/`|**"ABA"**|"B" (Except in JavaScript)
`/(A) and (B) to \1/`|**"A and B to A"**
`/(?:A) and (B) to \1/`|**"A and B to B"**|"A and B to A"


>Find and Replace **Name** with **Last Name,First Name** in file **us_presidents.txt**, the result is in file **us_presidents_find_replace_done.txt**
>1. Match the **Number, Name, Start** by `/^\d{1,2},[\w\.\s]+? [\w]+?,\d{4}/`
>2. Group **Number, Last Name, First Name, Start** by `/^(\d{1,2}),([\w\.\s]+?) ([\w]+?),(\d{4})/`
>3. Replace by backreferences `/$1,$3,$2,$4/`

## Assertions

>Position, zero-width, return true of false wheter it makes a match, but it does not actually match any characters   
>**Lookahead can be complex, but lookbehind should be simple**
>1. Look forward or backward
>2. Match string using multiple expressions
>3. Reject expressions with lookbehind assertion
>4. Find the last occurrence
>5. Find the position only for inserting with combination of lookahead and lookbehind assertions

Metacharacter|Name|Meaning
---|---|---
`?=`||Positive lookahead assertion, place at the beginning of group expression `/(?=regex)/`
`?!`||Negative lookahead assertion, place at the beginning of group expression `/(?!regex)/`
`?<=`||Positive lookbehind assertion, place at the beginning of group expression `/(?<=regex)/`
`?<!`||Negative lookbehind assertion, place at the beginning of group expression `/(?<!regex)/`

>Lookbehind assertion accept fixed length, simple regex such as literal text, character classes, fixed length alternation (`/(?<=cat|rat|fat)/` not `/(?<=cat|fish|width)/`) but not repetition or optional expressions

Example|Match|Not match
---|---|---
`/(?=seashore)sea/`|**"sea" in "seashore"** find the assertion before match the word, so it travel the word "sea" 2 times|"sea" in "seaside"
`/sea(?=shore)/`|**"sea" in "seashore"** match the word before checking the assertion|"sea" in "seaside"
`/\b[\w']+\b,/`|all words followed with a comma
`/\b[\w']+\b(?=,)/`|all words followed with a comma but not select the comma
`/\d{3}-\d{3}-\d{4}/`|**"555-302-4321" "555-789-6798"**
`/^[0-5\-]+$/`|any string contains 0 to 5 and "-" 
`/(?=^[0-5\-]+$)\d{3}-\d{3}-\d{4}/`|any US phone number contains only 0 to 5 digits **"555-302-4321" "555-302-1234"**, 2 regexs running in the same string
`/(?=^[0-5\-]+$)(?=.*1234)\d{3}-\d{3}-\d{4}/`|any US phone number contains only 0 to 5 digits and ending with "1234" **"555-302-1234"**, 3 regexs running in the same string
`/\b(?=\w*tr)[\w']+\b(?=,)/`|all words contain "tr" and followed with a comma but not select the comma
`^(?=.*\d).{8,15}$`|a string with length from 8 to 15 characters, contains a digit
`^(?=.*\d)(?=.*[A-Z]).{8,15}$`|a string with length from 8 to 15 characters, contains a digit and a capitalized character
`/(?!seashore)sea/`|any string start with **"sea"** but not "seashore"
`/sea(?!shore)/`|any string start with **"sea"** but not "seashore"
`/online(?! training)/`||does not match "online training"
`//online(?!.*training)`||does not match "online anything training"
`/(?=^[0-5\-]+$)(?!.*1234)\d{3}-\d{3}-\d{4}/`|any US phone number contains only 0 to 5 digits and not ending with "1234" **"555-302-1235"**, 3 regexs running in the same string
`/\b[\w']+\b(?![,\.]])/`|all words not followed with a comma or period
`/\blastWord\b(?!.*\blastWord\b)/`|find the last occurent of **"lastWord"**
`/(\blastWord\b)(?!.*\1)/`|find the last occurent of **"lastWord"**
`/(?<=base)ball/`|**"ball"** in **"baseball"**
`/ball(?<=baseball)/`|**"ball"** in **"baseball"**, rarely use this way
`/(?<!base)ball/`|**"ball"** in **"anythingball"**|"baseball"
`/(?<=Ben)(jamin\|ny)/`|**"jamin", "ny"** in **"Benjamin Benny"**
`/(?<![$\d])\d+\.\d\d/`|**"78.99"**|"$78.99" "7.89"
`/(?<![$\d])(?=\d+\.\d\d)/`|**zero-width is matched, the regex pointer is in front of "7" for "78.99"**|
`/(?<=\d)(\d\d\d)+(?!\d)/`|sets of 3 digits that have a number in front of them and do not have a number after them, **"345678" in "12345678.9" or  "345" in "12345"**
`/(?<=\d)(?=(\d\d\d)+(?!\d))/`|insert the pointer in front of sets of 3 digits that have a number in front of them and do not have a number after them 

## Unicode

>Use variable byte size to encode characters 

Metacharacter|Name|Meaning
---|---|---
`\u`||follow by 4 hex number range from 0000 to FFFF, support by Java, JavaScript, .NET, Python, Ruby
`\x`|| follow by 4 hex number range from 0000 to FFFF, support by Perl and PHP

Example|Match|Not match
---|---|---
`/caf\u00E9/`|**"café"**
`/caf\u0065\u0301/`|**"café"** `\u0065` is the character **"e"**

## Useful Regex

Possible Target| Description | Regex | Match Examples
---|---|---|---
Names|FirstName MiddleName LastName|`/^([A-Z][A-Za-z.'\-]+) (([A-Z][A-Za-z.'\-]+) )?([A-Z][A-Za-z.'\-]+)$/`|"George Washington", "Albert Einstein", "Anna K. Behrensmeyer", "Blaise Pascal", "Caroline Herschel", "Cecilia Payne-Gaposchkin", "Chien-Shiung Wu", "Dorothy Hodgkin", "Edmond Halley", "Edwin Powell Hubble", "Elizabeth Blackburn", "Enrico Fermi", "Erwin Schroedinger", "Flossie Wong-Staal", "Frieda Robscheit"-Robbins", "Geraldine Seydoux", "Gertrude B. Elion", "Ingrid Daubechies", "Jacqueline K. Barton", "Jane Goodall", "Jocelyn Bell Burnell", "Johannes Kepler", "Lene Vestergaard Hau", "Lise Meitner", "Lord Kelvin", "Maria Mitchell", "Marie Curie", "Max Born", "Max Planck", "Melissa Franklin", "Michael Faraday", "Mildred S. Dresselhaus", "Nicolaus Copernicus", "Niels Bohr", "Patricia S. Goldman-Rakic", "Patty Jo Watson", "Polly Matzinger", "Richard Phillips Feynman", "Rita Levi-Montalcini", "Rosalind Franklin", "Ruzena Bajcsy", "Sarah Boysen", "Shannon W. Lucid", "Shirley Ann Jackson", "Sir Ernest Rutherford", "Sir Isaac Newton", "Stephen Hawking", "Werner Karl Heisenberg", "Wilhelm Conrad Roentgen", "Wolfgang Ernst Pauli"
US Postal Codes|Five digits; Five digits, dash, four digits|`/^\d{5}(-\d{4})?$/`|"21342", "32145-3247"
Canada Postal Codes|"A9A 9A9", where A is a character from A-Z and 9 is a digit from 0-9|`/^[A-Z]\d[A-Z] \d[A-Z]\d$/`|"V5G 3Y2", "B3Z 2W4"
UK Postal Codes|"A9 9AA"; "A99 9AA"; "AA9 9AA"; "AA99 9AA"; "A9A 9AA"; "AA9A" "9AA"|`/^[A-Z](\d[A-Z]?\|[A-Z0-9]\d\|[A-Z]\d[A-Z0-9]) \d[A-Z][A-Z]$/` or `/^([A-Z]{1,2}\d{1,2}|[A-Z]{1,2}\d[A-Z]) \d[A-Z]{2}$/`|"A9 9AA"; "A99 9AA"; "AA9 9AA"; "AA99 9AA"; "A9A 9AA"; "AA9A" "9AA"
Email|"someone@somewhere.com"|`/^[\w.%+\-]+@[\w.\-]+\.\[A-Za-z]{2,6}$/`|"nvdhau@gmail.com", "abc@hcmiu.edu.vn"
URL|"http://www.nowhere.com"; "http://nowhere.com"; "http://blog.nowhere.com"; "https://nowhere.com"; "http://nowhere.com/products.html"; "http://nowhere.com/imgs/img-1.png"; "http://nowhere.com/product/3378"; "http://nowhere.com/products.php?id=78"; "http://nowhere.com/products.php?id=78&color=black"; "http://nowhere.com#about"; "http://168.192.255.186"|`/^(?:http\|https):\/\/[\w.\-]+(?:\.[\w+\-]+)+[\w\-.,@?^=%&:;/~\\+#]+$/`
Decimal Numbers|"5.1"; "312.32142"; "0.1234"; ".321"; "67"|`/^(\d*\.\d+\|\d+)$/`|
Currency|"$43"; "$321.00"; "$0.32"; "$.50"; "£321"|`/^(\$\|\u00A3)(\d*\.\d{2}|\d+)$/`
IP Address|"0.0.0.0" to "255.255.255.255" and can have leading 0 "067.032.143.089"|`/^(25[0-5]\|2[0-4][0-9]\|[01]?[0-9][0-9]?)\.(25[0-5]\|2[0-4][0-9]\|[01]?[0-9][0-9]?)\.(25[0-5]\|2[0-4][0-9]\|[01]?[0-9][0-9]?)\.(25[0-5]\|2[0-4][0-9]\|[01]?[0-9][0-9]?)$/`
Dates|"2000-11-15"; "2000-6-9"; "2000-06-09"; "2000/6/9"|`/^(19[5-9][0-9]\|20[0-4][0-9]\|2050)[-\/](0?[0-9]\|1[0-2])[-\/](3[01]\|[12][0-9]\|0?[1-9])$/`| from 1950 to 2050
Times|"2:34"; "2:34am"; "2:34PM"; "02:34"|`/^(0?[1-9]\|1[0-2]):([0-5][0-9])([aApP][mM])?$/`
Times|"14:34"; "14:34:56"; "14:56 EST"; "14:34 GMT -5"|`/^([0-1]?[0-9]\|2[0-3]):([0-5][0-9])(:([0-5][0-9]))?( ([A-Z]{3}|GMT [-+]([0-9]\|1[02])))?$/`
HTML tags|"<tag>texts</tag>"; "<tag id="id1" class="class1">texts</tag>"; "<tag />"| `/^<(?:([A-Za-z][A-Za-z0-9]*)\b[^>]*>(?:.*?)</\1>\|[A-Za-z][A-Za-z0-9]*\b[^>]/>)$/`
Passwords|any character except space; characters length in range 8-15; must include 1 uppercase, 1 lowercase, 1 digit and 1 symbol|`/^(?=.*[~!@#$%^&*()\-+=\|\\{}[\]:;<>?/])(?=.*\d)(?=.*[A-Z])(?=.*[a-z])\S{8,15}$/`
 