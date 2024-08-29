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


