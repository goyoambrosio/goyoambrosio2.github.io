---
title: "Dealing with UTF-8 in Emacs"
layout: # single
author_profile: # true
read_time: # true
comments: # true
share: # true
related: #true
excerpt: # Auto-generated page excerpt is added to the overlay text or can be overridden here.
excerpt_separator: # "<!--more-->"
header:
  overlay_color: "#000000" 
  overlay_filter: "0.7" # 1.0 Darkest
  overlay_image: "/assets/images/2018/06/simpson-utf-8.jpg"
  caption: "Photo credit: [**perl.com**](http://perl.com)"
  image_description: "Simpson studies UTF-8"
  teaser: "/assets/images/2018/06/simpson-utf-8.jpg"
  og_image: "/assets/images/2018/06/simpson-utf-8.jpg" # OpenGraph override.
categories:
  - Tutorial
tags: 
  - emacs 
  - utf-8
  - encoding
last_modified_at: 2018-06-07
---

Emacs has facilities in place for changing the coding system for a variety of
things, such as processes, buffers and files. 

You can also force Emacs to invoke a command with a certain coding system.

The easy way to set the encoding of a file in Emacs consists in setting a file
local variable in the first line, e.g.:

```markdown
-*- mode: markdown; coding: utf-8 -*-
```

Specifically, there are various types of utf-8 in Emacs that you can set with
the last part of the encoding name:

To describe the special character that will be used at the end of lines:

- `-mac` : `CR (0D)` the standard line delimiter with MacOS until OS X.

- `-unix`: `LF (0A)` the standard delimiter for Unix systems so the BSD-based Mac OS X.

- `-dos` : `CR+LF (0D 0A)` the delimiter for DOS / Windows.

some additional encodings parameters include:

- `-emacs`         : support for encoding all Emacs characters (including non Unicode)

- `-with-signature`: force the usage of the BOM (see next section)

- `-auto`          : autodetect the BOM

You can combine the different possibilities through the mode name as in the following example:

``` markdown
-*- coding: utf-8-with-signature-unix -*-
```

## Byte Order Mark (BOM) ##

Byte Order Mark is a special signature defined by UTF standard and placed at the
beginning of text files to specify if it is UTF encoded. Basically there are three
possible encodings:

- `utf-16` encoding stores the characters with 2 bytes. Depending on endianess
  there are two possibilities. If the system places the *most significant byte*
  first, we have a big-endian system and the encoding mode is named as
  `utf-16be`. Otherwise, if the system places the *least significant byte*
  first, we have a little-endian system and the encoding mode is named as
  `utf-16le`.
  
- `utf-8` encodes each character with a single byte except the extended
  characters greater than 127. For that case it uses a special sequence of
  bytes. In principle, in this encoding mode the signature does not make sense
  but it will be useful to detect an `utf-8` file without parsing the whole file
  until finding an extended character. The signature tells instantly to the
  system that the file is not an ascii plain text. However Emacs is very
  efficient to make such auto-detection when there's no signature available.

The very first bytes of a text file used as BOM signature are:

 | encoding   | hex bytes  |
 |------------|------------|
 | `utf-16be` | `FE FF`    |
 | `utf-16le` | `FF FE`    |
 | `utf-8`    | `EF BB BF` |

To open a file without any conversion, execute `find-file-literally` function in
Emacs. If the first line begins with `ï»¿` you will be watching the undecoded
`utf-8` BOM.

To get some information on type of line ending, BOMs and character sets provided
by encodings, you can use `describe-coding-system`, or `C-h C`.

Changing the coding variable and saving the file will make Emacs to convert the
encoding of the file.

Emacs will respect whatever encoding an existing file uses, but you can set a default
encoding mode when it has an unsetted mode, like when you create a new file:

``` emacs-lisp
(prefer-coding-system 'utf-8-unix)
```

You may also ask Emacs to use a specific Unicode friendly font:

``` emacs-lisp
(set-face-attribute 'default nil
                    :family "Source Code Pro"
                    :height 100
                    :weight 'normal
                    :width 'normal)
```

To treat files as UTF-8 by default, when no information appears explicitly in
the file, you could set some variables to enforce UTF-8 as the default encoding
for all files, processes and buffers:

``` emacs-lisp
(prefer-coding-system 'utf-8)
(set-default-coding-systems 'utf-8)
(set-terminal-coding-system 'utf-8)
(set-keyboard-coding-system 'utf-8)
(set-selection-coding-system 'utf-8)
(set-file-name-coding-system 'utf-8)
(set-clipboard-coding-system 'utf-8)
(set-w32-system-coding-system 'utf-8)
(set-buffer-file-coding-system 'utf-8) 
```

**Important**, you could always try `hexl-mode` to see how exactly the file is stored.

## Things you can do in Linux ##

In Linux you can check the encoding of a file using the `file` command:

``` shell
$ file test.md
test.md: UTF-8 Unicode text
$ file test.py
test.py: ASCII text, with CRLF line terminators

$ file -i test.md
test.md: text/plain; charset=utf-8
$ file -i test.py
test.py: text/plain; charset=us-ascii
```

To convert text from an encoding mode to another in Linux you can use the `iconv` command:

``` shell
$ iconv option
$ iconv options -f from-encoding -t to-encoding inputfile(s) -o outputfile 
```

For instance:

``` shell
$ iconv -f ISO-8859-1 -t UTF-8 input.file -o out.file
```

Where `-f` or `--from-code` means input encoding and `-t` or `--to-encoding`
specifies output encoding.

To list all known coded character sets, run the command below:

``` shell
$ iconv -l
```

## More info ##

[UTF-8 for the impatient](http://www.skybert.net/craftsmanship/utf-8-for-the-impatient/)

[Working with Coding Systems and Unicode in Emacs](https://www.masteringemacs.org/article/working-coding-systems-unicode-emacs)

[UTF-8 and Unicode FAQ for Unix/Linux](http://www.cl.cam.ac.uk/~mgk25/unicode.html)

[Convert files to utf-8 encoding in Linux](https://www.tecmint.com/convert-files-to-utf-8-encoding-in-linux/)
