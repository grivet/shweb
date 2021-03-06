# Shweb

Static blog generator written in POSIX shell.

## Quick start

    cargo install pulldown-cmark
    ./shweb ./example
    python -m http.server 8000 --directory /tmp/shweb

The example configuration uses the [pulldown-cmark](https://github.com/raphlinus/pulldown-cmark) interpreter.
It implements [commonmark](https://commonmark.org/), a better defined version of markdown.

The website is then available [locally](http://0.0.0.0:8000/).

## Installation

    make install DESTDIR=~/bin # DESTDIR is memorized
    make uninstall             # Uses DESTDIR previously defined
    make install               # Uses DESTDIR previously defined
    make clean                 # Removes DESTDIR memo

## Usage

Create your source directory. Write `'site.conf'` within. Mandatory variables are:

    SITE_TITLE="Your website title"
    SITE_URL="Your website URL"
    OUTPUT_DIR="/path/to/served/directory"
    MD2HTML=/path/to/markdown

Optionally, the following variables can be overloaded, otherwise
their default is used:

    # Format used with `'date'` for human-read dates.
    DATE_FMT="--iso-8601" 

    # Directory containing your articles in source dir.
    POSTS_DIR=posts

    # Navigation menu on top.
    # List each path to be linked and how they should be named.
    NAV_MENU="
    /about.html: about
    "

    # Path to the generated RSS feed.
    # Skipped if this variable is set to "".
    RSS="/feed.xml"

    # Metadata separator
    META="-->"

    # HEAD finisher
    # Included at the end of the <head> section.
    # Can be used to enable a JS payload or anything.
    HEAD=""

The `'*.md'` sources of your articles must be put in `POSTS_DIR` in the
source directory.

`CSS` and `HTML` documents will be copied as-is to `OUTPUT_DIR` in their respective
directories. All markdown files will be transformed into `HTML` documents and put
into their respective directories as well:

    Source                          Ouput

    .                               .
    +-- about.md                    +-- about.html
    +-- posts                       +-- posts
    +   +-- markdown.md             |   +-- index.html
    |   `-- shweb.md        ->      |   +-- markdown.html
    +-- site.conf                   |   `-- shweb.html
    `-- style.css                   +-- style.css
                                    +-- feed.xml
                                    `-- index.html

All `'*.css'` files will be included in each `HTML` page.

## Metadata

Articles are written in `POSTS_DIR` as `'*.md'` files.
Any markdown document can contain metadata. Currently only the following fields are supported:

    <!---
    title: Article title
    date: 1970-01-01
    summary: One line description of an article.
    draft
    -->

The `${META}` separator is configured by default to `'-->'`. This allows using non-standard
markdown comments for the metadata container `'<!--- ... -->'` (note the three dash on comment
open). This separator can be redefined to anything else, this is only a suggestion.

Only markdown code written after this `${META}` separator is interpreted into `HTML`.

### title

The title is used in the site index as well as the atom feed. If no `title` field is defined
in metadata, the first occurence of the pattern `'^# '`is used as title instead. Only this
pattern is recognized, alternatives such as `'=====...'` underline are not.

### date

The date is used in the site index as well as the atom feed. The date should be given in a
format that can be understood by POSIX `'date -d'`. If no date is given, the source file
last modification time is used instead.

Dates are only shown in documents generated from markdown in the `${POSTS_DIR}` directory.

### summary

The summary is optional. It will be used in the atom feed if present.

### draft

An article can be marked as a draft. If will still be processed as markdown,
but it won't be listed by the indexes (`RSS` feed or directory indexes).

## Caveats

* Only titles of the form `'# Title'` are supported (no `'====...'` underline).

* If `'date: '` is not used, make sure you backup your files with the original attributes.

## TODO

 - Support tagging articles.
 - Support article series.
