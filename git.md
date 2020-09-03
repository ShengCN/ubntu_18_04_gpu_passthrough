# Git commands

## Submodules
### Add submodules
```bash
# Add existing submodule.
git submodule add url

# see difference
git diff --cached [name]

# commit
git commit -am 'commence'
```

### Clone submodules
**Method 1**
```bash
git clone url

# submodule 
# submodule init (git file register for local)
git submodule init

# submodule update
git submodule update

```

**Method 2**
```bash
git clone --recurse-submodules url
```

**Method 3**

If have cloned project, to set up submodules, use this command:
```bash
git submodule --init --recursive
```

### Working on a submodule
```bash
# Git submodule's HEAD will not point to any branch.
git checkout [branch name]

# After modification, merge with remote update
git submodule update --remote --merge/--rebase
```