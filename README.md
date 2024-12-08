# refs.txt

refs.txt is a fork of the [todo.txt](https://github.com/todotxt/todo.txt) plain text (human-readable) format specification. Whereas [todo.txt](https://github.com/todotxt/todo.txt) is intended for representing tasks and their priorities, refs.txt is intended for representing personal collections of bibliographic references (e.g. articles, theses, technical reports). Both are intended for easy entry, search, filtering and manipulation using a text editor and/or basic command line utilities. In other words, refs.txt is a plain text and minimalist alternative to reference management software like [Zotero](https://www.zotero.org/) or [Mendeley](https://www.mendeley.com/).

Being a derivative of todo.txt, the central feature of refs.txt is that any given item in your collection is represented using a single (human-readable) line in your refs.txt file. Items have the following structure (diagram credit [dinosv](https://github.com/todotxt/todo.txt/pull/68)):
```
┌────────────────────────────────────────────────────────── Optional - Marks completion e.g. "I've read/annotated this"
│     ┌──────────────────────────────────────────────────── Optional - Completion date
|     |                                                     (may only be specified if marked complete)
│     │       ┌──────────────────────────────────────────── Optional - Marks priority e.g. "I should read this today"
│     │       │      ┌───────────────────────────────────── Required - Creation date
│     │       │      │                 ┌─────────────────── Optional - Reference key
|     |       |      |                 |               ┌─── Required - Title
│     │       │      │                 │               |    tags (optional) may appear anywhere within it
│ ┌───┴────┐ ┌┴┐ ┌───┴────┐ ┌──────────┴───────────┐ ┌─┴──────────────────────────────────────────────────────────────────────────┐
x 2016-05-20 (A) 2016-04-30 #shannon1948mathematical A Mathematical Theory of Communication +informationTheory @article doi:10/b39t
                                                                                            └────────┬───────┘ └───┬──┘ └────┬────┘
                                                                    collection tag ──────────────────┘             │         |
                                                              publication type tag ────────────────────────────────┘         │
                                                             special key-value tag ──────────────────────────────────────────┘
```
The completion marker, creation/completion date, priority, reference key and title must appear in the order listed in the diagram above. In this way, items are guaranteed to appear in the following order after a lexicographical sort:
1. Items with a priority and not marked complete
2. Items without a priority and not marked complete
3. Items marked complete

Lines in refs.txt are intentionally terse approximations of bibliographic references. The optional reference key is a human-readable identifier that is unique to each item and which is used to link to more comprehensive bibliographic information. To this end, the reference key should be the name of a directory containing any files related to the item, such as BibTeX/RIS entries as well as PDFs and notes. 

## refs.txt vs. todo.txt
As of the most recent release, refs.txt differs in syntax from [todo.txt](https://github.com/todotxt/todo.txt) as follows:
* Creation dates are required (Non-breaking change to ensure that items marked as 'completed' sort correctly)
* Optional completion date must precede priority (Breaking change, but in practice already implemented, see e.g. [simpletask](https://github.com/mpcjanssen/simpletask-android))
* Optional reference key using `#` token (Non-breaking change)
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
### Incomplete Tasks
### Complete Tasks
## Format Specification

## Key-Value Tags

Tool developers may define additional formatting rules for extra metadata.

Developers should use the format `key:value` to define additional metadata (e.g. `due:2010-01-02` as a due date).

TODO fix - Both `key` and `value` must consist of non-whitespace characters, which are not colons. Only one colon separates the `key` and `value`.

