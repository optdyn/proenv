# Proenv

A simple framework to manage git project environments. 

> `proenv`, a portmanteau of **pro**ject and **env**ironment, also playing on [direnv](https://github.com/direnv/) which proenv uses at its core with a sprinkle of git hacks and conventions.

## Goals

1. easy, simple, and fast set up
2. unobtrusive, safe, repeatable, and reliable envs
3. enforce policy, consistency, and encapsulation
4. simple stupid implementation accessible and understandble by anyone

### Clone Time Setup

Git repositories cloned using proenv use post-clone hooks to set up project environments local to the cloning host. This involves checking for tooling dependencies, installing tools missing, and configuring tool settings for the project. Client-side git hooks are set for the git repo as intended by project maintainers to consistently provide the nessessary env, tools, and use them as automatically as possible.

> Previously post-clone was used independent of proenv: clutter is prevented if integrated into proenv. Likewise, git-hooks was integrated into proenv.


