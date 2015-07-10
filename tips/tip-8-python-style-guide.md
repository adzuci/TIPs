# TIP 8: Python Style Guide

* **TIP**: 8
* **Title**:  Python Style Guide
* **Author**: Bartek Ciszkowski
* **Status**: Draft
* **Created**: July 9th, 2015
* **Updates**: N/A

A style guide? But don't we have [PEP 8](https://www.python.org/dev/peps/pep-0008/)? Yes, we do! But this document will discuss

* Recommended PEPs to ignore
* Integration of PEP Linters within your editor.
* Updating legacy code to PEP 8

Why should everyone follow PEP 8? Because when everyone writes code the same,
projects become more accessible, the barrier to entry is lowered, and it
creates a better sense of community within our code base. The less we can silo,
the better.

## Recommended PEPs to ignore

* **E501**: Line too long. By default, PEP 8 suggests lines max out at 80
    characters. We're in the 21st century, so we can spare more space. The
    suggested line length is 90 characters.
* **E121**: Wrong hanging indentation. This has historically felt very finnicky
    in editors and unnecessarly loud.

## Integration with your editor

In this guide, we'll focus on two editors. Atom, and Vim. Please feel free to
edit this document to add your favourite editor.

### Atom

The most active linter seems to be [linter-pep8](https://github.com/AtomLinter/linter-pep8).

You'll want to update `maxLineLength` to be set to 90, as proposed in the `E501`
rule above.

### Vim

The recommended linter for Vim is without a doubt,
[Syntastic](https://github.com/scrooloose/syntastic). Installation works with
Pathogen, and is easy to configure. Once installed, add these to your `.vimrc`
to be setup for Python editing!

    let g:syntastic_python_checkers=['flake8']
    let g:syntastic_python_flake8_args="--ignore=E501,E121" 

## Updating legacy code to PEP 8

Any code you touch should be updated to PEP 8. Although this is rarer today, it
ensures we keep our code base clean for the future.

When you are updating legacy code, it is best to commit all PEP 8 changes in a
separate commit. 

For example, if you are fixing a bug, first, you'll commit the bug fix. Then,
you will have a second commit containing your PEP 8 changes.

The title of said commit can simply be `PEP 8`
