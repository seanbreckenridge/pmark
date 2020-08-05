## Examples

Generated README files:

```
>>>PMARK
#!/usr/bin/env python3
import os
for path in os.listdir():
  if "P_" not in path and path.endswith('md'):
    # create markdown which links to that chapter
    print("* `{}`".format(path))
```
