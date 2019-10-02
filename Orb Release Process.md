
# Orb Release Process


# Needs

In order to test and publish orbs, we need a robust release process. This includes the various forms of testing (linting, validation, integration, etc) as well as publishing versions in accordance with [SemVer](https://semver.org/) (Semantic Versioning). Integration testing specifically can be tricky with some orbs and so is very important we get that right.

Some specific needs are:



*   Support for building pull requests (PRs) in parallel - two PRs should be able to be opened around the same time with their build processes working independently of each other. Meaning, their builds such not affect the outcome of each other. This is a limitation of our current release process.
*   Various Testing Types
    *   Linting (YAML)
    *   Validating (orb schema)
    *   Integration testing (various based on type of orb)
    *   Shell check
*   Proper SemVer support
*   Automatic deployment on merge


# Current Process(es)

We’re actually using a couple of variations on a release process currently. The current process is based on using the `circleci/orb-tools` orb as well as a `dev:alpha` testing release of the orb to be released.

Using several workflows and jobs, an orb is linted, validated, and published under `@dev:alpha`, then a new build is kicked off to run integration for that dev orb. Depending on the branch, a dev release or a patch release is published.

For some orbs, such as the Slack orb or our deprecated orb monorepo, we also implement a staging branch release process. This adds another step to the release process which causes some updates to orbs not to be published for awhile because they just sit in the staging branch.


## Limitations

Updating the major or minor version isn’t as obvious as it should be. Also, with the reliance on the single dev version of the orb, multiple branches/PRs can’t be run at the same time (or even near each other) without the second build altering the results of the first one.


# Short-Term Release Process

Our goal is to get to the “Future Release Process” (below) but we’re waiting on API support in order to get there. Until then, this is how we should be releasing orbs.

The following are the steps the orb release process should go through:

## Feature Branch Work [Contributors & Maintainers]

*   An orb should already be written in destructured format and ideally by the Orb Starter Kit
*   Changes must be made on a non-master branch. Non-draft PRs should be opened when it’s ready for review
*   `orb-tools` will pack the orb during a build
*   Validate -> lint -> shell check generated orb.yml
*   Publish dev version of orb with branch name and short git hash as version
    *   Ensure branch names encode properly
*   Create a new branch, based off of current branch
    *   Git hash should be appended to branch name
    *   The CircleCI config updated to import the dev orb just created
*   Push to trigger new build
*   Run integration testing
*   Clean up old branch
    *   Post link to PR [WHERE?]
    
## PR Review and Merge [Maintainers]
* At least 1 non-author must review and approve changes.
* Reviewer or Author (if maintainer) may merge once approved.
* Merge message must indicate the semantic impact using `[semver:FOO]` in the commit *subject* where FOO is (major, minor, patch)


## Master Release Stream
* Master build must repeat any automated testing used in feature branch
* Orb should be promoted based on `[semver:XXXX]` commit indicator ([example](https://github.com/CircleCI-Public/cloudfoundry-orb/pull/18/files#diff-1d37e48f9ceff6d8030570cd36286a61R67) )
   * Absense of semver in commit subject must skip release
* Production version of Orb should be added to the original PR by cpe-bot ([example](https://github.com/CircleCI-Public/cloudfoundry-orb/pull/18/files#diff-1d37e48f9ceff6d8030570cd36286a61R77))



## Examples

As we implement this process with orbs, we should provide CircleCI config examples here and/or link out to them.

- Using SemVer in Release Process [example](https://github.com/CircleCI-Public/cloudfoundry-orb/pull/18/files#diff-1d37e48f9ceff6d8030570cd36286a61R67) 
- Adding Dev versions of orbs to open PRs [example](https://github.com/CircleCI-Public/cloudfoundry-orb/pull/18/files#diff-1d37e48f9ceff6d8030570cd36286a61R39)
- Adding Prod version of orbs to closed PRs [example](https://github.com/CircleCI-Public/cloudfoundry-orb/pull/18/files#diff-1d37e48f9ceff6d8030570cd36286a61R77)
- 


# Future Release Process

The future process will be mostly the same as the short-term process. The difference is using parameterized pipelines to replace the need to create a temporary branch that is pushed to GitHub. Parameterized Pipelines is currently an outstanding request for the API v2.0 team.
