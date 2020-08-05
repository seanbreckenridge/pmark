## README

Description of project

```
>>>PMARK
#!/bin/sh
printf 'This was last edited by %s in *%s*\n' "$(whoami)" "$(date +'%Y')"
```

Hello world:

```
>>>PMARK
#!/bin/sh
# i.e. print the beginning of a codeblock, without explicitly writing a codeblock to not break the regex
perl -E 'print "`"x3, "\n"'
printf 'echo "Hello World"\n'
perl -E 'print "`"x3, "\n"'
```

```
echo "This code block is ignored..."
```
