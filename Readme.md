# Proenv

A simple framework to manage and automate project environments, tooling, configurations, and policies built on tools you know and love.

> A portmanteau of ***pro***ject and ***env***ironment, also a play on [direnv](https://github.com/direnv/) used at its core with a sprinkle of git hacks and conventions. These are patterns and conventions that made life easier and happier for me. Hope it does the same for you.

***Why bloat and over provision systems with tools you'll never use on them?*** Effortlessly provision project dependencies and tools when they are needed at project clone time. Save time, save space, and stay nimble letting computers do the rot work.

***Why let the environment get you down?*** Every project uses tools, but those tools manifest different behaviors in the different environments they're installed in. Besides variations in the environment, users also have their own personal settings and preferences that impact behavior. These unspoken configurations cause a majority of the friction and frustration developers experience when they just need to clone, code, test, debug and push changes consistently.

## Goals

1. easy, simple, and fast set up
2. unobtrusive, safe, repeatable, and reliable envs
3. enforce policy, consistency, and encapsulation
4. KISS accessible, understandable, and hackable by everyone

## How does it work?

### Clone-time Setup

Git repositories cloned using proenv use pseudo "post-clone" hooks to set up project environments locally on the cloning host. Scripts, or links act as post-clone hook scripts checking for tooling dependencies, install missing tools, and configure tool settings specifically for the project. Projects can be checked out using the following curl pipe to bash:

```shell
  # pick your target repo
  repo='http://github.com/optdyn/proenv-project.git'
  curl -fsSL https://raw.githubusercontent.com/optdyn/proenv/main/bin/proenv-clone | \
    bash -s -- ${repo}
```

### Git Hooks

Default client-side git hooks are set for the git repo. These hooks execute any executable files found in their respective `${hook_name}.d` directories. For example, all executable files under `commit-msg.d` are executed for the `commit-msg` hook:

```shell
───────┬────────────────────────────────────────────────────────────────────────
       │ File: proenv-project/.githooks/commit-msg
───────┼────────────────────────────────────────────────────────────────────────
   1   │ #!/usr/bin/env bash
   2   │
   3   │ source "$(git rev-parse --show-toplevel)/.git/hooks/common"
   4   │ exec_dotd "$(basename $0)" $@
   5   │
───────┴────────────────────────────────────────────────────────────────────────
```

#### Multiple Scripts One Hook

These default hooks allow for multiple scripts to be launched per hook in `${hook}.d` directories. Without this pattern, git would limit a hook to a single script. Client-side hooks are kept versioned within the project repository using `${repo}/.githooks` instead of `${repo}/.git/hooks` which is not versioned. Project maintainers drop scripts or links into respective dot.d directories so they execute when the hook triggers:

```shell
.githooks/
├── commit-msg*
├── commit-msg.d/
├── common*
├── post-checkout.d/
├── post-clone.d/
├── post-commit*
├── post-commit.d/
├── pre-commit*
├── pre-commit.d/
├── prepare-commit-msg*
└── prepare-commit-msg.d/
```

> Previously [post-clone](https://github.com/git-hook/post-clone) was used independent of proenv: clutter is prevented if it is integrated into proenv (as `${repo}/bin/proenv-clone`). Likewise, a separate repository, ***git-hooks***, was used for just the default hooks, but was later integrated into proenv to also reduce clutter.

#### No `post-clone` Hook Script

Notice that unlike the other hook scripts, there is no executable `post-clone` script, however, a `post-clone.d` directory does exists. This is because post-clone, remember, is a pseudo-hook. Git does not have such a hook so there is no use for such a file. Furthermore, scripts in `post-clone.d` are only invoked one time, at clone-time, by a temporary post-checkout script that runs only once after the clone operation. The scripts in `post-clone.d` never again execute.

## Fast Test

Real PITA to commit every little bit to try and test. Do the following to prevent this need:

```shell
git clone https://github.com/optdyn/proenv.git ~/.proenv
ln -s ~/.proenv/bin/proenv-clone ~/.local/bin/proenv-clone
```

> If `~/.local/bin` is not on your PATH make sure it is

Then don't curl pipe bash, instead use the following line replacing the project git url:

```shell
rm -rf proenv-project
proenv-clone https://github.com/optdyn/proenv-project.git
```

> If using zsh, issue a `rehash` before executing the above

