Regular Expression (aka *regex* or *regexp*) is a sequence of characters that defines a certain search pattern to match a certain amount of text. The name *regular* is from related mathematical theory, we will not mention that in this blog.

## Engineddd
Generally, we have two kinds of regular expression engines.

- **DFA**. DFA is based on the text itself, it ensures that the longest possible string will be matched. ***MySQL*** use DFA engine.

- **NFA**. NFA is based on the regular expression, it accepts the first match it receives and ignore others, maybe the longer match. This "greedy" recursion matching method will be slow if the worst case (many choices matches the different branches in regular expression) happens. ***Python***, ***Java*** use NFA engine.

#### A Simple Example
Take string 'blog.xliu.info' for an example, the regular expression is ``/xl(iu|ui)\.[Ii]n*/``. 

DFA reads the string and match the text to the expression. That is, read 'b', give up, then 'l' give up, ..., until it finds that 'x' matches the first meta-character of the regular expression. After DFA finds 'x', it will read 'l' to check if it still matches the expression. Finally, DFA will find 'xliu.in'.

NFA, on the other hand, reads the regular expression. That is, it first read 'x' in regular expression and find it in the string, then finds 'l' in it, then 'iu', if not found, find 'ui'. Finally, NFA will find 'xliu.in'.

In a word, DFA use string to test regular expression, while NFA use regular expression to test string. Therefore, we could tell why NFA will suffer a cannot-found-longest-match problem. However, NFA respects the regular expression with the biggest effort. We should balance our need between find a proper result and find the longest result.


## Basic Usage

#### Anchors
- ``` \ ```	**Escape Indicator**. Transfer next character to a special character. For example, ``/\n/`` will match a line break;
- ``^`` **Beginning**. Match the start point of the string;
- ``$`` **Ending**. Match the end point of the string;
- ``.`` **Dot**. Match any characters without ``\n``.

#### Quantifier
- ``*`` **Star**. Match the previous character zero or more times;
- ``+`` **Plus**. Match the previous character one or more times;
- ``?`` **Optional**. Match the previous character zero or one time;
- ``{n}`` Match the previous character *exactly* n times;
- ``{n,}``  Match the previous character *at least* n times;
- `` {n,m} ``  Match the previous character *at least* n times and *at most* m times;
- ``?`` **Lazy**. Match in a lazy(non-greedy) way or a less-match-way when follows a repeat character. In string 'zoo', ``zo*?`` will match 'z'.

#### Logic Characters
- ``x|y`` **Alternation**. Match ``x`` or ``y``, basically not limits on character numbers. One should indicate the range explicitly;
- ``[xyz]`` **Character Set**. Match any character in it. A better way to express multiple ``OR``s;
- ``[^xyz]`` **Negated Set**. Match any character that is *Not* in it;
- ``a-z`` **Range**. Match a character in the range `a` to `z`, in another words, whose char code lies in between 97 and 122.

#### Special Characters
- ``\.`` **Escape Character**. Match a dot "." character;
- ``\b`` **Word Boundary**. Match a word boundary position such as whitespace, punctuation, or the start/end of the string. For example, ``/ctrl\b-a/`` will match "ctrl-a";
- ``\B`` **Not word boundary**. Match any position that is not a word boundary. For example ``/ne\B/`` will match "never" but not "done";
- ``\cx`` **Control character escape**. Escaped control character in the form \cZ. For example, ``/\cM/`` match a *CARRIAGE RETURN* character; Note that x have a range in a-z or A-Z to match the first 26 characters in ASCII table, otherwise it will match "\\", "c" and "x";
- ``\d`` **Digit**. Match any digit character. Same as ``[0-9]``;
- ``\D`` **Not Digit**. Match any character that is *NOT* a digit;
- ``\f`` **Escape Character**. Match a *FORM FEED* character, same as ``\cl``. Like ``\newpage`` command in LaTex;
- ``\n`` **Escape Character**. Match a *LINE FEED* character, same as ``\cj``;
- ``\r`` **Escape Character**. Match a *CARRIAGE RETURN* character, same as ``\cm``;
- ``\s`` **Whitespace**. Match any whitespace character (spaces, tabs, line breaks);
- ``\S`` **Not Whitespace**. Match any character that is *NOT* a whitespace;
- ``\t`` **Escape Character**. Match a *TAB* character, same as ``\ci``;
- ``\v`` **Escape Character**. Match a *VERTICAL TAB* character, same as ``\ck``;
- ``\w`` **Word**. Match a word character (alphanumeric and underscore);
- ``\W`` **Not Word**. Match a character that is not a word;
- ``\xFF`` **Hexadecimal Escape**. Hexadecimal escaped character, match a character has char code "FF" in ASCII (must be a two digit hexadecimal). 

#### Groups and Lookahead
- ``(pattern)`` **Capturing group**. Groups multiple tokens together and creates a capture group for extracting a substring or using a [back reference](#back-reference).
- ``(?:pattern)`` **Non-capturing group**. Groups multiple tokens together without creating a capture group.
- ``(?=pattern)`` **Positive Lookahead**. Matches a group after the main expression without including it in the result. For example, ``/x(?=liu)/`` will match ``x``, which followed by a ``liu``. It is a non-capturing match. 
- ``(?!pattern)`` **Negative Lookahead**. Specifies a group that can not match after the main expression. For example, ``/x(?!liu)/`` will match ``x``, which is *NOT* followed by a ``liu``. It is a capturing match.

## Terminologies

#### Back Reference
In a regular expression, when one pattern is surrounded by a pair of brackets, like ``(pattern)``, the string this pattern matched will be captured and given a group number, in later regular expression, we could use `\num` to indicate that string. For example, regex ``/(\w+{4})\1/`` will match a 8-character string in which the last 4 characters are exactly the same as the previous 4, therefore, string 'bilibili' and 'niconico' will match.

If we want tear apart a URL into its components, such as

```http://blog.xliu.info/Regular-Expression#back-reference```

We could use regular expression 

```/(\w+):\/\/([^/:]+)(:\d*)?([^# ]*)/```

Therefore, in the above example, 
- Group $1 contains "http"
- Group $2 contains "blog.xliu.info"
- Group $3 contains ""
- Group $4 contains "Regular-Expression"

#### Capturing/Not-Capturing Group

In back reference, we know that the group is used to store a string in it for a latter reference, such group is called capturing group. While, sometimes we do not want to refer to one group but we need to group them together, that is where non-capturing group shows up. Pattern like ``(?:pattern)`` will not store the string the pattern matched, which is, in some respect, more efficient. 

## Simple Examples 

- **Integer & Decimal Numbers**. ``/(?:\d*\.)?\d+/``
- **English Name**. ``/^[A-Z]['a-zA-Z]+(\s*[A-Z][-a-zA-Z]+)+(\s*(junior|jr|Jr|JR)\.?)?$/``
- **IP Address**. ``/^(((25[0-5])|(2[0-4]\d)|([01]?\d\d?))\.){3}((25[0-5])|(2[0-4]\d)|([01]?\d\d?))$/``
- **Email Address**. ``/^( |\t)*([\w._+-]+\s*(@|\[?a+t+\]?)+\s*([\w._+-]+\s*(\.|\[?d+o+t+\]?)+\s*)+[\w._+-]+)( |\t)*$/``

## References
> [Regular Expression-代码树](http://www.codeyyy.com/regex/)

> [RegExr: Learn, Build & Test RegEx](http://www.regexr.com/)