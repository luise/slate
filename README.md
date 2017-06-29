# Generating Docuementation with Slate

This repository contains the code necessary to compile Quilt's
documentation, and is based on [Slate](https://github.com/lord/slate).
If you got here because you're
looking for documentation about Quilt, head over to
[docs.quilt.io](http://docs.quilt.io), which contains the Quilt docs,
compiled using the code here. If you're looking for the Markdown files
that hold the contents of the documentation, head to the
[docs folder of the Quilt repository](https://github.com/quilt/quilt/tree/master/docs).

This page describes how Slate works, how to install Slate, and how
to compile the Quilt documentation. 

## Slate and directory layout

Slate compiles markdown files to HTML using a Ruby application called
[Middleman](https://middlemanapp.com/).
Slate will generate one output HTML page for each file in the
[source](source) directory that has suffix `.html.md`. These
files are converted from Markdown to HTML using the layout defined 
in [layout.erb](source/layouts/layout.erb). To change the contents
of the documentation, change the Markdown files located in the Quilt
repository; to change the way the
HTML gets generated (e.g., to add an image at the top of the table of
contents), change `layout.erb` in this repository.

Files with suffix `.html.md` can include other markdown files by adding
an `includes` section at top.  For example, the following file:

```
---
title: My documentation
includes:
  - MySubFile
---
This is some introductory content.
```

would generate a HTML page with title "My documentation", and that contains
"This is some introductory content", followed by all of the content in
`source/_MySubFile.md` (note that the filename is the name listed under
"includes", with an added underscore prefix and ".md" suffix; this format
is dictated by Ruby).

## Generating the website

### One-time setup

If you've never generated the website before, you'll need to install
Ruby and bundler (which is Ruby's dependency management tool).  To do
this using Homebrew:

```console
$ brew install ruby
$ gem install bundler # gem is Ruby's equivalent of make.
```

Next, clone this repository, and use bundler to install Slate and fetch
the necessary dependencies:

```console
$ git clone https://github.com/quilt/slate.git
$ cd slate
$ bundle install
```

At this point, everything necessary to create the website has been
installed.

### Re-generating the HTML files

To generate the compiled HTML files, run:

```console
$ bundle exec middleman build --clean
```

This command will place all of the compiled files in the
`build` folder. For each file `source/foo.html.md`, there will
be a corresponding file `build/foo.html` that contains the compiled
HTML.

### Generating the Quilt docs

To generate the Quilt docs, you'll need to copy the markdown files
from the main [Quilt repository](https://github.com/quilt/quilt/tree/master/docs)
into the source folder here:

```console
$ cp <path to Quilt>/docs/*md source/
```

And then run the `bundle` command above. Alternately, you can run
`make docs` from the [Quilt repository](https://github.com/quilt).

