Markdown Document For Printing
==================================================

For quick printed documents like letters, I like to write in Markdown
and convert to PDF with [Pandoc](https://pandoc.org/).

Pandoc lets you put a YAML block at the beginning of the document with
variables that affect the paper size, margins, PDF metadata, etc.
I always forget what those options are, so I wanted to have a starter document
that I could copy and edit at will.

To use:

1. Copy the document `doc.markdown`
2. Edit it
3. Convert to PDF with Pandoc: `pandoc doc.markdown -o doc.pdf`

Give yourself a Vim macro:

    :nmap <Buffer> <Leader>p :w \| :!pandoc -o %.pdf %<CR>

For more complicated documents, I also have a project template for a LaTeX article:
<https://github.com/sleepymurph/template_latex_article>


Notes
--------------------------------------------------

### A4 paper

Note that this template insists on A4 paper because I live in Europe.
This is set by the `papersize: a4` in the YAML header.
Delete or change that as needed.

If you do change it, note that the Pandoc LaTeX template concatenates `paper` to the `papersize` setting.
So `papersize: a4` becomes `\documentclass[a4paper]{article}` in the LaTeX.
So whatever paper size you use, you need to leave the `paper` part off of the
`papersize` option.

Remembering exactly how to set this is probably the biggest reason I
need this template.

### Dependency: Open Sans

[Open Sans](https://fonts.google.com/specimen/Open+Sans)
is the official font at the university where I work.
Therefore it is the default font for my templates.

This means that this template requires the
[`opensans` package for LaTeX](https://ctan.org/pkg/opensans).
On Ubuntu this package is part of the
[`texlive-fonts-extra` package](https://packages.ubuntu.com/xenial/texlive-fonts-extra).

    apt install texlive-fonts-extra

Or you can use a different font by editing the file.
Edit or remove the `fontfamily` and `fontfamilyoptions`
values in the YAML block.

#### Trade-off: Open Sans vs full UTF-8 support

This method of switching to Open Sans only works with the default `pdflatex` engine.
And the `pdflatex` engine has impartial UTF-8 support.
The Pandoc LaTeX template (and my LaTeX template) use the `inputenc` package for partial support.
This works for most of my purposes, but it can still give errors like the following:

```
! Package inputenc Error: Unicode char А (U+410)
(inputenc)                not set up for use with LaTeX.

See the inputenc package documentation for explanation.
Type  H <return>  for immediate help.
 ...                                              
                                                  
l.66 ...YZŽabcčćdđefghijklmnopqrsštuvwxyzžА

Try running pandoc with --latex-engine=xelatex.
pandoc: Error producing PDF

shell returned 43
```

If you switch to `xelatex` for better UTF-8 support,
the document will no longer be in Open Sans:

    pandoc doc.markdown -o doc.markdown.pdf --latex-engine=xelatex

Apparently fonts work completely differently in XeLaTeX,
and I have not had a chance to figure that out.

TODO: Figure out how to get Open Sans with `xelatex` too.


### More options

Options to set can be found by looking at Pandoc's LaTeX template.

On my Ubuntu system, that lives at
[`/usr/share/pandoc/data/templates/default.latex`](file:///usr/share/pandoc/data/templates/default.latex)

Look for directives that are surrounded by dollar signs:

    $if(geometry)$
    \usepackage[$for(geometry)$$geometry$$sep$,$endfor$]{geometry}
    $endif$

Here we see `geometry` used in a loop.
So it would seem that we can create an array in the YAML with an array of configuration options for the geometry package.
