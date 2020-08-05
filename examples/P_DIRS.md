Information about the environment:

```
>>>PMARK
#!/bin/bash
printf "This is being called from %s\n\n" "$(dirname "${BASH_SOURCE[0]}")"
echo -e "The command was called from ${RUN_FROM}\n"
echo -e "The file is in ${IN_DIR}\n"
```
