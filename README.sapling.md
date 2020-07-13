# Node Module Sapling

This repository contains a sapling (skeleton) for building a Node Module using `npm` that can be uploaded to a NPM registry.

Modules uploaded to a NPM registry can be used by other modules or Node/Typescript/Javascript(TBD) applications

## Getting started

* Click "Use this template", and name your new creation.
  **Protip**: project names should be short, and descriptive. *Avoid using underscores for node modules*.

  ![you know it](https://help.github.com/assets/images/help/repository/use-this-template-button.png)

* Clone the resulting project to your computer
  ```
  cd ~/code
  git clone git@github.com:your-org/my-module.git
  ```

* Remove this `README.md` file, and replace it with your own

* Edit the `package.json`
  * set the package `name`, `description`, `url`, and `author`
  * **NOTE:** Do not set version in this file, CICD automation will create versions based on [annotated git tags](https://git-scm.com/book/en/v2/Git-Basics-Tagging#_annotated_tags)
  * **NOTE:** Do not delete `publishConfig`, if you want to learn about the importance of `publishConfig` read [private](https://docs.npmjs.com/files/package.json#private) and [publishConfig](https://docs.npmjs.com/files/package.json#publishconfig)
  * Add your dependencies

* Add your code
  * TBD
  * Run `npm install` to sync your local dependencies
    *Dependencies installed, but not listed in your lock file will not be installed in CICD!*

* Commit your code
  * See the section on [Workflow](#Workflow) for Git workflow processes and best practices

* Add your project to CICD

* Party!

## Features

`husky.hooks.pre-commit` in `package.json` file defines a git pre-commit `hook` script:

```
pretty-quick --staged && CI=true npm run test:coverage - lints the code and runs test coverage
```

## Using modules

This sapling relies on [npm version from-git](https://docs.npmjs.com/cli/version) to produce versioned node modules.

**Note:** `npm version from-git` relies on [annotated git tags](https://git-scm.com/book/en/v2/Git-Basics-Tagging#_annotated_tags)

### in your `package.json`
If you've created a project called `yodelling-coach` and had just freshly minted git tag `v0.9.9`,
your artifact would be called `yodelling-coach-v0.9.9.tgz`.

Using this module in another package.json would look like:
```
  "dependencies": {
...
    "@ivy/yodelling-coach": "0.9.9",
...
```

## Workflow

### Git bootstrap

The initial Git bootstrap workflow is:

1. Clone the sapling
2. Add your code in any state (working or not)
3. Commit your code
4. Execute `git tag -a v0.0.1 -m "The one where feature is done"`, this will tag the `master` branch with a semver of `v0.0.1`
5. Execute `git push --follow-tags`

CICD will build a `0.0.1` version of your library which depending on the state of your initial code commit may be all you need.

### Collaborating and Developing

Once you've decided you need to make updates or fix bugs, you've got two options:
* `feature-*` branches -> `develop` branch -> `master` branch
  Have multiple contributors? Want separation of duties and potentially a benevolent merge-dictator?
  Use this method if so.

* `develop` branch -> `master` branch
  Just one person working on the project? No time for process, just need to get builds **now**?
  Alright, *alright*. I hear ya. Use this method instead.

**Short and sweet version:** Don't commit directly to `master` branch.

### Feature Branches

![workflow example](https://i.imgur.com/pwEqlCs.png)

Setting up feature branches is easy.
After you've got the initial bootstrap done, create a `develop` branch and commit it.
Then create a feature branch (does not need to be named anything in specific, just keep it short and descriptive) and work from there.

As you work, make commits to the feature branch and push them to Github as you see fit.
CICD will run any unit tests and help you spot any issues along the way.

Once you've completed your feature, send a Pull Request from Github to merge into the `develop` branch.
The project admin (maybe you?) will review this PR and decide to merge it if they see fit.

Once the PR is merged into the `develop` branch, the project admin will usually allow CICD to build a
prerelease module that can be used in an integration test for any projects that require this module.

After all integration tests are completed, the project admin will merge `develop` into `master` and cut a new git tag.

### Develop -> Master

![workflow example](https://i.imgur.com/Aq6SasP.png)

Don't have time for the whole feature branch method? That's fine. You can shortcut it and migrate to feature branches later if required.

After the initial bootstrap, simply create and work on your `develop` branch. Commit directly to the `develop` branch and
merge (PR or manually) into `master` when you're ready to create a new production-ready version by tagging off `master`.
