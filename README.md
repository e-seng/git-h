# git-h
A quick git reference guide

### table of contents

[***🚨 !!! I don't care about the theory, bring me straight to the commands !!! 🚨***](#commands)

## what is `git`

`git` is _version control software_, which effectively means that the state of a
file throughout time can be managed, saved and shared.

nowadays, a lot of software is maintained using `git`, or some other version
control software. although this focuses on `git` in particular, a lot of
concepts of `git` can be used in other version control software.

## the layers of `git`

`git` typically operates with multiple "layers" or "stages", which I typically
see as "levels of readiness" before the code is merged into the final branch,
which is typically made ready for release.

listing each layer by "level of readiness" shows the following

```
remote repository        |  code is ready
    release branch       |        ^
    remote branch        |        |
                         |        |
local repository         |        |
    local branch         |        |
        commited changes |
        staged changes   |        v
        working changes  |   code is wip
                         |
        ignored changes  |
```

all code starts as "working changes". this effectively means that

- [ ] todo: finish :p

## commands

### working with local repositories

`git init`

- create a new local repository

`git status`

- double check the status of all files, except ignored files
- this will identify which files are not being captured by `git`, or which files
  have unsaved changes
- generally, this command is probably the most useful to see what needs to be
  updated

`git checkout <BRANCH NAME>`

- switches which branch you're working on
- `-b <BRANCH NAME>` can be used to create a new branch with the specified name
  and check into that new branch

for example:

```sh
# creating a new branch and check into it
git checkout -b fibonacci-func

# check into the branch with the name "master"
git checkout master
```

`git add [path/to/FILE1 path/to/FILE2...]`

- stage working files, either adding them to the repository for the first time
  or staging changes within those files

for example:

```sh
$ git add README.md main.c
```

`git commit`

- commit staged files
- this will typically open a text editor to write a _commit message_,
  usually in `vim` or `nano`
  - to simplify the committing process, `-m <message>` can be used to
    specify the commit message in the command line
- typically committing to the main/master branch is a _bad idea_, especially
  when collaborating with other people

for example:

```sh
$ git commit -m "Add files to repository"
```

> note that commit messages are typically formatted similar to a command. almost
> like you're telling the repository a command. commit messages are also
> typically specific and concise (within 50 characters, though this is never
> enforced)
>
> a typical "good" commit messages is similar to the following:
>
> "Define recursive Fibonacci function"
>
> - present tense
> - concise
> - specific
> - starts with the verb, like a command
>
> typical "bad" commit include
>
> - "Fibonacci function was defined"
>   - doesn't start with a verb, likely harder to parse when reviewing commits
> - "Defined recursive Fibonacci function" or "Defining recursive Fibonacci
>   function"
>   - inconsistent tense, not a war crime or something but harder to parse
> - "Define a recursive Fibonacci function in C by using tail recursion which
>   reduces memory usage when making the call"
>   - commit message is _way too long_
>   - any details can be added into the description of the commit, though this
>     can only be added if a text editor is being used to create the commit
>     message.
> - "committing changes"
>   - probably the worst of all
>   - no details about what changed are specified

`git branch`

- lists the branches that exist on the local repository
- `-d <BRANCH NAME>` will delete an existing branch

`git merge <BRANCH NAME>`

- merges the specified branch into the current branch
- this may lead to _merge conflicts_ if the two branches has commit changes
  which conflicts on the same lines within a file
- this is typically really difficult to deal with, especially starting out

for example:

```sh
# assuming we're not on the main branch at the moment
$ git merge main
```

> dealing with merge/rebase conflicts
>
> - [ ] TODO

### working with remote repositories

commands here typically affect the committed changes on the repository

`git fetch`

- checks for any updates made to the remote repository not found within the
  local repository
- this effectively "synchronizes" the states of the repositories, but does
  not actually change any file
- determine if any new remote branches were created
- any new branches found can be checked-into using `git checkout` (see above)

`git pull`

- pull all updates from the remote repository
- will perform `git fetch` implicitly
- typically, changes made to the main/master branch should be pulled, and then
  merged/rebased into the working branch
  - this is more-so etiquette to improve the code review process

`git push`

- copies any new local commits to the remote repository if that commit is not
  already present
- `-u <remote name> <branch name>` or `--set-upstream <remote name> <branch name>`
  can create a new remote branch with the provided name
  - what's really nice, whenever a new branch is created on a remote repository,
    the tool will usually automatically provide a link to create a "merge/pull
    request"
- it is ***fundamental*** that pushes are ***never*** made to the main/master
  branch

for example:

```sh
# assuming we are checked-out into the branch fibonacci-func
# initial new branch push, creates the "fibonacci-func" in the remote repository
git push -u origin fibonacci-func

# moving forward, any changes on this branch can be pushed up using:
git push
```
