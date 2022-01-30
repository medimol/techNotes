# git
## revert
``` sh
git revert commitId
```
Make a commit to delete changes made by commit of commitId.

## cherry-pick
``` sh
git cherry-pick commitId
```
Partially merge by commit from other branch.

## Commit Message

### Prefixes
- **Fix**: fix bug (Hotfix if critical bug)
- **Feat**: add new feature or file (Add)
- **Update**: correction of feature, not bug
- **Change**: change in specification
- **Clean**: refactor (Refactor, Improve)
- **Disable**: disable feature
- **Remove**: delete file (Delete)
- **Move**: move file
- **Upgrade**: upgrade version
- **Revert**: undo change
- **Docs**: Change of document
- **Style**: Format fix
- **Perf**: performance improvement
- **Test**: Fix according to test
- **Chore**: Commit of auto generated file

### Tips
- Rebase commits if possible to squash by feature
- `git commit --amend` can be used to fix typo

```
1: Abstract of commit details (issue)
2: Blank
3 and below: body
```

- imperative form
- capitalize head of sentence
- present tense

Body should include not HOW but WHAT and WHY

## gitignore
```
# comment

# specific extension
*.csv

# specific directory
bin/

# include
!/hoge/fuga.csv

# 
```
## Branch
### Name
```
{prefix}/{Issue#}-{name_with_snake_case}
```

### Change Branch Name
```
git branch -m <old branch name> <new branch name>
git branch -m <new branch name of present branch>
```
