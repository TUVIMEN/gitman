# gitman

My personal script for managing multiple git remotes like [gitolite](https://wiki.archlinux.org/title/Gitolite) or [github](https://github.com).

`gitman` script creates a registry of repositories in `~/.cache/gitman`, where it stores first and last commit hash, tags, description, path, and list of remotes.

While working on this script i came up with name `gitman`, searching through github to my dismay I've discovered that there already are popular projects with that name, since they are of no use to me i didn't change the name.

## Adding scripts managing remotes

`gitman-github` and `gitman-gitolite` are external script that can be added to registry as aliases.

```bash
gitman remote github ~/.local/bin/gitman-github
gitman remote home ~/.local/bin/gitman-gitolite
```

`gitman-github` requires [github-cli](https://cli.github.com/)

## Environment variables

`gitman` needs

```bash
export GITMAN_OWNER='hexderm'
```

`gitman-gitolite` needs

```bash
export GITMAN_GITOLITE_REPO="/home/hexderm/git/gitolite-admin"
```

`gitman-github` needs

```bash
export GITMAN_GITHUB_USER="TUVIMEN"
```

## Adding repositories

After creating a local repository you can add it to registry

```bash
gitman add
```

You can specify or change tags and description associated with it

```bash
gitman add -d 'description' -t 'tag1|tag2' -t tag3
gitman add -d 'other description' -t tag4 -t -tag1 # removes tag1 and adds tag4
```

Having repository in registry you can add managed remotes

```bash
gitman radd home github
```

When having managed remotes you can again add it to registry to update remotes

```bash
gitman add
```

This will create and push repositories to gitolite and github while changing tags and description

## pushing changes/renaming/deleting

You can push changes to all repositories by running

```bash
for i in $(git remote); do git push "$i"; done
```

or simply

```bash
gitman push
```

You can rename repository by running

```bash
gitman move new-name
```

It can also be deleted

```bash
gitman delete
gitman delete -f # remove without prompt
```

## Searching the registry

`gitman` provides extensive searching capabilities. You can search by specifying extended regex for 9 options - `path`, `description`, `tags`, `remote-name`, `remote-path`, `rmeote-date`, `firstcommit`, `lastcommit`, `lastchange`. Output can also be changed by the same options, but beginning with `--o-`.

 Get descriptions of all repositories
```bash
gitman get --o-description
```
Get path and remote path of repositories that are in projects directory and were modified in 2025-03

```bash
gitman get --path '/projects/' --lastchange '^2025-03-' --o-path --o-remote-path
```
Dump the registry

```bash
gitman get
```

## Help

You can read about all available subcommands and options by running

```bash
gitman help
```

Subcommands also have their own help messages

```bash
gitman add --help
gitman delete --help
gitman remote --help
```
