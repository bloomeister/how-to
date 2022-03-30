# Flow

## Basic Usage

```
$ git flow feature start test
Switched to a new branch 'feature/test'

Summary of actions:
- A new branch 'feature/test' was created, based on 'develop'
- You are now on branch 'feature/test'

Now, start committing on your feature. When done, use:

     git flow feature finish test
```

```
git flow feature finish test
```

## Create and merge pull request

> CREATE PULL REQUEST > MERGE on main > MERGE on develop > COMMIT on develop

It's OK

> CREATE PULL REQUEST > MERGE on main > COMMIT on develop > MERGE on develop