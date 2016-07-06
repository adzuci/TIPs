# TIP 6: Go Lang

* **TIP**: 6
* **Title**: Go Lang
* **Author**: Bartek Ciszkowski, Jakub Labath
* **Status**: Merged and Approved
* **Created**: October 27, 2015


Go is increasingly becoming part of daily development at G Adventures, more-so
in specific teams. This style guide is a work in progress for best-practices,
editor, and environment setup.

## Version

The current Go version used by production systems is `1.6`. You should use [ansible-gapi](https://github.com/gadventures/ansible-gapi) to ensure you have the correct version of Go installed on your development machine.

You can download Go from the [official site](https://golang.org/dl/) or use your
favourite package manager for your operating system.

## Environment Setup

For reference, here's what a development environment looks like:

    cat ~/GO_ENV 
    export GOROOT=$HOME/go
    export GOPATH=$HOME/GO_LIBS
    export PATH=$GOROOT/bin:$GOPATH/bin:$PATH
    export PS1="(go) $PS1"

These settings can be loaded by adding `. ~/GO_ENV` to your `.bashrc`

**What these variables mean**:

* `GOROOT` is where your Go is installed (It should contain `bin/go`)
* `GOPATH` is where you download and install libraries when you use `go get`.  Can be any folder on your disk. This is optional but recommended.
* `PS1` just modifies your bash prompt - It's just a visual clue that tells you you're in a Go code environment.

**Git configuration**

When using `go get`, If you're presented with an error like:

    fatal: could not read Username for 'https://github.com': terminal prompts disabled
    
Try configuring git to force ssh:

    git config --global url.ssh://git@github.com/.insteadOf https://github.com/

## Editor Setup

### Vim

There are quite a few tools that can make writing Go much easier for you.

1. First of you will want to set the `GOPATH` as described above so that all your command line tools such as `goimports` or `oracle` have a place to be installed into.
2. Install [pathogen](https://github.com/tpope/vim-pathogen) good chance you already have this
3. Install [vim-go](https://github.com/fatih/vim-go) `git clone https://github.com/fatih/vim-go.git ~/.vim/bundle/vim-go`
3. Install the vim-go help in vim `:call pathogen#helptags()`
4. Load Go env `. ~/GO_ENV`
5. Start `vim /tmp/foo.go` and then type `:GoInstallBinaries` this will behind the scenes call `go get github.com/usefultool` for all the needed tools.

For help you can do `:h vim-go`

You can map the various commands, like `:GoImports` to whatever you want in Vim
using standard Vim mapping techniques.

## Style Guide

Although `go fmt` does much of the work, there are various conventions that we
should adhere to in order to ensure our code is consistent across projects. This
makes it easier to jump between projects, teams, and there's less brain drain
trying to learn someone's own unique convention.


#### Naming

Go programmers use `camelCase` when forming names by combining words. Thus, the
standard library has names like:

    QuoteRuneToASCII
    parseRequestLine

And one should stay away from `names_like_this`, with the exception of any
`SYMBOLIC_CONSTANTS`. There is no limit on name length. However, variables that
have a smaller scope should generally be shorter than variables with a larger
scope. For example, the index on an iteration should generally be named `i`
since its scope is minimized to the single loop, where-as a package level
variable name may better describe the package it's being used in, e.g.
`FahrenheitToCelcius`.

When speaking about variable scope, we should not get lazy with variable naming.
Just because a variable may be thrown away quickly and has a small scope, do not
use single-character names that are not meaningful, unless in the scope of an
iteration. A variable that contains a result should always be meaningful.

Any variable or type which should be exported begins with a capital letter. If
you do not expect the variable to be exported or used outside of your module, it
should begin with a lower case.


#### Functions

Functions with long signatures should be broken down into a new line for each
argument. This is important when the signature reaches past 80 characters in
line.

Thus, instead of:

```
func pipeline(ctx context.Context, inConn, outConn *amqp.Connection, initFn, cacheRefreshFn, routeFn splitFunc, logFn logFunc) (Canceller, error) {
```

You should write:

```
func pipeline(
    ctx context.Context,
    inConn,
    outConn *amqp.Connection,
    initFn,
    cacheRereshFn,
    routeFn splitFunc,
    logFn logFunc,
) (Canceller, error) {
```

#### Variables

One way to create variables is the `new` function. A variable created with `new`
is no different from an ordinary local variable whose address is taken, except
there's no need to create a dummy name. Thus, `new` is only syntactic
convenience, not a fundamental notion.

The `new` function is relatively rarely used because the most common unnamed
variables are of `struct` types, for which the `struct` literal syntax is more
flexible.

Thus, it's not recommended to use it.

#### Documentation

*Package Level*

The _doc comment_ is a summary of the package immediately preceding the
`package` declaration, like so:

    // Package tempconv converts Fahrenheit to Celcius
    package tempconv

Only one file in each package should have a doc comment in this style. Extensive
comments are conventionally placed in a file named `doc.go`.

Higher level documentation (install, setup, process) should be within a
`README.md` or a larger Sphinx-based documentation. That particular style is
outside of the scope of this document.

*Comments*

Commenting is important, and being clear and consistent with it makes the code
all the more readable. We suggest:

* Comments should have a space between the starting slashes and the first character (`// Foo` and not `//Foo`)
* Comments should not be on the same line as code, unless really short.
* Comments should not exceed 80 characters per line.


