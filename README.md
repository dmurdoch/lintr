# lintr
[![Travis-CI Build Status](https://travis-ci.org/jimhester/lintr.svg?branch=master)](https://travis-ci.org/jimhester/lintr)
[![codecov.io](http://codecov.io/github/jimhester/lintr/coverage.svg?branch=master)](http://codecov.io/github/jimhester/lintr?branch=master)
[![CRAN_Status_Badge](http://www.r-pkg.org/badges/version/lintr)](https://cran.r-project.org/package=lintr)

## Static code analysis for R ##
![lintr](http://i.imgur.com/acV27NV.gif "lintr")

### RStudio ###
lintr lints are automatically displayed in the RStudio Marker pane, Rstudio versions (> v0.99.206).
![RStudio Example](http://i.imgur.com/PIKnpbn.png "Rstudio Example")

### Emacs ###
lintr has [built-in integration](http://www.flycheck.org/en/latest/languages.html#r) with [flycheck](https://github.com/flycheck/flycheck) versions greater than `0.23`.
![Emacs Example](http://i.imgur.com/vquPht3.gif "Emacs Example")

#### Installation ####
lintr is fully integrated into flycheck when using [ESS](http://ess.r-project.org/).  See the
installalation documentation for those packages for more information.

#### Configuration ####
You can also configure what linters are used. e.g. using a different line length cutoff.
- `M-x customize-option` -> `flycheck-lintr-linters` -> `with_defaults(line_length_linter(120))`

### Vim
lintr can be integrated with
[syntastic](https://github.com/scrooloose/syntastic) for on the fly linting.

![Vim Example](http://i.imgur.com/fR6Os5M.gif "Vim Example")

#### Installation ####
Put the file [syntastic/lintr.vim](inst/syntastic/lintr.vim)
in `syntastic/syntax_checkers/r`.  If you are using
[pathogen](https://github.com/tpope/vim-pathogen) this directory is
`~/.vim/bundles/syntastic/syntax_checkers/r`.

You will also need to add the following lines to your `.vimrc`.
```vim
let g:syntastic_enable_r_lintr_checker = 1
let g:syntastic_r_checkers = ['lintr']
```
#### Configuration ####
You can also configure what linters are used. e.g. using a different line length cutoff.
```vim
let g:syntastic_r_lintr_linters = "with_defaults(line_length_linter(120))"
```

### Sublime Text 3 ###
lintr can be integrated with
[Sublime Linter](https://github.com/SublimeLinter/SublimeLinter3) for on the fly linting.

![Sublime Example](http://i.imgur.com/3pua2yz.gif "Sublime Example")

#### Installation ####
Simply install `sublimeLinter-contrib-lintr` using [Package Control](https://packagecontrol.io/).

For more information see [Sublime Linter Docs](http://sublimelinter.readthedocs.org/en/latest/installation.html#installing-via-pc)

#### Configuration ####
You can also configure what linters are used. e.g. using a different line length cutoff.
In the SublimeLinter User Settings
```
{
  "user": {
    "linters": {
      "r": {
        "linters": "with_defaults(line_length_linter(120))"
      }
    }
  }
}
```

### Atom ###
lintr can be integrated with
[Linter](https://github.com/atom-community/linter) for on the fly linting.

![Atom Example](http://i.imgur.com/E1Isi4T.png "Atom Example")

#### Installation ####
Simply install `linter-lintr` from within Atom or on the command line with:
```bash
apm install linter-lintr
```

For more information and bug reports see [Atom linter-lintr](https://github.com/AtomLinter/linter-lintr).

## Available linters ##

* `Syntax errors`: reported by [parse](http://www.inside-r.org/r-doc/base/parse).
* `object_usage_linter`: check that closures have the proper usage using
  [codetools::checkUsage()](http://www.inside-r.org/r-doc/codetools/checkUsage).  Note this runs
  [base::eval()](http://www.inside-r.org/r-doc/base/eval) on the code, so do not use with untrusted code.
* `absolute_path_linter`: check that no absolute paths are used (e.g. "/var", "C:\\System", "~/docs").
* `nonportable_path_linter`: check that file.path() is used to construct safe and portable paths.
* `assignment_linter`: check that `<-` is always used for assignment
* `closed_curly_linter`: check that closed curly braces should always be on their
  own line unless they follow an else.
* `commas_linter`: check that all commas are followed by spaces, but do not
  have spaces before them.
* `infix_spaces_linter`: check that all infix operators have spaces around them.
* `line_length_linter`: check the line length of both comments and code is less than
  length.
* `no_tab_linter`: check that only spaces are used, never tabs.
* `camel_case_linter`: check that function and variable names are not camelCase.
* `snake_case_linter`: check that function and variable names are not snake_case.
* `multiple_dots_linter`: check that function and variable names are separated by `_` rather than `.`.
* `object_length_linter`: check that function and variable names are not more than `length` characters.
* `open_curly_linter`: check that opening curly braces are never on their own
  line and are always followed by a newline.
* `single_quotes_linter`: check that only single quotes are used to delimit
  string contestants.
* `spaces_inside_linter`: check that parentheses and square brackets do not have
  spaces directly inside them.
* `spaces_left_parentheses_linter`: check that all left parentheses have a space before them
  unless they are in a function call.
* `todo_comment_linter`: check that the source contains no TODO comments (case-insensitive).
* `trailing_blank_lines_linter`: check there are no trailing blank lines.
* `trailing_whitespace_linter`: check there are no trailing whitespace characters.
* `T_and_F_symbol_linter`: avoid the symbols `T` and `F` (for `TRUE` and `FALSE`).
* `unneeded_concatenation_linter`: check that the `c` function is not used without arguments nor
  with a single constant.

## Project Configuration ##

Lintr supports per-project configuration of the following fields.
The config file (default file name: `.lintr`) is in [Debian Control Field Format](http://www.debian.org/doc/debian-policy/ch-controlfields.html).

- `linters` - see `?with_defaults` for example of specifying only a few non-default linters.
- `exclusions` - a list of filenames to exclude from linting.  You can use a
  named item to exclude only certain lines from a file.
- `exclude` - a regex pattern for lines to exclude from linting.  Default is "# nolint"
- `exclude_start` - a regex pattern to start exclusion range. Default is "# nolint start"
- `exclude_end` - a regex pattern to end exclusion range. Default is "# nolint end"

An example file that uses 120 character line lengths, excludes a couple of
files and sets different default exclude regexs follows.
```
linters: with_defaults(line_length_linter(120))
exclusions: list("inst/doc/creating_linters.R" = 1, "inst/example/bad.R", "tests/testthat/exclusions-test")
exclude: "# Exclude Linting"
exclude_start: "# Begin Exclude Linting"
exclude_end: "# End Exclude Linting"
```

With the following command, you can create a configuration file for `lintr` that ignores all linters that show at least one error:

```r
library(magrittr)
library(dplyr)
lintr::lint_package() %>%
  as.data.frame %>%
  group_by(linter) %>%
  tally(sort = TRUE) %$%
  sprintf("linters: with_defaults(\n    %s\n    NULL\n  )\n",
          paste0(linter, " = NULL, # ", n, collapse="\n    ")) %>%
  cat(file = ".lintr")
```

The resulting configuration will contain each currently failing linter and the corresponding number of hits as a comment. Proceed by successively enabling linters, starting with those with the least number of hits. Note that this requires `lintr` 0.3.0.9001 or later.

## Travis-CI ##
If you want to run `lintr` on [Travis-CI](https://travis-ci.org) you will need
to have travis install the package first.  This can be done by adding the
following line to your `.travis.yml`

```yaml
r_github_packages:
  - jimhester/lintr
```

### Testthat ###
If you are already using [testthat](https://github.com/hadley/testthat) for
testing simply add the following to your tests to fail if there are any lints
in your project.  You will have to add `Suggests: lintr` to your package
`DESCRIPTION` as well.

```r
if (requireNamespace("lintr", quietly = TRUE)) {
  context("lints")
  test_that("Package Style", {
    lintr::expect_lint_free()
  })
}
```

### Non-failing Lints ###
If you do not want to fail the travis build on lints or do not use testthat you
can simply add the following to your `.travis.yml`
```yaml
after_success:
  - Rscript -e 'lintr::lint_package()'
```

In both cases the [lintr-bot](https://github.com/lintr-bot) will add comments
to the commit or pull request with the lints found and they will also be
printed on Travis-CI or Wercker.  If you want to disable the commenting you can
set the environment variable `LINTR_COMMENT_BOT=false`.

## Installation of development version ##
To install the latest development version of lintr from GitHub

```r
devtools::install_github("jimhester/lintr")
```

## References ##
Most of the default linters are based on [Hadley Wickham's R Style Guide](http://r-pkgs.had.co.nz/style.html).
