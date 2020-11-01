# git-commands

Some custom git commands to make life a bit easier.

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