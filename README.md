## premarkdown

A hacky, markdown pre-processor.

Uses code blocks to generate markdown, from within the markdown itself.

This allows you to specify any sort of script to execute as a code block, whose contents then get replaced by the output of the script.

As an example. In a file `PRE_README.md`


    ## README

    Description of project

    ```
    >>>PREMARKDOWN
    #!/bin/sh
    printf 'This was last edited by %s on *%s*\n' "$(whoami)" "$(date)"
    ```

Running `premarkdown ./PRE_README.md` would generate a `README.md` file, with contents like:

    ## README

    Description of project

    This was last edited by sean on *Wed 05 Aug 2020 05:44:55 AM PDT*

This also sets two environment variables that are accessible from within the script:

* `RUN_FROM`: The directory `premarkdown` was executed in.
* `IN_DIR`: The directory this markdown file is in.

As an example, say you want a markdown file to act as an index for other directories in the folder, to create a table of contents of sort.

Directory Contents:

```
.
├── 01
│   └── README.md
├── 02
│   └── README.md
├── 03
│   └── README.md
└── PRE_TABLE_OF_CONTENTS.md
```

`PRE_TABLE_OF_CONTENTS.md`:

    Book Contents:

    ```
    >>>PREMARKDOWN
    #!/usr/bin/env python3
    import os
    # move to directory this file is in
    os.chdir(os.path.join(os.environ["IN_DIR"]))
    for path in os.listdir():
        if os.path.isdir(path):
            # create markdown which links to that chapter
            print("* [Chapter {}]({})".format(path, path + '/README.md')
    ```

    For more info, visit ...

which would generate the following markdown:

`TABLE_OF_CONTENTS.md`

    Book Contents:

    * [Chapter 01](01/README.md)
    * [Chapter 02](02/README.md)
    * [Chapter 03](03/README.md)

    For more info, visit ...

Theres no restriction on the language, this creates the corresponding script and executes it from the directory the markdown file is in directly. As long as the shebang is valid on your system, you could even do:

```
>>>PREMARKDOWN
#!/usr/bin/go run

import (
  "fmt"
  "time"
)

func main() {
  dt := time.Now()
  fmt.Println("Edited on: ", dt.String())
}
```

### Specification

* Expects the filename to start with `PRE_`, the output filename is the rest of the filename excluding the `PRE_` prefix.
* The markdown block must start with `>>>PREMARKDOWN`, all other code blocks are ignored.
* Nested code blocks are not allowed. If you want to generate a codeblock with your premarkdown block, see [examples/PRE_README.md](examples/PRE_README.md) for a workaround.
