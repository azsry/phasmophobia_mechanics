# Versions #

***[Just Take Me To The Latest Information](./stale/README.md)***

This is the directory where per-version phasmophobia build updates will live,
currently this is rather empty besides [Our One File](./stale/README.md) is the state
of what this repository 'used to be'. The repository used to not have a clear
seperation of version information. So it became a bit unclear what was out of
date, and what was 'current'. This new format will hopefully alleviate that
unfortunately, it takes a lot of work to maintain an in-depth changelog.

We'll get there.

## Folder Naming ##

The folder name structure is built to help you find the information you need,
hopefully as quickly as possible. Specifically the folders will follow the
following naming scheme:

`${date the build was released}/${branchIdentifier}${build-id}/`

So for example at the time of writing this the latest build publicly available
was: 7062992, it's branch identifier was 's', and it was released on July 21st, 2021.
So the folder would be: `07-21-2021/s7062992`.

### What is this 'branch identifier'? ###

The game Phasmophobia has multiple 'releases', which can also be called 'branches'.

- `developer`: (which is password protected, and presumably only for the developer)
- `beta`: (which is _now_ password protected, but didn't use to be).
- `stable`: which is the build everyone without special access plays on, and is the actual game you know.

In order to make it simple for us to track if this is an update that went through
beta testing, etc. We attach a single letter at the front to signify where the
build first originated.

- If the build was pushed directly to stable without going through beta testing it's identifier starts with `s`.
- If the build was pushed to beta testers, and then later released to stable with no changes it's identifier starts with `b`.
- If the build was pushed to developers, then beta testers, and then released to stable with no changes it's identifier starts with `d`.

_note: if builds were pushed to multiple branches at the same time we assign to stable first, beta second._