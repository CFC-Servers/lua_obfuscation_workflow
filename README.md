# Lua obfuscation workflow
Reusable Github Workflow to automatically obfuscate your Lua code

This uses [Prometheus](https://github.com/levno-710/Prometheus) as the obfuscation tool.

## Using

This workflow accepts the following inputs:

**`input-branch`**
 - The name of the branch that the tool will obfuscate
 - (Defaults to the repo's primary branch)

**`output-branch`**
 - Where the tool will force-push the obfuscated code
 - (Defaults to `obfuscated`)

**`paths`**
 - A JSON array of paths for the obfuscator to act on.
 - This can be any unix-compatible path, including wildcards.
 - i.e. `'["lua/autorun/sh_*","lua/project/client/","lua/project/shared/"]'`
 - (Defaults to all lua files)

**`preset`**
 - The name of the preset used to configure Prometheus.
 - You can learn more about the available presets [**here**](https://github.com/levno-710/Prometheus/blob/master/doc/getting-started/presets.md)
 - (Defaults to `Strong`)

**`censor-commits`**
 - A boolean string describing whether or not to censor the commit messages on the output branch
 - (Defaults to `'false'`)

**`prometheus-repo`**
 - A string path describing where to pull Prometheus from, in the format of `user/repo`
 - (Defaults to `levno-710/Prometheus`)

**`prometheus-ref`**
 - The ref (branch, tag or SHA) of the Prometheus repo to use
 - (Defaults to `master`)

## Examples
Here are some example workflows using this tool:

---

 - On every push to `main`, obfuscate the following lua files and push them to the `obfuscated` branch
    - All lua files prefixed with `sh_` in the `lua/autorun/` dir
    - All lua files prefixed with `sh_` in the `lua/myproject/` dir or its children
    - All lua files in the `lua/myproject/client/` dir
 - Commit messages will not be censored
 - Uses the "Weak" preset
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
      paths: '["lua/autorun/sh_*","lua/myproject/**/sh_*","lua/myproject/client"]'
      preset: "Weak"
```


---


 - On manual dispatch, allow the user to select a number of options
 - Will obfuscate all lua files in the `lua/myproject/client/` dir
```yml
name: Obfuscator

on:
  workflow_dispatch:
    inputs:
      output-branch:
        type: string
        description: "Output branch"
        required: true
        default: obfuscated
      preset:
        type: choice
        description: "Which prometheus preset to use"
        options:
        - Minify
        - Vm
        - Weak
        - Medium
        - Strong
        default: Strong
      prometheus-branch:
        type: choice
        description: "Which prometheus branch to use"
        options:
        - master
        - develop
      censor-commits:
        type: boolean
        description: "Censor commit messages"

jobs:
  run-obfuscator:
    uses: CFC-Servers/lua_obfuscation_workflow/.github/workflows/obfuscate_lua.yml@main
    with:
      input-branch: ${{ github.event.repository.default_branch }}
      output-branch: ${{ github.event.inputs.output-branch }}
      preset: ${{ github.event.inputs.preset }}
      censor-commits: ${{ github.event.inputs.censor-commits }}
      prometheus-branch: ${{ github.event.inputs.prometheus-branch }}
      paths: '["lua/myproject/client/"]'
```
