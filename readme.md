## Overview

Polymode is an emacs package that offers generic support for multiple major
modes inside a single emacs buffer. It is lightweight, object oriented and
highly extensible. Creating new polymodes commonly takes just several lines of
code.

Polymode also provides extensible facilities for external literate programming
tools such as exporting, weaving and tangling.

- [High-level view](#high-level-view)
- [Installation](#installation)
- [Polymodes Activation](#activation-of-polymodes)
- [Basic Usage](#basic-usage)
- [Warnings](#warning)
- [Development](modes)
- [Screenshots](#Screenshots)

## High-level view

`polymode` is a tool that enables multiple different Emacs modes to
apply to different substrings (positions? spans?) of a single Emacs
buffer.  Some features of polymode include:

  - Each location within the buffer is associated with a *polymode*.
    The polymode mimics a standard Emacs major mode such as
    `tex-mode`, `text-mode`, `c-mode`, `scheme-mode`, and so on.
    The intended effect is that each location within the buffer
    behaves as if it has its own mode.

  - A polymode is sensitive to the values of Emacs variables in the
    same way as the major mode that it mimics.  [NOTE: THERE SHOULD BE
    SOME VOCABULARY---A NAME--- FOR "THE MAJOR MODE A POLYMODE MIMICS."]

  - When multiple locations in the buffer are associated with the same
    polymode, those locations share values of all variables, including
    buffer-local variables, of that polymode.

  - At all times, `polymode` makes sure that Emacs behaves "as if" it
    is in the major mode that mimics the polymode associated with
    `point`.  In particular, at all times, the keybindings that are
    active are the ones defined by the major mode that is mimicked by
    the polymode that is associated with point. [UGH]

  - `polymode` interoperates with `font-lock-mode` so that each part
    of the visible window is syntax-highlighted according to the
    conventions of the polymode associated with that part of the
    window (which are the conventions of the major mode that the
    polymode mimics).

  - **How does `polymode` know which polymode to associate with a
    location?**  (THERE IS SOME SORT OF PARSING GOING ON.)

  - **How (and how often) is this information updated?**
      
    


## Installation

*_Polymode does't work in emacs devel. Jit-lock support for indirect buffers was
 recenly removed. I am looking for workarounds._*

### From [MELPA](https://github.com/milkypostman/melpa)

<kbd>M-x</kbd> `package-install` `polymode`.

### Manually for Development

```sh
git clone https://github.com/vitoshka/polymode.git
```

Add "polymode" directory and "polymode/modes" to your emacs path:

```lisp 
(setq load-path
      (append '("path/to/polymode/"  "path/to/polymode/modes")
              load-path))
```

Require any polymode bundles that you are interested in. For example:

```lisp
(require 'poly-R)
(require 'poly-markdown)
```

## Activation of Polymodes

Polymodes are functions just like ordinary emacs modes. The can be used in place
of emacs major or minor modes alike. There are two main ways to automatically
activate  emacs (poly)modes:

   1. _By registering a file extension by adding modes to `auto-mode-alist`_:

    ```lisp
    ;;; MARKDOWN
    (add-to-list 'auto-mode-alist '("\\.md" . poly-markdown-mode))

    ;;; R modes
    (add-to-list 'auto-mode-alist '("\\.Snw" . poly-noweb+r-mode))
    (add-to-list 'auto-mode-alist '("\\.Rnw" . poly-noweb+r-mode))
    (add-to-list 'auto-mode-alist '("\\.Rmd" . poly-markdown+r-mode))
    ```

    See [polymode-configuration.el](polymode-configuration.el) for more
    examples.

2. _By setting local mode variable in you file_:
   
   ```c++
   // -*- mode: poly-C++R -*-
   ```
   or 
   ```sh
   ## -*- mode: poly-brew+R; -*-
   ```


## Basic Usage

All polymode keys start with the prefix defined by `polymode-prefix-key`,
default is <kbd>M-n</kbd>. The `polymode-mode-map` is the parent of all
polymodes' maps:

* BACKENDS

     <kbd>e</kbd> `polymode-export`

     <kbd>E</kbd> `polymode-set-exporter`

     <kbd>w</kbd> `polymode-weave`

     <kbd>W</kbd> `polymode-set-weaver`

     <kbd>t</kbd> `polymode-tangle` ;; not implemented yet

     <kbd>T</kbd> `polymode-set-tangler` ;; not implemented yet

     <kbd>$</kbd> `polymode-show-process-buffer`

* NAVIGATION

    <kbd>C-n</kbd> `polymode-next-chunk`
     
    <kbd>C-p</kbd> `polymode-previous-chunk`
     
    <kbd>C-M-n</kbd> `polymode-next-chunk-same-type`
     
    <kbd>C-M-p</kbd> `polymode-previous-chunk-same-type`

* MANIPULATION

    <kbd>M-k</kbd> `polymode-kill-chunk`

    <kbd>M-m</kbd> `polymode-mark-or-extend-chunk`

    <kbd>C-t</kbd> `polymode-toggle-chunk-narrowing`

    <kbd>M-i</kbd> `polymode-insert-new-chunk`


## Warnings

  * Tested with Emacs 24.3.1
  * _Does not work in emacs devel._

Some things still don't work as expected. For example:
    
   * To kill a polymode buffer you will have position the cursor in the base mode buffer. 
   * Customization interface is not working as expected (an eieio bug) and is
     not tested. 
   * Indentation and font-lock is not always right and requires some more
     tweaking. This is especially true for complex modes like `c-mode`.

## Developing with Polymode

See [development](modes).

## Screenshots

### markdown+R

<img src="img/Rmd.png" width="400px"/>

### markdown+R+YAML

<img src="img/rapport.png" width="400px"/>

### org mode

<img src="img/org.png" width="400px"/>

### Ess-help buffer

<img src="img/ess-help.png" width="400px"/>

### C++R
<img src="img/cppR.png" width="400px"/>

