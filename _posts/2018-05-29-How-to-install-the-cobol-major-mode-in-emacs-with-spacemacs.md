---
title: "How to install COBOL major mode in Emacs with Spacemacs"
layout: # single
author_profile: # true
read_time: # true
comments: # true
share: # true
related: #true
excerpt: "" # Auto-generated page excerpt is added to the overlay text or can be overridden here.
excerpt_separator: "<!--more-->"
header:
  overlay_color: "#000000" 
  overlay_filter: "0.7" # 1.0 Darkest
  overlay_image: "/assets/images/2018/05/Cobol-Punch-Card.jpg"
  image_description: "Cobol punch card"
  teaser: "/assets/images/2018/05/Cobol-Punch-Card.jpg"
categories:
  - HowTo
tags: 
  - cobol
  - emacs
  - spacemacs
  - mode
last_modified_at: 2018-05-29
---

COBOL (COmmon Business-Oriented Language") is a compiled English-like computer
programming language designed for business use. It is imperative, procedural
and, since 2002, object-oriented. COBOL is primarily used in business, finance,
and administrative systems for companies and governments. COBOL is still widely
used in legacy applications deployed on mainframe computers, such as large-scale
batch and transaction processing jobs.[^1]

Currently (2018), there is a large amount of software developed in COBOL that
supports a large number of important services for our society. However, there is
a great shortage of COBOL programmers. If you are looking for a good job as a
software developer, perhaps you should consider learning this old but widely
used programming language.

I learned COBOL programming in 1987, during my second year at the university.
Now, after 31 years, while many new programming languages are born continuously,
I could not imagine that I would be writing a publication on how to use Emacs to
write COBOL code !

On the other hand, if you are serious about computer science or engineering,
it's extremely important to become comfortable with the
[CLI](https://en.wikipedia.org/wiki/Command-line_interface). Using the command
line is like being able to talk to your computer, and offers unlimited
flexibility. The Command Line Interface gives you extreme flexibility to
actually tinker with the brain of your computer.

Within the CLI community (programmers, sysadmins, hackers, ...) text editors
like [vim](https://www.vim.org/) or [Emacs](https://www.gnu.org/software/emacs/)
are often used as main tools along other text based utilities, mainly in the
UNIX-like world.

In this framework, if you intend to use Emacs as a programming environment for
COBOL, you will have difficulty when trying to obtain information on the
Internet.

For beginners or simply to use the best of vim and Emacs I recommend
[Spacemacs](http://spacemacs.org/) which is a community-driven Emacs
distribution that allow us to use vim and Emacs commands and shortcuts.

The learning of Emacs and Spacemacs is not within the scope of this post.
Although the learning curve of Emacs is high, I strongly recommend it. You can
make a google search for
[tutorial](https://ontologicalblog.com/2016/10/14/an-absolute-beginners-guide-to-spacemacs-for-academic-writing/)s
or read some books.

## Installing COBOL major mode in Emacs with Spacemacs ##

Spacemacs divides its configuration into self-contained units called
configuration layers. 

A configuration layer is a collected unit of configuration that can be enabled
(or disabled) in Spacemacs. A layer typically brings together one or more
packages, as well as the glue configuration code required to make them play well
with each other and Spacemacs in general.[^2]

On the other hand, a package is a set of Emacs Lisp files that, taken together,
provide some feature. Packages may be available on a package repository, such as
ELPA or MELPA or on a third-party service provider (such as github) or even
locally on the disk.

Major modes specialize Emacs for editing or interacting with particular kinds of
text. Each buffer has exactly one major mode at a time which determines the
editing behavior of Emacs while that buffer is current. Major modes are
installed on Emacs as other packages.

Our goal is to use a major mode to write COBOL code in Emacs, so we must
install a package that provide us with that functionality.

Spacemacs includes a good set of configuration layer for the most common
packages. These layers are stacked on top of each other to achieve a custom
configuration. By default Spacemacs uses a dotfile called `.spacemacs` to
control which layers to load. Within this file you can also configure certain
features. However there are packages not included as configuration layers. That
is the case of the COBOL major mode.

To install packages not included as configuration layer edit `.spacemacs` file,
go to `dotspacemacs-additional-packages` and include your packages separated by
commas. In the case of the COBOL major mode: 

``` emacs-lisp
;; List of additional packages that will be installed without being
;; wrapped in a layer. If you need some configuration for these
;; packages, then consider creating a layer. You can also put the
;; configuration in `dotspacemacs/user-config'.
dotspacemacs-additional-packages '(cobol-mode)
```

To associate file extensions to a major mode so that Emacs automatically adopts
that mode when you open this type of files edit `.spacemacs`, go to
`dotspacemacs/user-config ()` section and add your associations as follows:

```emacs-lisp
(defun dotspacemacs/user-config ()
  "Configuration function for user code.
This function is called at the very end of Spacemacs initialization after
layers configuration.
This is the place where most of your configurations should be done. Unless it is
explicitly specified that a variable should be set before a package is loaded,
you should place your code here."
  
;;; Other stuff

;;; COBOL
  (add-to-list 'auto-mode-alist '("\\.cob" . cobol-mode))

  )
```

An alternative that does not depend on the file extension consists in is to
include the mode as an intrinsic major mode at the beginning of the COBOL file:

``` cobol
      -*-cobol-*-
      *Simple Hello World
       IDENTIFICATION DIVISION.
       PROGRAM-ID. HELLO.
       PROCEDURE DIVISION.
           DISPLAY "Hello world".
           STOP RUN.
```

Finally press `<SPC f e r>` (Vim style) or `<M-m f e r` (Emacs style) to reload
`.spacemacs` and install the new package without rebooting Emacs.




[^1]: https://en.wikipedia.org/wiki/COBOL

[^2]: https://github.com/syl20bnr/spacemacs/blob/master/doc/LAYERS.org
