[![MELPA](https://melpa.org/packages/fstar-mode-badge.svg)](https://melpa.org/#/fstar-mode) [![Build Status](https://travis-ci.org/FStarLang/fstar-mode.el.svg?branch=master)](https://travis-ci.org/FStarLang/fstar-mode.el)

## Use F* in Emacs!

![Screenshot](img/fstar-mode.png)

## Setup

### From MELPA

F*-mode requires Emacs 24.3 or newer, and is distributed through [MELPA](https://melpa.org). Add the following to your init file (usually `.emacs`), if it is not already there:

```elisp
(require 'package)
(add-to-list 'package-archives '("melpa" . "http://melpa.org/packages/") t)
(package-initialize)
```

Restart Emacs, and run <kbd>M-x package-refresh-contents</kbd>, then <kbd>M-x package-install RET fstar-mode RET</kbd>. Future updates can be downloaded using <kbd>M-x list-packages</kbd>.

If `fstar.exe` is not already in your path, set the `fstar-executable` variable:

```elisp
(setq-default fstar-executable "PATH-TO-FSTAR.EXE")
```

### From source

Setup MELPA as described above, then install the dependencies and clone the repo to a directory of your choice (`~.emacs.d/lisp/fstar-mode.el` for example). Optionally byte-compile `fstar-mode.el`, and finally add the following to your init file:

```elisp
(require 'fstar-mode "~/.emacs.d/lisp/fstar-mode.el/fstar-mode.el")
```

If you're using emacs on windows with cygwin, you should also install [windows-path.el](https://www.emacswiki.org/emacs/windows-path.el)

The short version is to do:

```
$ wget https://www.emacswiki.org/emacs/download/windows-path.el
```

Then, in your .emacs
```elisp
(require 'windows-path "<path to windows-path.el>")
(windows-path-activate)
```

## Using the interactive mode

Please be aware of the [current restrictions](https://github.com/FStarLang/FStar/wiki/Dealing-with-F%E2%98%85-dependencies#when-invoking-f-in-interactive-mode) on the interactive-mode.

The keybindings are as follows:


Emacs                | Atom  | Action
---------------------|-------|----------------------------------------------------------
C-c C-n              | C-S-n | Process the next block (terminated by two empty lines)
C-c C-u or C-c C-p   | C-S-p | Retract last block
C-c RET or C-c C-RET | C-S-i | Process the file up to the current point
C-c C-l              | C-S-l | Process the next block in lax mode
C-c C-x              | C-M-c | Kill the F* process

Use <kbd>M-x customize-variable RET fstar-interactive-keybinding-style RET</kbd> to pick a keybinding style. The default is Proof-General; the other option is Atom.

## Customization

### Enabling and disabling individual F* mode components

Use <kbd>M-x customize-variable RET fstar-enabled-modules RET</kbd> to choose which parts of fstar-mode to enable.

In particular, you can get real-time verification (instead of interactive, Proof-General style verification), by enabling the `flycheck` module. You'll need to install Flycheck from MELPA.

### Using F*-mode for `.fsi` files

Use the following snippet:

```elisp
(add-to-list 'auto-mode-alist '("\\.fsi\\'" . fstar-mode))
```

### Include non-standard libraries when using fstar-mode

Add the following line to your `.emacs`.

```
(setq fstar-subp-prover-args '("--include" "<your-path>"))
```

Note that under cygwin, `fstar` is a windows program and expects windows pathnames. For example, 
```
(setq fstar-subp-prover-args '("--include" "E:\\FStar\\ucontrib\\Platform\\fst")
```
('\\\\' is used to escape a Windows pathname.) 
See [fstar-subp-prover-args] for more documentation.

[fstar-subp-prover-args]: https://github.com/FStarLang/fstar-mode.el/blob/master/fstar-mode.el#L779

### Using F*'s bundled version of Z3

F* looks for Z3 in your system path.  To use the version of Z3 included in F* binary releases instead, add the following to your `.emacs`:

```
(advice-add 'fstar-subp-start-process :around #'fstar-subp-adjust-path)
```

## Font issues

### Missing characters

Boxes instead of math symbols are most likely due to missing fonts. DejaVu Sans Mono, [Symbola](http://shapecatcher.com/downloads/Symbola706.zip), FreeMono, STIX, Unifont, Segoe UI Symbol, Arial Unicode and Cambria Math are all good candidates. If Emacs doesn't pick up on the new fonts after a restart, the following snippet (add it to your .emacs) should help:

```elisp
(set-fontset-font t 'unicode (font-spec :name "YOUR USUAL EMACS FONT") nil 'append)
(set-fontset-font t 'unicode (font-spec :name "SOME FONT WITH GOOD COVERAGE AS LISTED ABOVE") nil 'append)
```

### Fonts for specific characters

Use the following snippet to use `Symbola` for `∀` only:

```elisp
(set-fontset-font t (cons ?∀ ?∀) "Symbola" nil 'prepend)
```
