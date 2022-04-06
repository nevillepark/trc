- [	Workflow](#workflow)
  * [Formatting](#formatting)
    + [Regexes](#regexes)
    + [Common acronyms](#common-acronyms)
  * [Preparing .txt version](#preparing-txt-version)
  * [Preparing .md version](#preparing-md-version)
  * [Git Commands](#git-commands)
  * [To do](#to-do)

# Workflow

**Note:** This is very much a work in progress and there are lots of ways to improve this!

`pdftotext -f [FIRST PAGE] -l [LAST PAGE] -y 60 -W 450 -H 700 -layout -nopgbrk file.pdf file.txt`

This converts the PDF to text while

- `-y 60 -W 450 -H 700` cropping out the header and footer,
- `-layout -nopgbrk` preserving paragraph indents and omitting page break characters, for easier formatting with regex search & replace

To test, select a few pages and append ` - ` instead of a filename so it prints to stdout.

For the columns of text in Appendices 2.1 and 2.2, I converted each column on each page separately, then concatenated them together into one file, using a script like this:

```
for i in {369..370}; do
	pdftotext -f $i -l $i -y 60 -W 240 -H 700 -nopgbrk Executive_Summary_English_Web.pdf ${i}a.txt 
	pdftotext -f $i -l $i -x 240 -y 60 -W 240 -H 700 -nopgbrk Executive_Summary_English_Web.pdf ${i}b.txt 
	cat ${i}a.txt ${i}b.txt >> appendix-2.2.txt
done
```

## Formatting

- Use [regexr.com](http://regexr.com) or `tr` to add newlines after headings/between paragraphs as necessary.

- In `nano`, un-indent things that shouldn't be indented (e. g. bulleted lists, text that flows around an image); indent things that should be indented (blockquotes). Check against PDF to find image captions in the text and separate them from the paragraphs.

- Regex out hyphenated words at the ends of lines. Make sure to read through search results and fix any words that are supposed to be hyphenated.

- Open with `nano -r 10000` and justify (M-J) to remove line breaks in the middle of paragraphs.

- Kate's "unwrap" command can also be used for this.

- Mark up footnotes with regex. Make sure the number of footnotes matches the number of search results.

- Put acronyms/abbreviations into uppercase.

- Mark up headings.

- **Markdown version:** Wrap footnote numbers in `<sup>` tags using regex; indent blockquotes with regex; run through [markdown-toc](https://ecotrust-canada.github.io/markdown-toc/) to generate a table of contents, paste at beginning of file.

- **Plaintext version:** AFTER all text changes have been made, justify text versions at 80 characters; replace Markdown headings with = and - as underlines. 

### Regexes

| | Search | Replace | Notes |
| --- | --- | --- | --- |
| Paragraph breaks | `\n\ {3,}` | `\n\n` | |
| Hyphenated words | `\b-\n` | |
| Footnotes, plaintext | `([.?!"”’])(\d+)` | `$1^$2` | 
| Footnotes, Markdown | `\^(\d+)` | `<sup>$1</sup>` | Find a way to exclude dollar figures, percents, pop'n figures, etc. |
| School names in Appendices 2.1-2 | `(?!\#\#\s)(?<=\n\n)(.+)` | `**$&**` | Yes this is a mess |

### Common acronyms

Abbreviations and acronyms are in small caps in the PDF and are output in lowercase in the text file. To convert them all to uppercase in one go, I created [a file called `abbr`](abbr) in my `~/.local/bin` directory with the search-and-replace expressions, with the pattern

`s/\babbreviation\b/ABBREVIATION/g`

Then ran the command

`sed -f abbr <original-file.txt >new-file.txt`.

## Git Commands

- do stuff locally

```
git add *
git commit -m "Message"
git push origin master
```

- fetch updates from github

`git pull`

- reset everything

`git fetch origin`
`git reset --hard origin/master`

See [this guide](https://kbroman.org/github_tutorial/pages/routine.html) for a more detailed explanation.

## To do

- [ ] Is there a way to extract pictures & graphs?
- [ ] lern 2 tr nub
