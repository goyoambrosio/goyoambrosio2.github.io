---
title: "Julia: a new direction in scientific computing"
related : true
categories:
  - Info
tags:
  - data science
  - HPC
  - IJulia
  - Julia
  - Juno
  - Jupyter
  - programming language
  - scientific computing
---

![Julia](/assets/images/2016/02/Julia-logo.png){: .align-left}[Julia](https://github.com/JuliaLang/julia) is a high-level, high-performance dynamic programming language for technical computing, with syntax that is familiar to users of other technical computing environments. It came from math, science, and engineering MIT students who wanted a fast, practical language to replace C and Fortran designed to give you answers quickly. It provides a sophisticated compiler, distributed parallel execution, numerical accuracy, and an extensive mathematical function library.

As a new programming language, it lacks of the tons of libraries other older and extensively used languages have but the Julia language makes it [unusually easy](http://docs.julialang.org/en/release-0.2/manual/calling-c-and-fortran-code/) to interface with existing C libraries. Julia’s Base library, largely written in Julia itself, also integrates mature, best-of-breed open source C and Fortran libraries for linear algebra, random number generation, signal processing, and string processing. Julia makes it simple to call external functions in C and Fortran shared libraries, without writing any wrapper code or even recompiling existing code. You can try calling external library functions directly from Julia’s interactive prompt, getting immediate feedback.

Julia is a free alternative to proprietary tools for doing data science, like [MATLAB](http://www.mathworks.com/) and [Mathematica](https://www.wolfram.com/mathematica/), and it’s more contemporary than open-source languages [R](https://www.r-project.org/) and [Python](https://www.python.org/). More companies are hiring data scientists to make more data-driven decisions, and open-source tools often come in handy, so I expect an exponential growth for Julia Language in the coming years.

The core of the Julia implementation is licensed under the MIT license. Various libraries used by the Julia environment include their own licenses such as the GPL, LGPL, and BSD (therefore the environment, which consists of the language, user interfaces, and libraries, is under the GPL).

Other resources:
- [The Julia manual](http://julia.readthedocs.org/en/latest/manual/)
- [Learning page](http://julialang.org/learning/) from Julia
- [Hands-on Julia](Hands-on Julia): A hands-on tutorial covering the same ground as a series of exercises
- [Julia users Google group](https://groups.google.com/forum/#!forum/julia-users)

Now, if I have managed to attract your interest you can follow these simple instructions to start using Julia.

## Installing Julia

The simplest way to install Julia is with a binary install. You must [download](http://julialang.org/downloads/) the installer for your operating system. If you use Windows, you probably want to also install [git for Windows](https://git-for-windows.github.io/), which provides a proper Unix-like shell.

## Running Julia

You have some ways to run Julia:
- In the terminal using the built-in Julia command line.![Julia splash screen](/assets/images/2016/02/julia.png){: .align-center}
- With the [Juno](http://junolab.org/) integrated development environment (IDE).![Juno](/assets/images/2016/02/Juno.png){: .align-center}
- In the browser on [JuliaBox.org](http://juliabox.org/) with IJulia notebooks.![JuliaBox](/assets/images/2016/02/JuliaBox.png){: .align-center}
- Through [IJulia](https://github.com/JuliaLang/IJulia.jl), the Julia interface for the [Jupyter](http://jupyter.org/) interactive environment also used by [IPython](http://ipython.org/). For this option, the best method is to install [Anaconda](https://www.continuum.io/downloads), the free Python distribution, which includes Jupyter. The run Julia application and type `Pkg.add("IJulia")` at prompt.![IJulia](/assets/images/2016/02/IJulia.png){: .align-center}

## What next?

And finally I'd like to suggest you the [David P. Sanders' Invitation to Julia for scientific computing](https://github.com/dpsanders/invitation_to_julia): an introductory workshop on Julia at JuliaCon 2015.
