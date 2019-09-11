**Contents of the Mark Down Specs**
  1. [Headers](#headers)
  2. [Emphasis](#emphasis)
  3. [Paragraph](#paragraph)
  4. [Lists](#lists)
  5. [Tables](#tables)
  6. [Blockquotes](#blockquotes)
  7. [Links](#links)
  8. [Images](#images)
  9. [Code](#code)
  10. [Miscellaneous](#miscellaneous)
***  
# HEADERS
# H1
## H2
### H3
#### H4
##### H5
###### H6
# EMPHASIS
Italics with *asterisks* or _underscores_.\
Bold, with **double asterisks** or __double underscores__.\
To avoid creating bold or italic, place a backslash in front \* or \_\
Strikethrough uses two tildes. ~~Scratch this.~~
# PARAGRAPH
For a line break, add either a backslash \ or two blank spaces at the end of the line.\
To make the raw MD prettier, you can leave a blank line instead of putting slash at the end of para or sentence\
To start a line from a specified intentation, provide same intentation /(generally 2 spaces) in the raw MD itself
To have multiple paragraphs, insert 1 vertical space.\
This is first para

This is second para
# LISTS
+ Unordered list with asterisks or plus or minus
  1. ordered sub-list with 2 space prefixed and then number followed by dot or round-brace
  2. another ordered sub-list
* Unordered list
  * unordered sub-list with 2 space prefixed
  * another ordered sub-list
1. Ordered list
    - unordered sub-list with 4 space prefixed
    - another ordered sub-list
2. Second ordered list
   Paragraph with 3 space prefixed
# TABLES
There must be at least 3 dashes separating each header cell.\
The outer pipes (|) are optional, and you don't need to make the raw Markdown line up prettily.\
Colons can be used to align columns\
You must include a blank line before your table in order for it to correctly render.\
Example 1 -
```markdown
| Tables        | Are           | Cool  |
| ------------- |:-------------:| -----:|
| col 3 is      | right-aligned | $1600 |
| col 2 is      | centered      |   $12 |
| zebra stripes | are neat      |    $1 |
```
Example 2 -
```markdown
Markdown | Less | Pretty
--- | --- | ---
*Still* | `renders` | **nicely**
1 | 2 | 3
```
# BLOCKQUOTES
To create a blockquote, start a line with __greater than__ followed by an space.\
Blockquotes can be nested, and can also contain other formatting.
> The Quote by me.

Multiple lines Quote
> Allowing an unimportant mistake to pass without comment is a wonderful social grace.
>
> Ideological differences are no excuse for rudeness.
# LINKS
- Links can be either -
  - inline with the text **\[visible_text](URL)**, or 
  - placed at the bottom of the text as references **\[visible_text]\[id] ..... \[id]: URL "title"**\
- Another simple way - URLs may not become links in Markdown until enclosed in \< and \>
# IMAGES
- Images are almost identical to links, but an image starts with an exclamation point ! -
  - !\[alt](cat.png)
  - !\[alt]\[id] .... \[id]: dog.jpg "title"
# CODE
To create inline code, wrap with backticks \`\
To create a code block, either indent each line by 4 spaces, or place 3 backticks \`\`\` on a line above and below the code block.\
For code block, you can also specify the code language like \`\`\`json or \`\`\`html
# MISCELLANEOUS
1. Horizontal Rule - Put 3 asterisks under the sentence or word
2. Task List - To create a task list, preface list items with a regular space character followed by `[ ]`. To mark a task as complete, use `[x]`
```
- [x] Finish my changes
- [ ] Push my commits to GitHub
- [ ] Open a pull request
```
3. Emoji - You can add emoji to your writing by typing `:EMOJICODE:`\
   Example - :+1: This PR looks great - it's ready to merge! :shipit:\
   For a full list of available emoji and codes, check out [This Link](http://emoji-cheat-sheet.com/)
