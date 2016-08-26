<!--
author: zhangxuefeng
date: 2016-06-27
title: 正则表达式与preg函数
tags: 正则表达式
category: tool
status: publish
summary: 正则规范
-->

[原文链接](http://www.noupe.com/development/php-regular-expressions.html)

operator | DESCRIPTION
---|---
^ | The circumflex symbol marks the beginning of a pattern, although in some cases it can be omitted
$ |	Same as with the circumflex symbol, the dollar sign marks the end of a search pattern
. |	The period matches any single character
? |	It will match the preceding pattern zero or one times
+ |	It will match the preceding pattern one or more times
* |	It will match the preceding pattern zero or more times
\| | Boolean OR
–  | Matches a range of elements
() | Groups a different pattern elements together
[] | Matches any single character between the square brackets
{min, max} | It is used to match exact character counts
\d | Matches any single digit
\D | Matches any single non digit caharcter
\w |	Matches any alpha numeric character including underscore (_)
\W |Matches any non alpha numeric character excluding the underscore character
\s |Matches whitespace character




EXAMPLE | DESCRIPTION
---|---
‘/hello/’|It will match the word hello
‘/^hello/’|It will match hello at the start of a string. Possible matches are hello or helloworld, but not worldhello
‘/hello$/’|It will match hello at the end of a string.
‘/he.o/’|It will match any character between he and o. Possible matches are helo or heyo, but not hello
‘/he?llo/’|It will match either llo or hello
‘/hello+/’|It will match hello on or more time. E.g. hello or hellohello
‘/he*llo/’|Matches llo, hello or hehello, but not hellooo
‘/hello|world/’|It will either match the word hello or world
‘/(A-Z)/’|Using it with the hyphen character, this pattern will match every uppercase character from A to Z. E.g. A, B, C…
‘/[abc]/’|It will match any single character a, b or c
‘/abc{1}/’|Matches precisely one c character after the characters ab. E.g. matches abc, but not abcc
‘/abc{1,}/’|Matches one or more c character after the characters ab. E.g. matches abc or abcc
‘/abc{2,4}/’|Matches between two and four c character after the characters ab. E.g. matches abcc, abccc or abcccc, but not abc


## 常用函数

1. [preg_match](http://php.net/manual/zh/function.preg-match.php) 


2. [preg_repalce](http://php.net/manual/zh/function.preg-replace.php)


## game

[title](http://regex.alf.nu/)
[answer](http://felixc.at/regex.alf.nu)

