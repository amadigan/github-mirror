# GitHub Mirror

Github Mirror is a [GitHub Action](https://github.com/features/actions) that keeps a GitHub repository up to date as
a mirror of an external repository. It will synchronize all branches and commits from the specified repository, it
will not copy non-Git data such as Pull Requests.

## Setup
To create a mirror, start by creating a repository locally and creating a branch to hold the workflow file. The branch
should be named such that it will not conflict with a branch name in the source repository.

```
$ git init mirror
$ git -C mirror checkout -b __mirror
$ mkdir -p mirror/.github/workflows
```

Create a file like the one below in your repository. Replace *TARGET_REPO_HERE* with the URL of a the git repository
to mirror.

`.github/workflows/mirror.yml`
```
name: 'mirror'
on:
  push:
    branches:
        - __mirror
  schedule:
    - cron: '* 3 * * *'
  workflow_dispatch:

jobs:
  mirror:
    runs-on: ubuntu-latest
    name: mirror
    steps:
      - name: mirror
        id: mirror
        uses: bridgelightcloud/github-mirror@main
        with:
          origin: 'TARGET_REPO_HERE'
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

Commit the workflow file, then create a repository on GitHub and push the branch. The workflow will start
immediately and copy the source repository.

## License
The code is trivial, as such this repository is licensed under the Creative Commons CC-0 license.
