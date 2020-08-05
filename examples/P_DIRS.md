Information about the environment:

```
>>>PMARK
#!/bin/bash
printf "This is being called from %s\n" "$(dirname "${BASH_SOURCE[0]}")"
echo "The command was called from ${RUN_FROM}"
echo "The file is in ${IN_DIR}"
```
