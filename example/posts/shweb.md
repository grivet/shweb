# Shweb

Static blog generator, written in POSIX shell.

## Quick start

A markdown-to-HTML command must be provided. This example configuration
assumes `'/usr/bin/markdown'` is usable. You can install [discount] for example.

    $ # Local installation
    $ make install DESTDIR=~/bin
    $ shweb ./example
    $ cd /tmp/shweb
    $ python -m http.server 8000

The website is then available [locally](http://0.0.0.0:8000/).

[discount]: https://github.com/Orc/discount

## Installation

    $ make install DESTDIR=~bin # DESTDIR is memorized
    $ make uninstall # Uses DESTDIR previously defined
    $ make install # Uses DESTDIR previously defined

After some edits:

    $ make check # Run shellcheck

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

    # Path to an about page in output.
    # /about.html can be created by writing /about.md in source dir.
    # Link to about page will be omitted if this variable is set to "".
    ABOUT_PAGE="/about.html"

    # Path to the generated RSS feed.
    # Skipped if this variable is set to "".
    RSS="/feed.xml"

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

## Redaction

Write your articles in `POSTS_DIR` as `'*.md'` files.

A macro `'DATE='` is available to force a publication date. If not present,
the last modification date is used instead. Any format understood by `'date -d'` can be used.

    DATE=1970-01-01

      or e.g.

    DATE=Wed 03 Jun 2020 09:54:08 PM UTC

The macro `'DESC='` can be used to provide a description of the article.
This description will appear on the homepage list and in the `RSS` feed.

    DESC=One line description of an article.

## Caveats

* Only titles of the form `'# Title'` are supported (no `'====...'` underline).

* If `'DATE='` is not used, make sure you backup your files with the original attributes.

## TODO

 - [x] Use a date field in articles instead of last modification time.
 - [x] Sort articles per date.
 - [x] About page.
 - [x] RSS feed.
 - [ ] Allow undated about page.
 - [ ] Favicon.
 - [ ] deploy static assets (images, scripts, etc).
 - [ ] limit feed length to N last items.
