# Python RegEx

Python has a built-in package called re, which can be used to work with Regular Expressions.


## Metacharacters
```
import re
txt = "Data Science 01"
```
|Character |Description                     	              |Example          |  Code                         | Output        |
|----------|--------------------------------------------------|-----------------|-------------------------------|---------------|
|[]	|A set of characters	                                   |"[a-d]"         | re.findall("[a-d]", txt)      |  ['a', 'a', 'c', 'c'] |
|\	|Signals a special sequence (or escape special characters)|"\d"             |re.findall("\d", txt)          | ['0', '1']       |	
|.	|Any character (except newline character)	               |"D..a"          |re.findall("D..a", txt)        |  ['Data']        |
|^	|Starts with	                                           |"^Data"	    |re.findall("^Data", txt)       |  ['Data']        |
|$	|Ends with	                                               |"01$"	        |re.findall("01$", txt)         |['01']            |
|*	|Zero or more occurrences                                  |"D.*a"          | re.findall("D.*a", txt)       | ['Data']         |	
|+	|One or more occurrences                                   |"D.+a"	        | re.findall("D.+a", txt)       |['Data']          |
|?	|Zero or one occurrences                                   |"Da.?a"	        |re.findall("Da.?a", txt)       |['Data']          |
|{}	|Exactly the specified number of occurrences               |"D.{2}a"	    |re.findall("D.{2}a", txt)      |['Data'] |
|\||	Either or                                              |"Data\|Analytics"| re.findall("Data\|Analytics", txt) |['Data']       |

## RegEx Functions
- ### `findall`	Returns a list containing all matches.
```
import re
regex = r"@robot\d\W"

sentiment_analysis = '@robot9! @robot4& I have a good feeling that the show isgoing to be amazing! @robot9$ @robot7%'
re.findall(regex, sentiment_analysis)
```
`**Output** = ['@robot9!', '@robot4&', '@robot9$', '@robot7%']`

    

- ### `search`	Returns a Match object if there is a match anywhere in the string.
- ### `split`	Returns a list where the string has been split at each match.


- ### `sub`	Replaces one or many matches with a string.
```
sentiment_analysis = 'He#newHis%newTin love with$newPscrappy. #8break%He is&newYmissing him@newLalready'

regex_sentence = r"\W\dbreak\W"
sentiment_sub = re.sub(regex_sentence, " ", sentiment_analysis)

regex_words = r"\Wnew\w"
sentiment_final = re.sub(regex_words, "", sentiment_sub)
print(sentiment_final)
```

- Matches
    - `r"User_mentions:\d"`                - ['User_mentions:2']
    - `r"likes:\s\d"`                      - ['likes: 9']
    - `r"number\sof\sretweets:\s\d"`       - ['number of retweets: 7']
    - `r"https:\S+"`                       - ['https://www.tellyourstory.com']
    - `r"@\S+" `- [@blueKnight39]
    - `r"\d{1,2}\s\S+\sago" `- ['32 minutes ago']
    - `r"\d{1,2}\w{2}\s\S+\s\d{4}"` - ['23rd June 2018']
    - `r"\d{1,2}\w{1,2}\s\S+\s\d{4}\s\d{1,2}:\d{2}" `- ['23rd June 2018 17:54']
    - `r"^[a-zA-Z0-9!#%&*$.]+@\w+\.com"` - [n.john.smith@gmail.com]
    - `r"[a-zA-Z0-9*#$%!&.]{8,20}"` - [My87hou#4$]
    - `"<.+?>" `- \</strong>
    - `r"\d+?"` - ['5', '3', '6', '1', '2']
    - `r"\d+"` - ['536', '12']
    - `r"\(.+?\)" `- ['(They were so cute)', "(I'm crying)"]
    - `r"([a-zA-Z0-9]+)@\S+"` - ['msdrama098']
    - `r"(hate|dislike|disapprove).+?(?:movie|concert)\s(.+?)\." `- [('dislike', 'The cabin and the ant')]

## Match Object
A Match Object is an object containing information about the search and the result.
```
import re
txt = "Data Science 01"

x = re.search("Science", txt)
print(x)
```
`<re.Match object; span=(5, 12), match='Science'>`

- ### `.span()` returns a tuple containing the start-, and end positions of the match.
```
import re

txt = "Data Science 01"
x = re.search(r"\bS\w+", txt)
print(x.span())
```
`(5, 12)`
- ### `.string` returns the string passed into the function.
```
import re

#The string property returns the search string:

txt = "Data Science 01"
x = re.search(r"\bS\w+", txt)
print(x.string)
```

`Data Science 01`
- ### `.group()` returns the part of the string where there was a match.
```
import re

txt = "Data Science 01"
x = re.search(r"\bS\w+", txt)
print(x.group())
```
`Science`
