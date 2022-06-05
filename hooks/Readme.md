# What's this?

## Git Template

The entire proenv git project acts as a git template when cloning target repositories. That means its contents are copied into the cloned repository's "${repo}/.git" folder. As a result, the `hooks` directory is fetched into "${repo}/.git/hooks".

## One Time post-checkout Execution

With the `hooks` directory feteched into the cloned repository's "${repo}/.git/hooks", the `post-checkout` hook script is invoked once the clone operation finishes. This incarnation of the script runs only once to set up the repository to use proenv, its permanent hooks, and invoke the post-clone mimicing hook script.

The post-checkout script runs once because it deletes itself while executing. While executing it deletes this `hooks` directory which contains it. It links the "${repo}/.git/hooks" to the "${repo}/.githooks" directory which is created if not found.

