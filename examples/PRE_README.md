## README

Description of project

```
>>>PREMARKDOWN
#!/bin/sh
printf 'This was last edited by %s on *%s*\n' "$(whoami)" "$(date)"
```

Current Weather:

```
>>>PREMARKDOWN
#!/bin/sh
# i.e. print the beginning of a codeblock, without explicitly writing a codeblock to not break the regex
perl -E 'print "`"x3, "\n"'
curl 'wttr.in'
perl -E 'print "`"x3, "\n"'
```
