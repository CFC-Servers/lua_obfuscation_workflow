# Lua obfuscation workflow
Reusable Github Workflow to automatically obfuscate your Lua code

This uses [Prometheus](https://github.com/levno-710/Prometheus) as the obfuscation tool.

## Using

This workflow has four inputs:

**`input-branch`**
 - The name of the branch that the tool will obfuscate

**`output-branch`**
 - Where the tool will force-push the obfuscated code

**`paths`**
 - A JSON array of paths for the obfuscator to act on.
 - This can be any unix-compatible path, including wildcards.
 - i.e. `'["lua/autorun/sh_*","lua/project/client/","lua/project/shared/"]'`

**`preset`**
 - The name of the preset used to configure Prometheus.
 - You can learn more about the available presets [**here**](https://github.com/levno-710/Prometheus/blob/master/doc/getting-started/presets.md)


## Examples
Here are some example workflows using this tool:

```yml
name: Obfuscator

on:
  push:
    branches:
      - 'main'

jobs:
  run-obfuscator:
    uses: CFC-Servers/lua_obfuscation_workflow/.github/workflows/obfuscate_lua.yml@main
    with:
      input-branch: "main"
      output-branch: "obfuscated"
      dirs: '["lua/autorun/sh_*","lua/myproject/sh_*","lua/project/client/"]'
      preset: "Strong"
```
