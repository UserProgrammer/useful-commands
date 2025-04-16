### Commands
- [git](#git)

### git
Create a tag in the current commit:
```shell
git tag -a tag-name -m "Tag description"
```

Move existing tag to current commit (description will not be carried over; must be explicitly included in the command):
```shell
git tag -f tag-name -m "Tag description"
```

Delete a tag:
```shell
get tag -d tag-name
```

Show tag details:
```shell
git show tag-name
```


Push a tag to the remote repository (include `-f` option if the tag already exists):
```shell
git push origin tag-name
```
