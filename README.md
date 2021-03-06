Stapler
=======

Stapler is a pure Python alternative to [PDFtk][pdftk], a tool for manipulating
PDF documents from the command line.

[pdftk]: http://www.pdfhacks.com/pdftk/

History
-------
PDFtk was written in Java and C++, and
is natively compiled with gcj. Sadly, it has been discontinued a few years ago
and bitrot is setting in (e.g., it does not compile easily on a number of
platforms).

Philip Stark decided to look for an alternative and found pypdf, a PDF library
written in pure Python. He couldn't find a tool which actually used the
library, so he started writing his own.

This version of stapler is Fred Wenzel's fork of the project, with a completely
refactored source code, tests, and added functionality.

Like pdftk, stapler is a command-line tool. If you would like to add a GUI,
compile it into a binary for your favorite platform, or contribute anything else,
feel free to fork and send me a pull request.

License
-------
Stapler version 0.2 was written in 2009 by Philip Stark.
Stapler version 0.3 was written in 2010 by Fred Wenzel.

For a list of contributors, check the ``CONTRIBUTORS`` file.

Stapler is distributed under a BSD license. A copy of the BSD Style
License used can be found in the file ``LICENSE``.

Usage
-----
There are the following modes in Stapler:

### select/delete (called with ``sel`` and ``del``, respectively)
With select, you can cherry-pick pages from pdfs and concatenate them into
a new pdf file.

Syntax:

    stapler sel input1 page_or_range [page_or_range ...] [input2 p_o_r ...]

Examples:

    # concatenate a and b into output.pdf
    stapler sel a.pdf b.pdf output.pdf

    # generate a pdf file called output.pdf with the following pages:
    # 1, 4-8, 20-40 from a.pdf, 1-5 from b.pdf in this order
    stapler sel a.pdf 1 4-8 20-40 b.pdf 1-5 output.pdf

    # reverse some of the pages in a.pdf by specifying a negative range
    stapler sel a.pdf 1-3 9-6 10 output.pdf

The delete command works almost exactly the same as select, but inverse.
It uses the pages and ranges which you _didn't_ specify.

### split/burst:
Splits the specified pdf files into their single pages and writes each page
into it's own pdf file with this naming scheme:

    ${origname}_${zero-padded page no}.pdf

Syntax:

    stapler split input1 [input2 input3 ...]

Example for a file foobar.pdf with 20 pages:

    $ stapler split foobar.pdf
    $ ls
        foobar_01.pdf foobar_02.pdf ... foobar_19.pdf foobar_20.pdf

Multiple files can be specified, they will be processed as if you called
single instances of stapler.

### zip:
With zip, you can cherry-pick pages from pdfs (like select). The pages from
each pdf are merged together in an interleaving manner. This can be used to
collate a pdf with odd pages and a pdf with even pages into a single file.

Syntax:
    stapler zip input1 [range[rotation]] [range ...] [input2 [range...] ...] out

Examples:

    # combine a pdf with odd pages and a pdf with even pages into output.pdf
    stapler zip odd.pdf even.pdf output.pdf

    # combine a.pdf b.pdf and c.pdf, but use only some pages of c.pdf
    stapler zip a.pdf b.pdf c.pdf 1-3 output.pdf

If one of the ranges is shorter than the others, stapler will continue to merge
the remaining pages.


### info:
Shows information on the metadata stored inside a PDF file.

Syntax:

    stapler info foo.pdf

Example output:
    *** Metadata for foo.pdf

    /ModDate:  D:20100313082451+01'00'
    /CreationDate:  D:20100313082451+01'00'
    /Producer:  GPL Ghostscript 8.70
    /Title:  foo.pdf
    /Creator:  PDFCreator Version 0.9.9
    /Keywords:
    /Author:  John Doe
    /Subject:
