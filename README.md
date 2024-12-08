# refs.txt
refs.txt is a fork of the [todo.txt](https://github.com/todotxt/todo.txt) plain text (human-readable) format specification. Whereas [todo.txt](https://github.com/todotxt/todo.txt) is intended for representing tasks and their priorities, refs.txt is intended for representing personal collections of bibliographic references (e.g. articles, theses, technical reports) with associated files (e.g. BibTeX entries, PDFs, notes). Both are intended for easy entry, search, filtering and manipulation using a text editor and/or basic command line utilities. In other words, refs.txt is a plain text and minimalist alternative to reference management software like [Zotero](https://www.zotero.org/) or [Mendeley](https://www.mendeley.com/).

Being a derivative of todo.txt, the central feature of refs.txt is that any given item in your collection is represented using a single (human-readable) line in your refs.txt file. Items have the following structure (diagram credit [dinosv](https://github.com/todotxt/todo.txt/pull/68)):
```
┌───────────────────────────────── Optional - Marks completion e.g. "I've read/annotated this"
│     ┌─────────────────────────── Optional - Completion date
|     |                            (may only be specified if marked complete)
│     │       ┌─────────────────── Optional - Marks priority e.g. "I should read this today"
│     │       │      ┌──────────── Required - Creation date
|     |       |      |        ┌─── Required - Title
│     │       │      │        |    additional tags (optional) may appear anywhere within title
│ ┌───┴────┐ ┌┴┐ ┌───┴────┐ ┌─┴─────────────────────────────────────────────────────────────────────────────────────────────────────┐
x 2016-05-20 (A) 2016-04-30 A Mathematical Theory of Communication #shannon1948mathematical +informationTheory @myProject doi:10/b39t
                                                                   └───────────┬──────────┘ └────────┬───────┘ └────┬───┘ └────┬────┘
                                                              reference tag ───┘                     |              |          |
                                                                  topic tag ─────────────────────────┘              │          |
                                                                project tag ────────────────────────────────────────┘          │
                                                      special key-value tag ───────────────────────────────────────────────────┘
```
The completion marker, creation/completion date, priority and title must appear in the order listed in the diagram above. In this way, items are guaranteed to appear in the following order after a lexicographical sort:
1. Items with a priority and not marked complete
2. Items without a priority and not marked complete
3. Items marked complete

Lines in refs.txt are (intentionally terse) approximations of bibliographic references with additional metadata. The optional reference tag is a human-readable identifier that is unique to each item and which is used to link to more comprehensive bibliographic information. The reference tag might be the name of a directory containing any files related to the item, such as BibTeX/RIS entries as well as PDFs and notes. Thus, continuing with the previous example, your directory structure might look like this:
```
.
|-- refs.txt
|-- shannon1948mathematical   
|   |-- manuscript.pdf
|   |-- manuscript.txt
|   |-- notes.txt
|   `-- reference.bib
...  (Other directories omitted for brevity)
```
## refs.txt versus todo.txt
As of the most recent release, refs.txt differs in syntax from [todo.txt](https://github.com/todotxt/todo.txt) as follows:
* Creation dates are required (Non-breaking change to ensure that items marked as 'completed' sort correctly)
* Optional completion date must precede priority (Breaking change, but in practice already implemented, see e.g. [simpletask](https://github.com/mpcjanssen/simpletask-android))
* Optional reference tag using `#` token (Non-breaking change)
* Colons are permitted in the value of key-value tags

If you are already familiar with todo.txt, you will notice mainly semantic differences. The main reason for this specification is to describe how to represent data for the intended use case, namely reference management.

## Advantages of Plain Text
* Simplicity (no databases)
* Few dependencies (editor and/or basic command line utilities)
* Extensible entry/search/filtering/manipulation using standard tools
* Portability
* Avoids lock-in / future data conversion effort
* Light on resources (Hello, [Electron](https://www.electronjs.org/docs/latest))
* Easy to version and share (e.g. using git)

## Examples
### Items Not Marked Complete
At minimum, a valid item looks like this:
```
2016-04-30 A Mathematical Theory of Communication
```
These are valid alternatives:
```
(A) 2016-04-30 A Mathematical Theory of Communication
2016-04-30 A Mathematical Theory of Communication #shannon1948mathematical 
2016-04-30 #shannon1948mathematical A Mathematical Theory of Communication +topic1 +topic2 @myProject1 @myProject2
```
None of these are valid items:
```
A Mathematical Theory of Communication
2016-04-30 #shannon1948mathematical
2016-04-30 A Mathematical Theory of Communication #shannon1948mathematical #foo
 2016-04-30 A Mathematical Theory of Communication
```
### Items Marked Complete
At minimum, a valid item which has been marked as completed looks like this:
```
x 2016-04-30 A Mathematical Theory of Communication
```
These are valid alternatives:
```
x 2016-05-20 2016-04-30 A Mathematical Theory of Communication
x 2016-05-20 (A) 2016-04-30 A Mathematical Theory of Communication
```
None of these are valid items:
```
2016-05-20 2016-04-30 A Mathematical Theory of Communication
x2016-05-20 2016-04-30 A Mathematical Theory of Communication
x (A) 2016-05-20 2016-04-30 A Mathematical Theory of Communication
```

## ref.txt Format Specification
* The recommended encoding is UTF-8
* The recommended line separator is `\n`
* Lines must either be empty or start with a printable character
* Items must have a title
* The title must be preceded by a creation date in the format YYYY-MM-DD
* The creation date may be preceded by an optional priority in the format (P)
* The priority class should be a capital letter in the set [A-Z]
* Items may optionally be marked as 'complete', the recommended completion marker `x` must appear at the start of the line
* Items marked as complete may include an optional completion date in the format YYYY-MM-DD
* The completion date (if present) must appear after the completion marker and before the creation date. The completion date (if present) must appear before the priority (if present).
* Tags may appear anywhere in the title
* The title should contain no more than 1 reference tag.
* An item's reference tag should be human-readable and unique to the item; the recommended format is authorYearKeyword. 
* The title may contain any number of topic or project tags, however no distinct topic or project tag should occur more than once in the title.
* The title may contain any number of special key-value tags, however no distinct key should occur more than once in the title.
* Completion marker, start date, priority, end date, and title must be separated by the same whitespace character combination. It is recommended to use 1 space as the separator.
* Tags must be separated by 1 or more whitespace characters. It is recommended to use 1 space as the separator.
* With the exception of keys (which should consist of characters in the set [a-zA-Z] and which should be considered case-sensitive), tags may comprise all printable characters.

  
### Key-Value Tags

Tool developers may define additional formatting rules for extra metadata.

Developers should use the format `key:value` to define additional metadata (e.g. `due:2010-01-02` as a due date).

TODO fix - Both `key` and `value` must consist of non-whitespace characters, which are not colons. Only one colon separates the `key` and `value`.

TODO h:1 to hide an item
TODO ! comment h:1 for comments
TODO h:1 version:refs.txt_v1
