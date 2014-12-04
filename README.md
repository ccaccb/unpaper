unpaper-ffmpeg
==============

Originally written by Jens Gulden — see AUTHORS for more information.
Forked from Diego Elio Pettenò[5], who maintains a current version of unpaper.
Licensed under GNU GPL v2 — see COPYING for more information.

This is a unpaper fork for ffmpeg compability. There seem to be inconsistent/
incompatible APIs for libav and ffmpeg and until there is a way to be agnostic
to libav vs. ffmpeg I'll maintain this fork.

Overview
--------

`unpaper` is a post-processing tool for scanned sheets of paper,
especially for book pages that have been scanned from previously
created photocopies.  The main purpose is to make scanned book pages
better readable on screen after conversion to PDF. Additionally,
`unpaper` might be useful to enhance the quality of scanned pages
before performing optical character recognition (OCR).

`unpaper` tries to clean scanned images by removing dark edges that
appeared through scanning or copying on areas outside the actual page
content (e.g.  dark areas between the left-hand-side and the
right-hand-side of a double- sided book-page scan).

The program also tries to detect misaligned centering and rotation of
pages and will automatically straighten each page by rotating it to
the correct angle. This process is called "deskewing".

Note that the automatic processing will sometimes fail. It is always a
good idea to manually control the results of unpaper and adjust the
parameter settings according to the requirements of the input. Each
processing step can also be disabled individually for each sheet.

See [further documentation][3] for the supported file formats notes.

Dependencies
------------

The only hard dependency of `unpaper` is [ffmpeg][4], which is used for
file input and output.

I have succesfully tested unpaper-ffmpeg with
libavutil:	54. 7.100
libavcodec: 	55. 1.100
libavformat:	56. 4.101
If there is a problem with a particular version of ffmpeg please
create an issus.

The manpage is build using docbook[6].
For faster and offline building a local copy of docbook should be used.
It can be optained from http://sourceforge.net/projects/docbook.
Otherwise the first line of Makefile.am contains a line which
uses the current online version of docbook. Uncomment it to use that
(requires working network and is slower).

Building instructions
---------------------

`unpaper` uses GNU Autotools for its build system, so you should be
able to execute the same commands used for other software packages.
To create the initially used files it might be nessecary to call:

    aclocal
    automake --add-missing
    autoconf

and then:

    ./configure
    make
    sudo make install

There are, though, some recommendations about the way you build the
code. Since the tasks are calculation-intensive, it is important to
build with optimizations turned on:

    ./configure CFLAGS="-O2 -march-native -pipe"

Even better, if your compiler supports it, is to use Link-Time
Optimizations, as that has shown that execution time can improve
sensibly:

    ./configure CFLAGS="-O2 -march=native -pipe -flto"

Further optimizations such as `-ftracer` and `-ftree-vectorize` are
thought to work, but their effect has not been evaluated so your
mileage may vary.

Further Information
-------------------

You can find more information on the [basic concepts][1] and the
[image processing][2] in the available documentation.

[1]: doc/basic-concepts.md
[2]: doc/image-processing.md
[3]: doc/file-formats.md
[4]: https://ffmpeg.org/
[5]: https://www.flameeyes.eu/projects/unpaper
[6]: http://docbook.sourceforge.net/
