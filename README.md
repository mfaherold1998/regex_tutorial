# regex_tutorial

## Table of content
1. [Introduction](#Introduction)
2. [Special Characters](#Special-Characters)
3. [Non-printable characters](#non-printable.characters)
4. [Class Characteres](#class-characteres)

## Introduction

word => La palabra word en caso sensitivo

word /i => La palabra word en caso insensitivo

a => Match la primera ocurrencia de a

a /g => Match todas las ocurrencias de a

> Las engine de regex son caso sensitivo por defecto

## Special Characters

Meta caracteres:
- ^ => Inicio de la cadena
- $ => Fin de la cadena
- . => Cualquier caracter
- \ => Escape de caracteres
- | => Alternativa (or operator)
- ? => Cero o una ocurrencia (zero or one)
- * => Cero o mas ocurrencias (zero or more)
- + => Una o mas ocurrencias (one or more)
- () => Grupo de caracteres
- [] => coincide con cualquier caracter dentro
- {} => marca el inicio de una cuantificacion explicita

> Para ser usados deben escaparse con \

> \Q..\E => todos los caracteres en el medio son interpretados como literales. Por ejemplo: 
>
>*\Q\d+\E*
>
>se entiende como "\d+".

## Non-printable characters 

- \t => tab
- \n => salto de linea
- \r => retorno de carro
- \e => escape
- \f => form feed
- \a => bell
- \r\n => terminacion de lineas en windows
- \n => terminacion de lineas en linux

> \cA-\cZ => insert ASCII control characters (No en todos los lenguajes)
>
>\uFFFF or \x{FFFF =>insert unicode character 
>
>U+0041 = A (unicode => U+hexadecimal)
>
>\R => line break (match a CRLF pair)
>
>\r\R => \r toma el CR y \R el LF
>
>\0377 or \377 or \o{377} => octal escapes (\o match null)


(?:regex) => no crea backreferences en las regex

> Las backreferences permiten referirse a partes previamente capturadas en la string


> Los motores de expresiones devuelven la coincidencia mas a la izquierda incluso si despues hay una coincidencia mejor.

## Class Characteres

[abc] => match un caracter en la lista


[^abc] => match cualquier caracter que no este en la lista


[^] => match invisible line break characteres \r\n

> Dentro de [] los unicos metacaracteres permitidos son ] \ ^ -

[0-9]+ => match cualquier caracter dentro de la lista que aparecen una o varias veces

([0-9])\1+ => match el caracter encontrado con backreferences

[class - [subclass]] => restar los caracteres de la subclass a una class (Ejemplo: [0-9 -[0-6 - [0-3]]])

> The class substraction must be the last element in thecharacter class.
>
> No se pueden restar dos clases [class - [subclass1]-[subclass2]]
> Se debe reunir todo en la misma clase y restarlo una sola vez.

[^1234 - [3456]] => not (1234) menos 3456

[class && [intersect]] => interseccion de clases

[class && [ class && [ class ] ] ] => se puede

[^1234 && [3456]] => not (1234) and 3456

[1234 && [^3456]] => 1234 and not 3456


## Shorthands character classes

- \d => [0-9] digito
- \w => [A-Za-z0-9] word
- \s => [ \t\r\n\f] whitespace
- [\da-fA-F] => hexadecimal digit


## Negated shorthands

- \D => [^\d]
- \W => [^\w]
- \S => [^\s]
- [\D\S] => cualquier caracter que no sea digito ***o*** espacio (como todos los digitos son no espacios y todos los espacios son no digitos esto match any character)
- [^\d\s] => cualquier caracter que no sea digito ***y*** no sea espacio


> Perl 5.10 
>
>\h = [\t\p{Zs}] horizontal whitespace
>
>\v =[\n\ck\f\r\x85\x{2028}\x{2029}] vertical whitespace


> XML character classes (see: )

## Usos y mal usos del .

> The dot matches a single character, without caring what that character is. The only exception are line break characters.

=> [\s\S] to match any character.

=> (\s|\S) do not use, is slow; (.|\s) catastrophic backtraking as spaces and tabs can be matched by both . and \s.

>  JavaScript adds the Unicode line separator \u2028 and paragraph separator \u2029.

\N => match cualquier caracter que no sea un line break (igual que el .)

The problem with use . is that the regex also matches in cases where it should not match:

We want to match a date in mm/dd/yy format, but we want to leave the user the choice of date separators. The quick solution is \d\d.\d\d.\d\d. Trouble is: 02512703 is also considered a valid date by this regular expression. \d\d[- /.]\d\d[- /.]\d\d is a better solution.

This regex is still far from perfect. It matches 99/99/99 as a valid date. [01]\d[- /.][0-3]\d[- /.]\d\d is a step ahead, though it still matches 19/39/99. How perfect you want your regex to be depends on what you want to do with it. If you are validating user input, it has to be perfect.

Use Negated Character Classes Instead of the Dot:

".*" =>  any number of any character between the double quotes

The regex matches "string one" and "string two". Definitely not what we intended.

The proper regex is "[^"\r\n]*". If your flavor supports the shorthand \v to match any line break character, then "[^"\v]*" is an even better solution.
 

 ## Start and end of a string

 ^ => start of a string


 $ => end of a string


 ^\d+$ => una cadena unicamente de numeros


=> \A only ever matches at the start of the string. Likewise, \Z only ever matches at the end of the string. These two tokens never match at line breaks. 


\B matches at every position where \b does not. Effectively, \B matches at any position between two word characters as well as at any position between two non-word characters.


## Alternation with The Vertical Bar or Pipe Symbol


> The alternation operator has the lowest precedence of all regex operators.


\b(cat|dog)\b => match only words


## Optional Items


The question mark makes the preceding token in the regular expression optional. colou?r matches both colour and color. The question mark is called a quantifier.

Nov(ember)? => matches Nov and November

Feb(ruary)? 23(rd)? => matches February 23rd, February 23, Feb 23rd and Feb 23

colou{0,1}r => is the same as colou?r

> The question mark is a greedy metacharacter. You can make the question mark lazy (i.e. turn off the greediness) by putting a second question mark after the first.

## Limiting Repetition

{min,max} => where min is zero or a positive integer number indicating the minimum number of matches, and max is an integer equal to or greater than min indicating the maximum number of matches.

{0,1} => ? 

{0,} => *

{1,} => +

If the comma is present but max is omitted, the maximum number of matches is infinite
Omitting both the comma and max tells the engine to repeat the token exactly min times.

Make the plus lazy instead of greedy: putting a question mark after the plus in the regex.
(Lazy quantifiers are sometimes also called “ungreedy” or “reluctant”)

<[^>]+> => mejor opcion que <.+?> hacer lazy el + dado que con mas se hace backtraking y retrasa mas el calculo

## Grouping and capturing

Besides grouping part of a regular expression together, parentheses also create a numbered capturing group. It stores the part of the string matched by the part of the regular expression inside the parentheses. Set(Value)? matches Set or SetValue. In the first case, the first (and only) capturing group remains empty. In the second case, the first capturing group matches Value.

If you do not need the group to capture its match, you can optimize this regular expression into Set(?:Value)?.

## Backreferences

Backreferences match the same text as previously matched by a capturing group

<([A-Z][A-Z0-9]*)\b[^>]*>.*?</\1> => This regex contains only one pair of parentheses, which capture the string matched by [A-Z][A-Z0-9]*. This is the opening HTML tag. (Since HTML tags are case insensitive, this regex requires case insensitive matching.) The backreference \1 (backslash one) references the first capturing group. \1 matches the exact same text that was matched by the first capturing group. The / before it is a literal character. It is simply the forward slash in the closing HTML tag that we are trying to match.

([a-c])x\1x\1 => matches axaxa, bxbxb and cxcxc

There is a clear difference between ([abc]+) and ([abc])+. Though both successfully match cab, the first regex will put cab into the first backreference, while the second regex will only store b. That is because in the second regex, the plus caused the pair of parentheses to repeat three times. The first time, c was stored. The second time, a, and the third time b. Each time, the previous value was overwritten, so b remains.

When editing text, doubled words such as “the the” easily creep in. Using the regex \b(\w+)\s+\1\b in your text editor, you can easily find them. To delete the second word, simply type in \1 as the replacement text and click the Replace button.

> Parentheses and Backreferences Cannot Be Used Inside Character Classes

## Forward references

(\2two|(one))+ => matches oneonetwo

## Nested References

A nested reference is a backreference inside the capturing group that it references

## Named Capturing Groups and Backreferences

(?P<name>group) => named group

(?P=name) => backreferences of a named group

Example => <(?P<tag>[A-Z][A-Z0-9]*)\b[^>]*>.*?</(?P=tag)>

## Relative Backreferences

(a)(b)(c)\k<-1> => matches abcc

(a)(b)(c)\k<-3> => matches abca

>If the backreference is inside a capturing group, then you also need to count that capturing group’s opening parenthesis.

(a)(b)(c\k<-2>) => matches abcb

## Branch Reset Group

Dentro de un "Branch Reset Group", las alternativas se agrupan en un solo conjunto de capturas. Esto significa que si pattern1 captura algo, será almacenado en el grupo de captura 1, pero si pattern2 captura algo en la misma posición del patrón, también será almacenado en el grupo 1.

En una regex normal sin "Branch Reset Group", las alternativas (a)|(b) corresponderían a dos grupos de captura diferentes (1 para a y 2 para b). Con el "Branch Reset Group", las alternativas comparten los mismos números de grupo.

The syntax is **(?|regex)**

If you don’t use any alternation or capturing groups inside the branch reset group, then its special function doesn’t come into play.

(?|(a)|(b)|(c))\1 => matches aa, bb, or cc.

The alternatives in the branch reset group don’t need to have the same number of capturing groups. (?|abc|(d)(e)(f)|g(h)i) has three capturing groups. When this regex matches abc, all three groups are empty. When def is matched, $1 holds d, $2 holds e and $3 holds f. When ghi is matched, $1 holds h while the other two are empty.

You can have capturing groups before and after the branch reset group. Groups before the branch reset group are numbered as usual. Groups in the branch reset group are numbered continued from the groups before the branch reset group, which each alternative resetting the number. Groups after the branch reset group are numbered continued from the alternative with the most groups, even if that is not the last alternative. So (x)(?|abc|(d)(e)(f)|g(h)i)(y) defines five capturing groups. (x) is group 1, (d) and (h) are group 2, (e) is group 3, (f) is group 4, and (y) is group 5.

## Named Capturing Groups in Branch Reset Groups

(?'before'x)(?|abc|(?'left'd)(?'middle'e)(?'right'f)|g(?'left'h)i)(?'after'y)

## Day and Month with Accurate Number of Days

^(?:(0?[13578]|1[02])/(3[01]|[12][0-9]|0?[1-9]) # 31 days
 |  (0?[469]|11)/(30|[12][0-9]|0?[1-9])         # 30 days
 |  (0?2)/([12][0-9]|0?[1-9])                   # 29 days
 )$

 ^(?|(0?[13578]|1[02])/(3[01]|[12][0-9]|0?[1-9]) # 31 days
 |  (0?[469]|11)/(30|[12][0-9]|0?[1-9])         # 30 days
 |  (0?2)/([12][0-9]|0?[1-9])                   # 29 days
 )$

 The first version uses a non-capturing group (?:…) to group the alternatives. It has six separate capturing groups. $1 and $2 hold the month and the day for months with 31 days. $3 and $4 hold them for months with 30 days. $5 and $6 are only used for February.

 The second version uses a branch reset group (?|…) to group the alternatives and merge their capturing groups. The 4th character is the only difference between these two regexes. Now there are only two capturing groups. These are shared between the three alternatives. When a match is found $1 always holds the month and 2 always holds the day, regardless of the number of days in the month.

 ## Free-spacing mode

 > The mode is usually enabled by setting an option or flag outside the regex. With flavors that support mode modifiers, you can put (?x) the very start of the regex

In free-spacing mode, whitespace between regular expression tokens is ignored. Whitespace includes spaces, tabs, and line breaks. Note that only whitespace between tokens is ignored. a b c is the same as abc in free-spacing mode.

But \ d and \d are not the same

Likewise, grouping modifiers cannot be broken up. (?>atomic) is the same as (?> ato mic ) and as ( ?>ato mic).

They’re not the same as (? >atomic). The latter is a syntax error. The ?> grouping modifier is a single element in the regex syntax, and must stay together.

A character class is generally treated as a single token. [abc] is not the same as [ a b c ].

In free-spacing mode, you can use \  or [ ] to match a single space.

[ ^ a b c ] matches any of the characters ^, a, b, c or space

In free-spacing mode is that the # character starts a comment.

Many flavors also allow you to add comments to your regex without using free-spacing mode. The syntax is (?#comment) where “comment” can be whatever you want, as long as it does not contain a closing parenthesis. The regex engine ignores everything after the (?# until the first closing parenthesis.


## Unicode Regular expresions

A single Unicode code point => a single character

The dot matches any single Unicode code point: à can be encoded as two code points U+0061 (a) and U+0300 (grave accent). '.' applied to à will match a without the accent.

Unfortunately, à can also be encoded with the single Unicode code point U+00E0 (a with grave accent).

## Matching unicode point

\uFFFF => FFFF numero hexahesimal del code point

\u00E0 => à when encoded as U+00E0

> Otros lenguajes usan \x{}
>
> x{1234}{5678} will try to match code point U+1234 exactly 5678 times

> More in https://www.regular-expressions.info/unicode.html

## Mode modifiers

> https://www.regular-expressions.info/modifiers.html

## Atomic Grouping

> An atomic group is a group that, when the regex engine exits from it, automatically throws away all backtracking positions remembered by any tokens inside the group.
>
> Atomic groups are non-capturing
>
> The syntax is (?>group). Lookaround groups are also atomic.

The regular expression a(bc|b)c (capturing group) matches abcc and abc. The regex a(?>bc|b)c (atomic group) matches abcc but not abc.

## Optimization using atomic groups

\b(?>integer|insert|in)\b => to try to match integers

## Posesive Quantifiers

Possessive quantifiers are a way to prevent the regex engine from trying all permutations. This is primarily useful for performance reasons. You can also use possessive quantifiers to eliminate certain matches.

Like a greedy quantifier, a possessive quantifier repeats the token as many times as possible. Unlike a greedy quantifier, it does not give up matches as the engine backtracks. With a possessive quantifier, the deal is all or nothing.

You can make a quantifier possessive by placing an extra + after it.

'*' is greedy

*? is lazy

*+ is possessive

++, ?+ and {n,m}+ are all possessive as well.

> The main practical benefit of possessive quantifiers is to speed up your regular expression.

## Lookahead and Lookbehind

> Are zero-length assertions just like the start and end of line, and start and end of word anchors
> 
> The difference is that lookaround actually matches characters, but then gives up the match, returning only the result: match or no match.

## Negative lookahead

Match something not followed by something else:

q(?!u) => q not followed by u

## Positive lookahead

q(?=u) => matches a q that is followed by a u, without making the u part of the match

You can use any regular expression inside the lookahead (but not lookbehind)

The lookahead itself is not a capturing group

If you want to store the match of the regex inside a lookahead, you have to put capturing parentheses around the regex inside the lookahead, like this: (?=(regex)).

## Positive and Negative lookbehind

(?<!a)b => matches a “b” that is not preceded by an “a”

(?<=a)b => matches a “b” that is preceded by an “a”

 \b\w+(?<!s)\b => a word non ending with an s (\b\w*[^s\W]\b)

Most regex flavors do not allow you to use just any regex inside a lookbehind, because they cannot apply a regular expression backwards. You cannot use quantifiers or backreferences. You can use alternation, but only if all alternatives have the same length.

## Lookaround is atomic

The fact that lookaround is **zero-length** automatically makes it atomic

## Double requirement regex

(?=\b\w{6}\b)\b\w*cat\w*\b => match a word of 6 letter containing the substring cat. 

Optimizing:

(?=\b\w{6}\b)\w*cat\w* => remove \b\b en la segunda regex dado que son zero-lenght y el primer lookahead garantiza los limites de palabras.

(?=\b\w{6}\b)\w{0,3}cat\w* => porque sabemos que antes de cat solo pueden venir 3 letras

\b(?=\w{6}\b)\w{0,3}cat\w* => since it is zero-length itself, there’s no need to put it inside the lookahead

\b(?=\w{6,12}\b)\w{0,9}(cat|dog|mouse)\w* => any word between 6 and 12 letters long containing either “cat”, “dog” or “mouse”

## Keep The Text Matched So Far out of The Overall Regex Match

\K keeps the text matched so far out of the overall regex match. h\Kd matches only the second d in adhd.

You can use \K pretty much anywhere in any regular expression. You should only avoid using it inside lookbehind.

You can have as many instances of \K in your regex as you like

(ab\Kc|d\Ke)f => matches cf when preceded by ab. It also matches ef when preceded by d.

\K does not affect capturing groups. When (ab\Kc|d\Ke)f matches cf, the capturing group captures abc as if the \K weren’t there. When the regex matches ef, the capturing group stores de.

\K does not provide a way to negate anything

## Conditionals if-else

(?ifthen|else) => syntax

For the if part, you can use the lookahead and lookbehind constructs. Using positive lookahead, the syntax becomes (?(?=regex)then|else). Because the lookahead has its own parentheses, the if and then parts are clearly separated.

(?(?=condition)(then1|then2|then3)|(else1|else2|else3)) => to use alternation in then and else part

(a)?b(?(1)c|d) => match bd and abc

(?<test>a)?b(?(test)c|d) => same but using named capturing

 **Example: extract email headers**

 ^((From|To)|Subject): ((?(2)\w+@\w+\.[a-z]+|.+))

 references: https://www.regular-expressions.info/conditional.html

 ## Balancing groups

.Net special feature

(?<capture-subtract>regex) or (?'capture-subtract'regex) => basic syntax of a balancing group

(?<-subtract>regex) or (?'-subtract'regex) is the syntax for a non-capturing balancing group.

The name “subtract” must be the name of another group in the regex

When the regex engine enters the balancing group, it subtracts one match from the group “subtract”. If the group “subtract” did not match yet, or if all its matches were already subtracted, then the balancing group fails to match.

Example:

Let’s apply the regex **(?'open'o)+(?'between-open'c)+** to the string **ooccc**.

(?'open'o) matches the first o and stores that as the first capture of the group “open”. The quantifier + repeats the group. (?'open'o) matches the second o and stores that as the second capture. Repeating again, (?'open'o) fails to match the first c. But the + is satisfied with two repetitions.

The regex engine advances to (?'between-open'c). Before the engine can enter this balancing group, it must check whether the subtracted group “open” has captured something. It has captured the second o. The engine enters the group, subtracting the most recent capture from “open”. This leaves the group “open” with the first o as its only capture. Now inside the balancing group, c matches c. The engine exits the balancing group. The group “between” captures the text between the match subtracted from “open” (the second o) and the c just matched by the balancing group. This is an empty string but it is captured anyway.

The balancing group too has + as its quantifier. The engine again finds that the subtracted group “open” captured something, namely the first o. The regex enters the balancing group, leaving the group “open” without any matches. c matches the second c in the string. The group “between” captures oc which is the text between the match subtracted from “open” (the first o) and the second c just matched by the balancing group.

The balancing group is repeated again. But this time, the regex engine finds that the group “open” has no matches left. The balancing group fails to match. The group “between” is unaffected, retaining its most recent capture.

The + is satisfied with two iterations. The engine has reached the end of the regex. It returns oocc as the overall match. Match.Groups['open'].Success will return false, because all the captures of that group were subtracted. Match.Groups['between'].Value returns "oc".

(?(open)(?!)) => a test to verify that open has no captures left

^(?'open'o)+(?'-open'c)+(?(open)(?!))$ fails to match ooc.

references: https://www.regular-expressions.info/balancing.html

## Matching Palindromes

^(?'letter'[a-z])+[a-z]?(?:\k'letter'(?'-letter'))+(?(letter)(?!))$ => matches palindrome words of any length.

## Recursion

(?R) | (?0) | \g<0> => sintaxis para la recursion

a(?R)?z | a(?0)?z | a\g<0>?z => all match one or more letters a followed by exactly the same number of letters z

The main purpose of recursion is to match balanced constructs or nested constructs. The generic regex is b(?:m|(?R))*e where b is what begins the construct, m is what can occur in the middle of the construct, and e is what can occur at the end of the construct.

You can use an atomic group instead of the non-capturing group for improved performance: b(?>m|(?R))*e.

\((?>[^()]|(?R))*\) matches a single pair of parentheses with any text in between, including an unlimited number of parentheses, as long as they are all properly paired.

> Not all the flawors support recursion with alternation outside a gruop. The solution is put all alternations inside one.


