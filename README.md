# git-commands

Some custom git commands to make life a bit easier.

- [`git conventional-commits`](#git-conventional-commits)
- [`git modifications`](#git-modifications)
- [`git max-commit-size`](#git-max-commit-size)

## git-conventional-commits

Filter structured git history that uses [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/).

```
$ git conventional-commits --type feat

3e60ad1 feat(senedd-cymru): Add support for 'senedd.*' election ids
936b28b feat(senedd-cymru): Deprecate national_assembly_for_wales
feeb456 feat(senedd-cymru): Create entrypoint for the Senedd Cymru
c5ab320 feat(date): Add documentation for new date class
cb65776 feat(no-pandas): Remove dependency on pandas
```

Output can be structured as JSON for post-processing

```
$ git conventional-commits --type feat --json \
    | jq -r -s 'group_by(.scope)[] | "\(length),\(.[0].scope)"' \
    | sort -nr

7,date
6,api
3,senedd-cymru
3,implementation
3,for_id
3,eu-parl
2,no-pandas
```

## git-modifications

Export a detailed history of file modifications in CSV format from a git repo.

The columns are:
 - Repository (current directory)
 - Commit short hash
 - Additions
 - Deletions
 - Filename

```
$ git modifications 

git-commands,514889b,30,0,git-modifications
git-commands,87ccc04,8,6,git-conventional-commits
git-commands,f1ba0b2,5,7,git-conventional-commits
git-commands,a8b5c93,33,0,README.md
git-commands,0ff569c,2,1,git-conventional-commits
git-commands,965c839,10,3,git-conventional-commits
git-commands,c4bb513,34,0,git-conventional-commits
```

Command takes any flags that are accepted by `git log`

```
$ git modifications HEAD..HEAD~1

git-commands,514889b,30,0,git-modifications
```

# git-max-commit-size

Enforce a maximum size of commit in characters useful as a pre-commit hook.

```
$ git max-commit-size
Total commit size (761) exceeds +280 characters
$ echo $?
1
```

It takes one numeric argument which is the upper bound on commit size.

```
$ git max-commit-size 1000
$ echo $?
0
```