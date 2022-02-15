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
      paths: '["lua/autorun/sh_*","lua/myproject/**/sh_*"]'
      preset: "Strong"
      censor-commits: 'true'
```

 - On manual dispatch, obfuscate all lua files in `main` and push them to `obfuscated`.
 - Commits on the output branch will be censored.
 - Uses the `develop` branch of the Prometheus repo
```yml
name: Obfuscator

on:
  workflow_dispatch:

jobs:
  run-obfuscator:
    uses: CFC-Servers/lua_obfuscation_workflow/.github/workflows/obfuscate_lua.yml@main
    with:
      input-branch: "main"
      output-branch: "obfuscated"
      preset: "Strong"
      censor-commits: 'true'
      prometheus-branch: "develop"
```
