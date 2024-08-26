# copyright<sup>2</sup>

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

copyright<sup>2</sup> (copyright-right) is a text file copyright notice lint and update
tool to help keep the years listed in your copyright notices up to date with when they
were modified.

## Features

- Supports any text file.
- Configurable to match your specific copyright notice format.
- Configurable to include/exclude specific files/directories.
- Check mode which shows changes missing from files.
- Fix mode which updates files with missing changes.
- Intelligently simplifies copyright year expressions e.g. `2009,2010,2011,2013` to
  `2009-11,3`.

## Getting Started

### Prerequisites

- Python 3.9 or later

### Installation

Install from source using your favourite package manager

```bash
pip install git+https://github.com/phyrwork/copyright2.git
```

### Usage

#### Configuration

Create a `.copyrightrc.yaml` at the top level of your project and describe your copyright
notice and specify some files to manage using regular expressions.

By default, no files or directories are included.

```yaml
# .copyrightrc.yaml
root: yes
copyright: "Copyright \(c\) {ts}"
include_dirs:
  - .*  # All directories.
simplify: yes  # Simplify copyright year expression.
```
   
Options specified in `.copyrightrc.yaml` of subdirectories take precedence for that
directory.

```yaml
# src/.copyrightrc.yaml
include_files:
  - .*\.py$  # All .py files in this directory and below.
```

```yaml
# src/ext/.copyrightrc.yaml
include_files:
  - .*\.(c|h)$  # Only .c, .h files in this directory and below.
```

#### Run

Check files that will be managed by `copyright`

```bash
examples/readme$ copyright list
src/readme.py
src/ext/readme.h
src/ext/readme.c
3
```

then scan for changes that need to be made to any copyright notices

```bash
examples/readme$ copyright check
src/readme.py: 1: simplified timestamp expression
1
```

(the exit code will be `0` if no changes are found) and fix them

```bash
examples/readme$ copyright fix
fixing src/readme.py... ok
1

examples/readme$ git diff
diff --git a/examples/readme/src/readme.py b/examples/readme/src/readme.py
index 65785b6..cb2603b 100644
--- a/examples/readme/src/readme.py
+++ b/examples/readme/src/readme.py
@@ -1 +1 @@
-"""Copyright (c) 2023,2024,2025."""
+"""Copyright (c) 2023-5."""
```
