Regular Expression (aka *regex* or *regexp*) is a sequence of characters that defines a certain search pattern to match a certain amount of text. The name *regular* is from related mathematical theory, we will not mention that in this blog.

## Engine
Generally, we have two kinds of regular expression engines.

- **DFA** DFA is based on the text itself, it ensures that the longest possible string will be matched. ***MySQL*** use DFA engine.

- **NFA** NFA is based on the regular expression, it accepts the first match it receives and ignore others, maybe the longer match. This "greedy" recursion matching method will be slow if the worst case (many choices matches the different branches in regular expression) happens. ***Python***, ***Java*** use NFA engine.

#### A Simple Example
Take string 'blog.xliu.info' for an example, the regular expression is ```xl(iu|ui)\.[Ii]n*```. 

DFA reads the string and match the text to the expression. That is, read 'b', give up, then 'l' give up, ..., until it finds that 'x' matches the first metacharacter of the regular expression. After DFA finds 'x', it will read 'l' to check if it still matches the expression. Finally, DFA will find 'xliu.in'.

NFA, on the other hand, reads the regular expression. That is, it first read 'x' in regular expression and find it in the string, then finds 'l' in it, then 'iu', if not found, find 'ui'. Finally, NFA will find 'xliu.in'.

In a word, DFA use string to test regular expression, while NFA use regular expression to test string. Therefore, we could tell why NFA will suffer a cannot-found-longest-match problem. However, NFA respects the regular expression with the biggest effort. We should balance our need between find a proper result and find the longest result.


## Basic Usage
- ``` \```	balka
- ```{n,m}```	balsk