Syntax analysis, also known as ==parsing==
### Introduction
![[Pasted image 20230406011707.png]]

After lexical analysis, the compiler can now produce tokens, i.e. characters of the input have been grouped into tokens. Syntax analysis will then try to understand if these tokens are syntactically correct. If they are in a correct order and do they form valid sentences of the input language.
Syntax analysis therefore discovers a derivation for some sentence.

Not all languages can be defined by ==regular expressions==
The descriptive power of regular expressions has limits
* Regular expressions (RE) cannot be used to describe balanced or nested constructs: Example set of all strings of balanced parantheses {(), (()), ((())), ...}
* In regular expressions, a non-terminal symbol cannot be used before it has been fully defined.

#### Chomsky's Hierarchy of Grammar:
1. Phrase structured: Natural languages that we use everyday
2. Context sensitive: Number of left hand side symbols are $\le$ number of right hand side symbols
3. Context-free: The left hand side symbol is a non-terminal
4. Regular: Only rules of the form: $A \implies \epsilon, A \implies a, A \implies pB$ are allowed
Regular languages $\subset$ Context-Free languages $\subset$ Cont.Sens.Ls $\subset$ Phr.Str.Ls
Regular languages are the most constrained type of languages.

#### Expressing Syntax (03:35)

















