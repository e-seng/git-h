# git-h
A quick git reference guide

### table of contents

[***🚨 !!! I don't care about the theory, bring me straight to the commands !!! 🚨***](#commands)

## what is `git`

`git` is _version control software_, which effectively means that the state of a
file throughout time can be managed, saved and shared. `git` will scan the
directory tree of a repository, looking for any changes _made to the files
stored on disk_. As such, `git` aims to replace file versions, especially in
instances where the code may look like the following:

```
$ tree project_dir
project_dir
├── mycode-version0.1.c
├── mycode-version0.2.c
├── mycode-version0.3.c
├── mycode-version0.4.c
├── mycode-version0.5.c
├── mycode-version0.6.c
├── mycode-version0.7.c
├── mycode-version0.8.c
├── mycode-version0.9.c
├── mycode-version1.0.c
├── mycode-version1.1.c
├── mycode-version1.1-final-actuallyfinal.c
├── mycode-version1.1-final.c
└── mycode-version1.1-wip.c
```

instead, using `git` makes the file system look a lot cleaner and easier to
follow.

```
$ tree -laL1 ./project_dir
./project_dir
├── .git
└── mycode.c
```

this works as `git` provides all the benefits of saving the file with a new
name, along with a lot more features.

nowadays, a lot of software is maintained using `git`, or some other version
control software. although this focuses on `git` in particular, a lot of
concepts of `git` can be used in other version control software.

## the layers of `git`

`git` typically operates with multiple "layers" or "stages", which I typically
see as "levels of readiness" before the code is merged into the final branch,
which is typically made ready for release.

listing each layer by "level of readiness" shows the following

```
remote repository        |  file is ready
    release branch       |        ^
    remote branch        |        |
                         |        |
local repository         |        |
    local branch         |        |
        commited changes |        |
        staged changes   |        v
        working changes  |   file is wip
                         |
        ignored changes  |
```

### working changes

all changes, including new files, start as "working changes". this effectively
means that `git` does not save the state of these changes, but will be able to
detect that these changes exist within the repository.

for brand new files, [`git status`](#git-status) will mark these files as
"untracked", as it would no sense of how the file has changed over time. as
such, `git` has no idea what the initial state for any file would be, or even if
the file is of importance to the repository, so it will continue to ignore
untracked files until it is explicitly told to keep an eye on them

any file with history tracked by `git` can be changed and
[restored](#git-restore). as such, this enables a sense of "experimentation"
while working on the repository since any "bad changes" can be reverted entirely
to a previous known and typically stable state.

working files and changes can be "staged" when they are [added to the
repository](#git-add)

### staged changes

I like to think of this stage as the "hat containing changes to track". this
staging layer acts like place where you can set up what your next saved state is
going to look like. it really, effectively, is a step _in preparation to_ commit
the changes. through a mixture of [adding](#git-add) and [restoring](#git-restore)
staged changes, the set of changes you specifically wish to save (and create a
strong commit message for :>) can be narrowed down by moving changes between the
staged and working layers.

note that changes across multiple files can be staged to be committed at the
same time, or a subset of the changes within a _single_ file can be staged.

once the set of changes to save is determined then they can be [committed to the
repository](#git-commit) with an effective commit message

### committed changes

once changes have been committed, `git` will store that state in a chronological
list of changes. at this point, that code is typically in a static (ish) state,
and can be referred back to at any point in time. along with this, `git` will
use the state of this code to determine all future changes made to the file.

as there is now a temporal list of states, someone may wish to view the history
of the code base and see how the code has changed over time. `logs` can be read
to obtain an overview of what each commit did, as long as the commit message is
descriptive enough.

changes committed on a branch can then be [merged](#git-merge) with other
branches, to combine previous or collaborative work.

- [ ] todo: remote branches

## commands

### working with local repositories

##### `git init`

- create a new local repository in the current working directory
- this will be named whatever the working directory is named

##### `git status`

- double check the status of all files, except ignored files
- this will identify which files are not being captured by `git`, or which files
  have unsaved changes
- generally, this command is probably the most useful to see what needs to be
  updated

##### `git checkout`

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

##### `git add`

- stage working files, either adding them to the repository for the first time
  or staging changes within those files

for example:

```sh
$ git add README.md main.c
```

##### `git commit`

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

##### `git branch`

- lists the branches that exist on the local repository
- `-d <BRANCH NAME>` will delete an existing branch

##### `git merge`

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

##### `git fetch`

- checks for any updates made to the remote repository not found within the
  local repository
- this effectively "synchronizes" the states of the repositories, but does
  not actually change any file
- determine if any new remote branches were created
- any new branches found can be checked-into using `git checkout` (see above)

##### `git pull`

- pull all updates from the remote repository
- will perform `git fetch` implicitly
- typically, changes made to the main/master branch should be pulled, and then
  merged/rebased into the working branch
  - this is more-so etiquette to improve the code review process

##### `git push`

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
