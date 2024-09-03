# regex_tutorial

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



