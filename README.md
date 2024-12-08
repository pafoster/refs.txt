# refs.txt

refs.txt is a fork of the [todo.txt](https://github.com/todotxt/todo.txt) plain text (human-readable) format specification. Whereas [todo.txt](https://github.com/todotxt/todo.txt) is intended for representing tasks and their priorities, refs.txt is intended for representing personal collections of bibliographic references (e.g. articles, theses, technical reports). Both are intended for easy entry, search, filtering and manipulation using a text editor and/or basic command line utilities. In other words, refs.txt is a plain text and minimalist alternative to reference management software like [Zotero](https://www.zotero.org/) or [Mendeley](https://www.mendeley.com/).

Being a derivative of todo.txt, the central feature of refs.txt is that any given item in your collection is represented using a single (human-readable) line in your refs.txt file. Items have the following structure (diagram adapted from [existing PR](https://github.com/todotxt/todo.txt/pull/68/files#)):
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
The completion marker, creation/completion date, priority, reference key and title must appear in the order listed above in the diagram. In this way, items are guaranteed to appear in the following order after a lexicographical sort:
1. Items with a priority and not marked complete
2. Items without a priority and not marked complete
3. Items marked complete

Lines in refs.txt are intentionally terse approximations of bibliographic references. The optional reference key is a human-readable identifier that is unique to each item and which is used to link to more comprehensive bibliographic information. To this end, the reference key should be the name of a directory containing any files related to the item, such as PDFs, notes, and BibTeX/RIS entries. 

## refs.txt versus todo.txt
As of the most recent release, refs.txt differs in syntax from [todo.txt](https://github.com/todotxt/todo.txt) as follows:
* Creation dates are required (Non-breaking change to ensure that items marked as 'completed' sort correctly)
* Optional completion date must precede priority (Breaking change, but in practice already implemented, see e.g. [simpletask](https://github.com/mpcjanssen/simpletask-android))
* Optional reference key using `#` token (Non-breaking change)
* Colons are permitted in the value of key-value tags

If you are already familiar with todo.txt, you will notice mainly semantic differences. The main reason for this specification is to describe how to represent data for the intended use case, namely reference management.

## Why plain text?

Plain text is software and operating system agnostic. It's searchable, portable, lightweight, and easily manipulated. It's unstructured. It works when someone else's web server is down or your Outlook .PST file is corrupt. There's no exporting and importing, no databases or tags or flags or stars or prioritizing or _insert company name here_-induced rules on what you can and can't do with it.


## The 3 axes of an effective todo list

Using special notation in todo.txt, you can create a list that's sliceable by 3 key axes.


### Priority
Your todo list should be able to tell you what's the next most important thing for you to get done - either by project or by context or overall. You can optionally assign tasks a priority that'll bubble them up to the top of the list.


### Project
The only way to move a big project forward is to tackle a small subtask associated with it. Your `todo.txt` should be able to list out all the tasks specific to a project.

In order to move along a project like "Cleaning out the garage," my task list should give me the next logical action to take in order to move that project along. "Clean out the garage" isn't a good todo item; but "Call Goodwill to schedule pickup" in the "Clean out garage" project is.


### Context
[Getting Things Done](https://en.wikipedia.org/wiki/Getting_Things_Done) author David Allen suggests splitting up your task lists by context - ie, the place and situation where you'll work on the job. Messages that you need to send go in the `@email` context; calls to be made `@phone`, household projects `@home`.

That way, when you've got a few minutes in the car with your cell phone, you can easily check your `@phone` tasks and make a call or two while you have the opportunity.

This is all possible inside `todo.txt`.



## `todo.txt` format rules

<img src="./description.svg" width="100%" height="500">

Your `todo.txt` is a plain text file. To take advantage of structured task metadata like priority, projects, context, creation, and completion date, there are a few simple but flexible file format rules.

Philosophically, the `todo.txt` file format has two goals:

- The file contents should be human-readable without requiring any tools other than a plain text viewer or editor.
- A user can manipulate the file contents in a plain text editor in sensible, expected ways. For example, a text editor that can sort lines alphabetically should be able to sort your task list in a meaningful way.

These two goals are why, for example, lines start with priority and/or dates, so that they are easily sorted by priority or time, and completed items are marked with an `x`, which both sorts at the bottom of an alphabetical list and looks like a filled-in checkbox.

Here are the rest.


## Incomplete Tasks: 3 Format Rules

The beauty of todo.txt is that it's completely unstructured; the fields you can attach to each task are only limited by your imagination. To get started, use special notation to indicate task context (e.g. `@phone` ), project (e.g. `+GarageSale` ) and priority (e.g. `(A)` ).

A todo.txt file might look like the following:

```
(A) Thank Mom for the meatballs @phone
(B) Schedule Goodwill pickup +GarageSale @phone
Post signs around the neighborhood +GarageSale
@GroceryStore Eskimo pies
```

A search and filter for the `@phone` contextual items would output:

```
(A) Thank Mom for the meatballs @phone
(B) Schedule Goodwill pickup +GarageSale @phone
```

To just see the `+GarageSale` project items would output:

```
(B) Schedule Goodwill pickup +GarageSale @phone
Post signs around the neighborhood +GarageSale
```

There are three formatting rules for current todo's.

### Rule 1: If priority exists, it ALWAYS appears first.

The priority is an uppercase character from A-Z enclosed in parentheses and followed by a space.

This task has a priority:

```
(A) Call Mom
```

These tasks do not have any priorities:

```
Really gotta call Mom (A) @phone @someday
(b) Get back to the boss
(B)->Submit TPS report
```


### Rule 2: A task's creation date may optionally appear directly after priority and a space.

If there is no priority, the creation date appears first. If the creation date exists, it should be in the format `YYYY-MM-DD`.

These tasks have creation dates:

```
2011-03-02 Document +TodoTxt task format
(A) 2011-03-02 Call Mom
```

This task doesn't have a creation date:

```
(A) Call Mom 2011-03-02
```


### Rule 3: Contexts and Projects may appear anywhere in the line _after_ priority/prepended date.

- A *context* is preceded by a single space and an at-sign (`@`).
- A *project* is preceded by a single space and a plus-sign (`+`).
- A *project* or *context* contains any non-whitespace character.
- A *task* may have zero, one, or more than one *projects* and *contexts* included in it.

For example, this task is part of the `+Family` and `+PeaceLoveAndHappiness` projects as well as the `@iphone` and `@phone` contexts:

```
(A) Call Mom +Family +PeaceLoveAndHappiness @iphone @phone
```

This task has no contexts in it:

```
Email SoAndSo at soandso@example.com
```

This task has no projects in it:

```
Learn how to add 2+2
```



## Complete Tasks: 2 Format Rules

Two things indicate that a task has been completed.


### Rule 1: A completed task starts with an lowercase x character (`x`).

If a task starts with an `x` (case-sensitive and lowercase) followed directly by a space, it is marked as complete.

This is a complete task:

```
x 2011-03-03 Call Mom
```

These are not complete tasks.

```
xylophone lesson
X 2012-01-01 Make resolutions
(A) x Find ticket prices
```

We use a lowercase x so that completed tasks sort to the bottom of the task list using standard sort tools.


### Rule 2: The date of completion appears directly after the x, separated by a space.

For example:

```
x 2011-03-02 2011-03-01 Review Tim's pull request +TodoTxtTouch @github
```

If you’ve prepended the creation date to your task, on completion it will appear directly after the completion date. This is so your completed tasks sort by date using standard sort tools. Many Todo.txt clients discard priority on task completion. To preserve it, use the `key:value` format described below (e.g. `pri:A`)

With the completed date (required), if you've used the prepended date (optional), you can calculate how many days it took to complete a task.



## Additional File Format Definitions

Tool developers may define additional formatting rules for extra metadata.

Developers should use the format `key:value` to define additional metadata (e.g. `due:2010-01-02` as a due date).

Both `key` and `value` must consist of non-whitespace characters, which are not colons. Only one colon separates the `key` and `value`.

