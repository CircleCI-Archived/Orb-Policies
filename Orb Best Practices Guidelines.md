# Orb Best Practices Guidelines
A collection best practices for authoring [orbs](https://circleci.com/docs/2.0/orb-intro/#section=configuration).

CircleCI orbs are shareable packages of configuration elements, including jobs, commands, and executors. To begin learning how to create your own orb, check out our docs on [Reusable config](https://circleci.com/docs/2.0/reusing-config/) and get started with the [Orb Starter Kit (Beta)](https://github.com/CircleCI-Public/orb-starter-kit).


## Guidelines



*   **Meta data:**
    *   Ensure all Commands, Jobs, Executors, and Parameters have detailed descriptions.
    *   Include the code repo link in the Orb description.
    *   Include link to website in description.
    *   Define any prerequisites such as obtaining an API key in the description.
    *   Be consistent and concise in naming your orb elements. For example, don't mix "kebab case" and "snake case".
*   **Examples:**
    *   Must have at least 1 example.
    *   Show Orb version as `x.y` (patch version may not need to be included)
    *   Example should include most common/simplest use case calling a top-level job or other base-case elements if no job is present.
    *   If applicable, you may want to showcase the use of [pre and post steps](https://circleci.com/docs/2.0/reusing-config/#using-pre-and-post-steps) in conjunction with an Orb’s Job. 
*   **Commands:**
    *   In general, all Orbs should contain at least one Command. 
        *   Some exceptions may include creating an Orb for the sole task of providing an executor.
    *   Combine one or more parameterizable steps to simplify a task.
    *   All commands available to the user should complete a full task. Do not create a command for the sole purpose of being a “partial” to another command unless it can be used on its own.
    *   It is possible not all CLI commands need to be transformed into an Orb command. Single line commands with no parameters do not necessarily need to have an Orb command alias.
    *   Command descriptions should call out any dependencies or assumptions, particularly if you intend for the command to run outside of a provided executor in your orb.
    *   **Suggested Naming Conventions**
        *   **CLI Orbs**
            *   **install**
                *   Check for and install the tool if it is not currently installed
            *   **setup**
*   **Parameters:**
    *   When possible, use defaults for parameters unless a user input is essential.
    *   Utilize the [“env_var_name” parameter type](https://circleci.com/docs/2.0/reusing-config/#environment-variable-name) to secure API keys or other sensitive data. 
    *   [Injecting steps as a parameter](https://circleci.com/docs/2.0/reusing-config/#steps) is a useful way to run user defined steps within a job between your orb-defined steps.Good for if you need to perform an action both before and after user-defined tasks - for instance, you could run user-provided steps between your caching logic inside the command.
*   **Jobs:**
    *   Jobs should utilize Commands defined within the Orb to orchestrate common use cases for this Orb.
    *   Plan for flexibility
        *   Plan how users might utilize post-steps, pre-steps, or steps as a parameter.
    *   Consider creating pass-through parameters. 
        *   If a Job utilizes an Executor or Command that accepts parameters, the Job will need those parameters as well if they are to be passed down to the Executor or Commands.
    *   Never hard-code the executor. Utilize a parameterizable (ex: ‘default’) executor that is able to have the image or tag overwritten.
*   **Executors:**
    *   At least one Executor per supported OS (MacOs, Windows, Docker, VM).
    *   Must include a “default” executor.
    *   Executor should be parameterized to allow the user to overwrite the version/tag in the event an issue arises with the included image.
*   **Imported Orbs:**
    *   Must import full semver immutable version of orb.
    *   [Partner Only] - Should only import Certified/Partnered namespaces, or Orbs of the same namespace.
*   **Maintainability:**
    *   Deploy full CI/CD for your orb with a fully automated build > test > deploy workflow using the [Orb Starter Kit (Beta)](https://github.com/CircleCI-Public/orb-starter-kit). This handles all of the below.
    *   Optional: Utilize a destructured orb pattern to more easily maintain individual orb components.
*   **Deployment:**
    *   **Versioning:**
        *   Utilize [semver versioning](https://semver.org/) (x.y.z)
            *   Major: Incompatible changes
            *   Minor: Add new features (backwards compatible)
            *   Patch: Minor bug fixes, metadata updates, or other safe actions.
    *   View our Orb Deployment best practices here: [https://docs.google.com/document/d/1MqQCo8aeYjvGJAa0v60L2FJelLFjWP1IgkWuBfMiC3c](https://docs.google.com/document/d/1MqQCo8aeYjvGJAa0v60L2FJelLFjWP1IgkWuBfMiC3c) [NOT COMPLETE]
    *   This section is handled automatically via the Orb Starter Kit.
* GitHub/Bitbucket
    * GitHub (and maybe Bitbucket) has the ability to tag repositories. This is used as a datapoint in GitHub search but more importantly, in their Explore page to group repositories by tags. We suggest using the tag `circleci-orbs` for a repo containing an orb. This allows users to view a listing of orb repos whether they are CircleCI, Partner, or community orbs on [this page](https://github.com/topics/circleci-orbs).
