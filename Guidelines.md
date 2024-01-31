# Development Guidelines

This document aims to define a set of rules to adhere to when writing code as well as provide information to anyone who may use, modify or collaborate on my code at any given time.

A version of this document MUST be present at the root of a repository that adheres to these rules and MUST be made accessible to collaborators. A copy of this document MAY be made publicly available for anyone who may use my code however this is OPTIONAL.

A link to the latest version of this document MUST be provided at the top of any code files that adhere to these rules providing they are not part of a project (e.g. Scripts).
The GitHub section SHALL NOT apply to code files that adhere to these rules but are not part of a project.

---

> [!NOTE]
>
> Please read this document in it's entirety.
>
> You can find the latest version of this document [here](https://gist.github.com/Darnel-K/8badda0cabdabb15359350f7af911c90).

> [!IMPORTANT]
>
> The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED",  "MAY", and "OPTIONAL" in this document are to be interpreted as described in [RFC 2119](https://datatracker.ietf.org/doc/html/rfc2119) and [RFC 8174](https://datatracker.ietf.org/doc/html/rfc8174).

---

## Variables & Functions

- Classes / Methods: Use Pascal Case
- Functions: Use Camel Case
- Constants / Globals: Use Screaming Snake Case
- Variables: Use Snake Case

### Snake Case / Screaming Snake Case (Uppercase Snake Case)

Snake case (also known as pothole case in python programming) separates each word with an underscore character (`_`).
When using snake case, all letters are lowercase however when using Screaming Snake Case, all letters are uppercase.

#### Screaming Snake Case Example

```python
NUMBER_OF_DONUTS = 34

FAVE_PHRASE = "Hello World"
```

#### Snake Case Example

```python
number_of_donuts = 34

fave_phrase = "Hello World"
```

### Camel Case

When using camel case, you start by making the first word lowercase. Then, you capitalize the first letter of each word that follows.

#### Example

```python
numberOfDonuts = 34

favePhrase = "Hello World"
```

### Pascal Case

Pascal case is almost the same as Camel case however, only difference between the two is that pascal case requires the first letter of the first word to also be capitalized.

#### Example

```python
NumberOfDonuts = 34

FavePhrase = "Hello World"
```

### Kebab Case

Kebab case is very similar to snake case.

The difference between snake case and kebab case is that kebab case separates each word with a dash character, `-`, instead of an underscore.

#### Example

```python
number-of-donuts = 34

fave-phrase = "Hello World"
```

## GitHub

### Branching

#### Quick Legend

| Instance | Branch Name | Description, Instructions, Notes                                                                                         |
| -------- | ----------- | ------------------------------------------------------------------------------------------------------------------------ |
| Stable   | stable      | Production (Live). Accepts merges / pull requests from Staging                                                           |
| Staging  | staging     | Pre production (Stable) testing. Accepts merges / pull requests from Master & Hotfix branches                            |
| Working  | master      | Latest development changes ready for next release. Accepts merges / pull requests from Feature, Bugfix & Hotfix branches |
| Feature  | feature/*   | Always branch off HEAD of master                                                                                         |
| BugFix   | bugfix/*    | Always branch off HEAD of master                                                                                         |
| HotFix   | hotfix/*    | Always branch off Stable                                                                                                 |

#### Main Branches

The main repository will always hold three evergreen branches:

- stable
- staging
- master

The main branch should be considered `origin/master` and will be the main branch where the source code of `HEAD` always reflects a state with the latest delivered development changes for the next release. As a developer, you will be branching and merging from `master`.

Consider `origin/stable` to always represent the latest code deployed to production. During day to day development, the `stable` branch will not be interacted with.

When the source code in the `master` branch is ready for release, all of the changes will be merged into `staging` and tested ready for production.

Once the source code in the `staging` branch has been tested and is verified working, all of the changes will be merged into `stable` and tagged with a release number.

#### Supporting Branches

Supporting branches are used to aid parallel development between team members, ease tracking of features, and to assist in quickly fixing live production problems. Unlike the main branches, these branches always have a limited life time, since they will be removed eventually.

The different types of branches we may use are:

- Feature branches
- Bug branches
- Hotfix branches

Each of these branches have a specific purpose and are bound to strict rules as to which branches may be their originating branch and which branches must be their merge targets. Each branch and its usage is explained below.

#### Feature Branches

Feature branches are used when developing a new feature or enhancement which has the potential of a development lifespan longer than a single deployment. When starting development, the deployment in which this feature will be released may not be known. No matter when the feature branch will be finished, it will always be merged back into the master branch.

During the lifespan of the feature development, the lead should watch the `master` branch (network tool or branch tool in GitHub) to see if there have been commits since the feature was branched. Any and all changes to `master` should be merged into the feature before merging back to `master`; this can be done at various times during the project or at the end, but time to handle merge conflicts should be accounted for.

Replace `<reference>` with the reference of the issue/ticket/bug you are working on. If there's no reference, replace with 'no-ref'.

- Must branch from: `master`
- Must merge back into: `master`
- Branch naming convention: `feature/<reference>/description-in-kebab-case`

##### Working with a feature branch

If the branch does not exist yet (check with the Lead), create the branch locally and then push to GitHub. A feature branch should always be 'publicly' available. That is, development should never exist in just one developer's local branch.

```text
git checkout -b feature-id master                 // creates a local branch for the new feature
git push origin feature-id                        // makes the new feature remotely available
```

Periodically, changes made to `master` (if any) should be merged back into your feature branch.

```text
git merge master                                  // merges changes from master into feature branch
```

When development on the feature is complete, the lead (or engineer in charge) should merge changes into `master` and then make sure the remote branch is deleted.

```text
git checkout master                               // change to the master branch
git merge --no-ff feature-id                      // makes sure to create a commit object during merge
git push origin master                            // push merge changes
git push origin :feature-id                       // deletes the remote branch
```

#### Bug Branches

Bug branches differ from feature branches only semantically. Bug branches will be created when there is a bug on the live site that should be fixed and merged into the next deployment. For that reason, a bug branch typically will not last longer than one deployment cycle. Additionally, bug branches are used to explicitly track the difference between bug development and feature development. No matter when the bug branch will be finished, it will always be merged back into `master`.

Although likelihood will be less, during the lifespan of the bug development, the lead should watch the `master` branch (network tool or branch tool in GitHub) to see if there have been commits since the bug was branched. Any and all changes to `master` should be merged into the bug before merging back to `master`; this can be done at various times during the project or at the end, but time to handle merge conflicts should be accounted for.

Replace `<reference>` with the reference of the issue/ticket/bug you are working on. If there's no reference, replace with 'no-ref'.

- Must branch from: `master`
- Must merge back into: `master`
- Branch naming convention: `bugfix/<reference>/description-in-kebab-case`

##### Working with a bug branch

If the branch does not exist yet (check with the Lead), create the branch locally and then push to GitHub. A bug branch should always be 'publicly' available. That is, development should never exist in just one developer's local branch.

```text
git checkout -b bug-id master                     // creates a local branch for the new bug
git push origin bug-id                            // makes the new bug remotely available
```

Periodically, changes made to `master` (if any) should be merged back into your bug branch.

```text
git merge master                                  // merges changes from master into bug branch
```

When development on the bug is complete, [the Lead] should merge changes into `master` and then make sure the remote branch is deleted.

```text
git checkout master                               // change to the master branch
git merge --no-ff bug-id                          // makes sure to create a commit object during merge
git push origin master                            // push merge changes
git push origin :bug-id                           // deletes the remote branch
```

#### Hotfix Branches

A hotfix branch comes from the need to act immediately upon an undesired state of a live production version. Additionally, because of the urgency, a hotfix is not required to be be pushed during a scheduled deployment. Due to these requirements, a hotfix branch is always branched from a tagged `stable` branch. This is done for two reasons:

- Development on the `master` branch can continue while the hotfix is being addressed.
- A tagged `stable` branch still represents what is in production. At the point in time where a hotfix is needed, there could have been multiple commits to `master` which would then no longer represent production.

Replace `<reference>` with the reference of the issue/ticket/bug you are working on. If there's no reference, replace with 'no-ref'.

- Must branch from: tagged `stable`
- Must merge back into: `master` and `stable`
- Branch naming convention: `hotfix/<reference>/description-in-kebab-case`

##### Working with a hotfix branch

If the branch does not exist yet (check with the Lead), create the branch locally and then push to GitHub. A hotfix branch should always be 'publicly' available. That is, development should never exist in just one developer's local branch.

```text
git checkout -b hotfix-id stable                  // creates a local branch for the new hotfix
git push origin hotfix-id                         // makes the new hotfix remotely available
```

When development on the hotfix is complete, [the Lead] should merge changes into `stable` and then update the tag.

```text
git checkout stable                               // change to the stable branch
git merge --no-ff hotfix-id                       // forces creation of commit object during merge
git tag -a <tag>                                  // tags the fix
git push origin stable --tags                     // push tag changes
```

Merge changes into `master` so not to lose the hotfix and then delete the remote hotfix branch.

```text
git checkout master                               // change to the master branch
git merge --no-ff hotfix-id                       // forces creation of commit object during merge
git push origin master                            // push merge changes
git push origin :hotfix-id                        // deletes the remote branch
```

#### Workflow Diagram

![Git Branching Model](https://raw.githubusercontent.com/digitaljhelms/public/master/gitflow-model.png)

### Releases, Tags & Versioning

> [!IMPORTANT]
>
> The repository MUST follow [Semantic Versioning](https://semver.org/)

Tags MUST follow the below format.

> - Format: `vX.Y.Z-{State}`
>   - v = Prefix
>   - X = Major
>   - Y = Minor
>   - Z = Patch
>   - State = `stable` or `rc` / `pre` or `beta`
>   - Example: v0.56.523-stable

#### State Suffix

##### Stable

A `stable` release MUST be created from the `staging` branch when the `staging` branch has passed testing and is about to be merged into `stable`.

The version number used in the stable release MUST be the same as the version number of the release candidate merged from `staging`.

A stable release MUST use the suffix `-stable`.
A stable release MUST be created based off `staging` before `staging` is merged into `stable`.

###### Example

> `v0.56.523-rc` ---> `v0.56.523-stable`

##### RC (Release Candidate)

A `Release Candidate (-rc)` release MUST be created from the `master` branch when the `master` branch is ready to be merged into the `staging` branch for testing.

The version number used in a `Release Candidate (-rc)` release MUST be the same as the version number of the beta release merged from `master`.

A release candidate MUST use the suffix `-rc` or `-pre`. The suffix `-rc` is preferred.
A release candidate release MUST be created based off `master` before `master` is merged into `staging`.

###### Example

> `v0.56.523-beta` ---> `v0.56.523-rc`

##### Beta

A `beta` release MUST be created everytime one of the supporting branches is merged into `master`.

The tag used for a `beta` release MUST be unique.

A `beta` release SHALL only share a version number with a `Release Candidate (-rc)` or `Stable` release and MUST NOT share a version number with another `beta` release.

###### Examples

These are some examples of `beta` release tags that adhere to the specifications of this document.

> `v0.56.523-beta`
> `v0.56.396-beta`
> `v0.56.0-beta`
> `v0.55.293-beta`

#### Release Notes

#### Incrementing Major Minor or Patch

### Committing Code

#### General Rules

- Make [atomic commits](http://en.wikipedia.org/wiki/Atomic_commit) of changes, even across multiple files, in logical units. That is, as much as possible, each commit should be focused on one specific purpose.
- As much as possible, make sure a commit does not contain unnecessary whitespace changes. This can be checked as follows:

```text
git diff --check
```

#### Commit Messages

As a general rule, your commit message should start with a single line that's no more than about 50 characters and that describes the commit concisely. If you feel the need for more detailed explanations, create a blank line, followed by a more detailed explanation.

For consistency, try and use the imperative present tense when creating a message. Examples:

- Use "Add tests for" instead of "I added tests for"
- Use "Change x to y" instead of "Changed x to y"

*This information has been curated base on the [Issues 2.0 release blog post](https://github.com/blog/831-issues-2-0-the-next-generation)*

In order to associate commits with GitHub Issues, the commit message should indicate one or more issue number and (optionally) a state change for the story. The commit message should start with square brackets containing a hash mark followed by the issue number. For example:

```text
[#123] Diverting power from warp drive to torpedoes
```

To automatically close an issue by using a commit message, include "Closes" in the square brackets in addition to the issue number. For example:

```text
[Closes #123] Torpedoes now sufficiently powered
```

Note: There is more than one way of closing an issue via the commit message, however "Closes" is our suggested standard:

- `Fixes #xxx`
- `Fixed #xxx`
- `Fix #xxx`
- `Closes #xxx`
- `Close #xxx`
- `Closed #xxx`

Theoretically, it is possible to enclose more than one issue numbers within brackets, as well as combine actions with commit tracking. For example:

```text
[Closes #123][#124][#125] Torpedoes now sufficiently powered
```

This would close issue 123 and add commit references to issues 124 and 125 for tracking purposes.

#### Message Template

Based on the [blog post](http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html) by [tbaggery](http://tbaggery.com/), here is a template to use as guideline for commit messages:

```text
[<optional state> #issueid] (50 chars or less) summary of changes

More detailed explanatory text, if necessary. Wrap it to about 72
characters or so.

Further paragraphs come after blank lines.

- Bullet points are okay, too

- Typically a hyphen or asterisk is used for the bullet, preceded by a
single space, with blank lines in between, but conventions vary here
```

## References

- [Git/GitHub branching standards &amp; conventions](https://gist.github.com/digitaljhelms/4287848)
- [A Simplified Convention for Naming Branches and Commits in Git](https://dev.to/varbsan/a-simplified-convention-for-naming-branches-and-commits-in-git-il4)
- [Git/GitHub commit standards & conventions](<https://gist.github.com/digitaljhelms/3761873>)
- [Snake Case VS Camel Case VS Pascal Case VS Kebab Case – What's the Difference Between Casings?](https://www.freecodecamp.org/news/snake-case-vs-camel-case-vs-pascal-case-vs-kebab-case-whats-the-difference/)
- [README Template](https://gist.github.com/PurpleBooth/109311bb0361f32d87a2) Use this for a good Readme.md template
- [Keep a Changelog 1.1.0](https://keepachangelog.com/en/1.1.0/)
- [Naming cheatsheet - kettanaito](https://github.com/kettanaito/naming-cheatsheet)

---
Copyright &copy; 2024 [Darnel Kumar (Darnel-K)](mailto:support@abyss.org.uk?subject=Copyright%20%26%20Licensing%20Enquiry)

The content of this document is licensed under the [Creative Commons Attribution-ShareAlike 4.0 International](https://creativecommons.org/licenses/by-sa/4.0/) License.
