# Git Flow

## What is Gitflow 

>***Giflow is an alternative Git branching model that involves the use of feature branches and multiple primary branches. It was first published and made popular by [Vincent Driessen at nvie](https://nvie.com/posts/a-successful-git-branching-model/). Compared to trunk-based development, Giflow has numerous, longer-lived branches and larger commits. Under this model, developers create a feature branch and delay merging it to the main trunk branch until the feature is complete. These long-lived feature branches require more collaboration to merge and have a higher risk of deviating from the trunk branch. They can also introduce conflicting updates.***

Source: [Atlassian](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow)

## Getting Started

### Install Gitflow

Follow this instructions according to your OS [Gitflow](https://github.com/petervanderdoes/gitflow-avh/wiki/Installation)

### Instructions

Gitflow as a model, has a couple of preset thatwe can configure and work on it, such as:

- `git flow init` (Initializer your Repo with some structures available for development)
    - main branch
    - feature branch
    - release branch
    - hotfix branch
    - support branch
    - Version Tag prefix
- `git flow feature start {your_feature_branch}` create a brand new branch **your_feature_branch**, feature brach (here you will develop your new feature)
- `git flow feature finish {your_feature_branch}` end your branch **your_feature_branch** auto merging to the develop branch ready to pack it up for your release, and removing **your_feature_branch** after merge it
- `git flow release start {your_version_tag}` create your release from your current develop branch and our release is ready for QA
- `git flow release finish {your_version_tag}` finish release branch auto merging with our main branch and develop branch, then our release branch is deleted as well
- `git flow hotfix start {your_hotfix_branch}` create a hotfix branch from your target branch, normally our main branch
- `git flow hotfix start {your_hotfix_branch}` finish hotfix branch auto merging with our main branch and develop branch, then our hotfix branch is deleted as well

## Why Gitflow

Some key takeaways to know about Gitflow are:

1. The workflow is great for a release-based software workflow.
2. Gitflow offers a dedicated channel for hotfixes to production.
 
The overall flow of Gitflow is:

1. A develop branch is created from main
2. A release branch is created from develop
3. Feature branches are created from develop
4. When a feature is complete it is merged into the develop branch
5. When the release branch is done it is merged into develop and main
6. If an issue in main is detected a hotfix branch is created from main
7. Once the hotfix is complete it is merged to both develop and main

Source: [Atlassian](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow)

## Why not Gitflow

Nowadays, the *Agile* development paradigms lead us to seek less complicated solutions to improve the quality and quantity of features in the software development deliveries.

As the workflow complexity grows the CI/CD becomes more and more unsustainable.

Such complexity also cause, as a collateral effect, a big history problem, in large scale codebases where many developers work on, observability is easily lost, people get lost of some bug track or even, in microsservice world, "what's in God name" is deployed in production (main branch).

So before you start your project, think about scalability and your Branching Model!

You might check it this article about the [harmfulness of Gitflow work model](https://www.endoflineblog.com/gitflow-considered-harmful)

---

# Signing Commits

In repositories where many developers work, whether providing maintenance or developing new features, security is essential, since all code changes must undergo code reviews, we must ensure the authorship and authenticity of our work.

## GPG

GNU Privacy Guard (GnuPG or GPG) is a open source software that use private/public keys to store and transfer user information between hosts.

## Get Started

### Installation

- `sudo apt install gnupg` for Linux users (Ubuntu)
- [Windows GPG](https://www.gpg4win.org/) for Windows
- [MacOS](https://gpgtools.org/) for Mac

### Generate your Key

*Note: In these steps I'm going to use a Ubuntu as OS.*

First lets check if we have any keys registered:

```gpg --list-secret-key --keyid-form LONG```

Output -> Any key stored in your gpg / Nothing otherwise

```gpg --full-generate-key```

Output -> Here it will prompt some questions:

- What kind of key you want (*RSA and RSA* for GitHub GPG)
- RSA Length between 1024 and 4096 bits long (*4096* ***big hash, such security!***)
- How long do you want your key to be valid (days, weeks, months, years or never expires)
- Real Name (use your exact same username from Git here, to check what is your Git name: `git config --list`)
- Email Address (use your exact same email from Git here, to check what is your Git email: `git config --list`). Tip: you can append more emails in this key later on, if needed.
- Confirm and we got our GPG key

Get the rsa4096 ID

`gpg --list-secret-key --keyid-form LONG`

Copy all your RSA GPG key with

`gpg --armor --export {KEY_ID}`

Then go to GitHub and Paste your RSA Key to your ***SSH and GPG*** GitHub Configuration.

Finally 

`git config --global user.signingkey {KEY_ID}` Set Globally your RSA key in git config

In Linux we need to export the GPG TTY

`export GPG_TTY=$(tty)` 

Write the instruction above in your bash config file!

Set true to flag that allows git to sign all commits and tags

`git config --global commit.gpgSign true`
`git config --global tag.gpgSign true`

