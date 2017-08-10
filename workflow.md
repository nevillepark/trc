- [Workflow](#workflow)
  * [Formatting](#formatting)
    + [Regexes](#regexes)
    + [Common acronyms](#common-acronyms)
  * [Preparing .txt version](#preparing-txt-version)
  * [Preparing .md version](#preparing-md-version)
  * [Git Commands](#git-commands)
  * [To do](#to-do)

# Workflow

**Note:** This is very much a work in progress and there are lots of ways to improve this!

``` pdftotext -f [FIRST PAGE] -l [LAST PAGE] -y 60 -W 450 -H 700 -layout -nopgbrk file.pdf file.txt```

This converts the PDF to text while

- ```-y 60 -W 450 -H 700``` cropping out the header and footer,
- ```-layout -nopgbrk``` preserving paragraph indents and omitting page break characters, for easier formatting with regex search & replace

To test, select a few pages and append ``` - ``` instead of a filename so it prints to stdout.

## Formatting

- Use [regexr.com](http://regexr.com) or ```tr``` to add newlines after headings/between paragraphs as necessary.

- In ```nano```, un-indent things that shouldn't be indented (e. g. bulleted lists, text that flows around an image). Check against PDF to find image captions in the text and separate them from the paragraphs.

- Regex out hyphenated words at the ends of lines. Make sure to read through search results and fix any words that are supposed to be hyphenated.

- Open with ```nano -r 10000``` and justify (M-J) to remove line breaks in the middle of paragraphs.

- Mark up footnotes with regex. Make sure the number of footnotes matches the number of search results.

- Find acronyms and put into uppercase.

### Regexes

Footnotes, plaintext:	```([.?!"”’])(\d+)	$1^$2```
Footnotes, Markdown: 	```\^(\d+)				<sup>$1</sup>```
  (Note: find a way to exclude dollar figures, percents, etc.)

Paragraph breaks			```\n\s{3,}				\n\n```
  (Note: after this some paragraphs will still be indented by 1, find a way to fix)
Hyphenated words			```\b-\n	```
Acronyms						```\bACRONYM\b```

### Common acronyms

CEP
IAP
IRSSA
IRSSC
NCTR
OPP
TRC

## Preparing .txt version

- AFTER all text changes have been made, justify text versions at 80 characters.

- Replace Markdown headings with = and - as underlines.

## Preparing .md version

- Replace footnotes with HTML footnootes using regex.

- Run through [markdown-toc](https://ecotrust-canada.github.io/markdown-toc/) to generate a table of contents, paste at beginning of file.

## Git Commands

- do stuff locally

	git add *
	git commit -m "Message"
	git push origin master

- fetch updates from github

	git pull

- reset everything

	git fetch origin
	git reset --hard origin/master

## To do

- Is there a way to extract pictures & graphs?
- lern 2 tr nub
