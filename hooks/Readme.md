# What's this?

## Git Template

The entire proenv git project acts as a git template when cloning target repositories. That means its contents are copied into the cloned repository's `${repo}/.git` folder. As a result, the `hooks` directory is fetched into `${repo}/.git/hooks`.

> ***But this is deleted ..***

## One Time post-checkout Execution

With the `hooks` directory fetched into the cloned repository's `${repo}/.git/hooks` directory, this temporary `post-checkout` hook script is invoked once the clone operation finishes. This incarnation of the script runs only once to set up the repository and proenv: the `.proenv` folder, permanent hooks in `.githooks`, and invoke the post-clone pseudo hook scripts in `post-clone.d`.

The `post-checkout` script runs once because it deletes itself while removing the `${repo}/.git/hooks` directory where it was originally fetched. Then `${repo}/.git/hooks` is soft linked to the `${repo}/.githooks` directory which is created if not found.
