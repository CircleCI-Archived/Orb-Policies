# Language Orb Standardization

This document serves to define the standard schema/design for all language based orbs.

**Prerequisites**


1. Orb follows [CircleCI Orb Best Practices Guidelines (General guide/best practice)](https://github.com/CircleCI-Public/Orb-Policies/blob/master/Orb%20Best%20Practices%20Guidelines.md)

**Needs:**



*   **Examples**
    *   Must have at least 2 examples
        *   Show how to install the language
        *   Show how to run tests
        *   Showcase at least one job
    *   Example should include most common/simplest use case calling a top-level job
*   **Jobs:**
    *   Must include at least 1 job
        *   Include common use case job patterns
        *   Include a Build / Test job(s)
        *   Automatically Save/Load cache via parameter, utilizing included commands
*   **Executor:**
    *   Use cimg [[dockerhub](https://hub.docker.com/u/cimg)]
    *   Executor must have language tools pre-installed.
*   **Commands:**
    *   install-deps
        *   Description: Standard command for all language orbs to install dependencies
        *   Parameterize the package manager, with the most popular as the default
            *   Ensure the selected package manager (and version) is currently installed.
    *   test
        * must be wrapped inside restore and save cache commands
        * Calls language or frameworks's common testing tool(s)
    *   save-cache
        *   Automatically save cache for the selected language
        *   Parameterize path
        *   Parameterize key name
        *   Parameterize cache version number
    *   load-cache
        *   Automatically load cache for the selected language
        *   Parameterize path
        *   Parameterize key name
        *   Parameterize cache version number
*   **Maintainability:**
    *   Utilize the Orb Starter Kit so all future commits/prs are automatically tested and promoted.
