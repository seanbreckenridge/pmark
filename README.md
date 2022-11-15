# pmark

A hacky, markdown pre-processor.

Uses code blocks to generate markdown, from within the markdown itself.

This allows you to specify any sort of script to execute as a code block, whose contents then get replaced by the output of the script.

As an example, in a file `P_README.md`:

    ## README

    Description of project

    ```
    >>>PMARK
    #!/bin/sh
    printf 'This was last edited by %s on *%s*\n' "$(whoami)" "$(date)"
    ```

Running `pmark ./P_README.md` would generate the file `README.md`, with contents:

    ## README

    Description of project

    This was last edited by sean on *Wed 05 Aug 2020 05:44:55 AM PDT*

---

Say you want a markdown file to act as an index for other directories in the folder, to create a table of contents of sort.

Directory Contents:

```
.
├── 01
│   └── README.md
├── 02
│   └── README.md
├── 03
│   └── README.md
└── P_TABLE_OF_CONTENTS.md
```

`P_TABLE_OF_CONTENTS.md`:

    Book Contents:

    ```
    >>>PMARK
    #!/usr/bin/env python3
    import os
    for path in os.listdir():
        if os.path.isdir(path):
            # create markdown which links to that chapter
            print("* [Chapter {}]({})".format(path, path + '/README.md')
    ```

    For more info, visit ...

That would generate the following markdown:

`TABLE_OF_CONTENTS.md`

    Book Contents:

    * [Chapter 01](01/README.md)
    * [Chapter 02](02/README.md)
    * [Chapter 03](03/README.md)

    For more info, visit ...

Theres no restriction on the language, this creates the corresponding script file (with a random name, like `<script_directory>/CT1PIRQX5-pmark`) and executes it from the markdown files' directory.

---

This sets two environment variables that are accessible from within the script:

- `RUN_FROM`: The directory `pmark` was executed in.
- `IN_DIR`: The directory this markdown file is in.

---

If you wanted to run this against all files which contain `pmark` blocks recursively, can combine this with the `find` command:

`find . -name 'P_*.md' -exec pmark {} +`

Or, with [`fd`](https://github.com/sharkdp/fd):

`fd 'P_*.md' -X pmark`

If no files are provided, this searches for files matching `P_*.md` in the current directory.

---

I'll often use this in Github `README`s, especially for new projects, as while I'm developing tools/libraries it keeps my `README` up to date with the right flags/example output, e.g.,:

```
>>>PMARK
perl -E 'print "`"x3, "\n"'
make
./mytool -h
make clean
perl -E 'print "`"x3, "\n"'
```

For examples see [here](https://github.com/seanbreckenridge/ttally/blob/26f3b32ed6085efea965185c79e831a0ab033770/P_README.md) or [here](https://github.com/seanbreckenridge/plaintext-playlist/blob/8b053fa45a19ccb4e82f4d47cfe58802a94ff252/P_README.md)

As a more complex example, I use this for my blog, see [my blog index](https://github.com/seanbreckenridge/exobrain/tree/24a5440fe57943c531231fd24f4e2bd07856ccc2/sitemap).

### Installation

Download the `pmark` script, make it executable and put it on your `$PATH`. Can also use sinister:

`sh <(curl -sSL http://git.io/sinister) -u 'https://raw.githubusercontent.com/seanbreckenridge/pmark/master/pmark'`

Or [`basher`](https://github.com/basherpm/basher):

```
basher install seanbreckenridge/pmark
```

### Specification

- Expects the filename to start with `P_`, the output filename is the rest of the filename excluding the `P_` prefix.
- The markdown block must start with `>>>PMARK`, all other code blocks are ignored.
- Like markdown itself, nested code blocks are not allowed. If you want to generate a markdown codeblock using code, see [`examples/P_SHELL.md`](examples/P_SHELL.md) for a workaround.
