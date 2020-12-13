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

This also sets two environment variables that are accessible from within the script:

* `RUN_FROM`: The directory `pmark` was executed in.
* `IN_DIR`: The directory this markdown file is in.

I use this for my blog, see [`P_README.md`](https://raw.githubusercontent.com/seanbreckenridge/exobrain/master/sitemap/P_README.md) here and the generated page [here](https://exobrain.sean.fish/sitemap/)

If you wanted to run this against all files which contain `pmark` blocks recursively, can combine this with the `find` command:

`find . -name 'P_*.md' -exec pmark {} +`

Or, with [`fd`](https://github.com/sharkdp/fd):

`fd 'P_*' -X pmark`

If no files are provided, this searches for files matching `P_*.md` in the current directory.

### Installation

Download the `pmark` script, make it executable and put it on your `$PATH`. Can also use sinister:

`sh <(curl -sSL http://git.io/sinister) -u 'https://raw.githubusercontent.com/seanbreckenridge/pmark/master/pmark'`

### Specification

* Expects the filename to start with `P_`, the output filename is the rest of the filename excluding the `P_` prefix.
* The markdown block must start with `>>>PMARK`, all other code blocks are ignored.
* Like markdown itself, nested code blocks are not allowed. If you want to generate a markdown codeblock using code, see [`examples/P_SHELL.md`](examples/P_SHELL.md) for a workaround.
