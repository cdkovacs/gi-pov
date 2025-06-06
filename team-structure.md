GitHub/IBM POV  
GitHub Team Structure

Prepared by: Christopher Kovacs  
Date: May 20, 2025  
Version: 1.0

# Executive Summary

GitHub Teams segment members within an organization, provide granular access control, and enable collaboration. Teams can be structured hierarchically to reflect organizational structure, with parent teams managing broader access and child teams handling more specific responsibilities. This document outlines best practices for leveraging automation, creating team structure, implementing naming conventions, and how to effectively use teams in conjunction with repository topics.

# Guiding Principles

1. **Simplicity.** Maintain the lowest number of teams required. Avoid assigning members to parent teams.
2. **Least Privilege.** Grant teams only the access they need to perform their responsibilities.
3. **Scalability.** Design team structures that can grow with the organization without becoming unwieldy.
4. **Maintainability.** Create team structures that are easy to understand and manage.
5. **Visibility.** Use team visibility settings appropriately to balance transparency with privacy needs.

# Recommended Best Practices

## 1. Team Naming Conventions
Use lowercase letters and hyphens for team names. Follow the pattern: `[app prefix]-[optional subgroup / description]-[scope]` where scope is one of `c` (collaborator), `a` (administrator), or `r` (read-only)

For example, all repositories dealing with the prefix `aws` would have the following three groups:
  - `aws-cloud-c`
  - `aws-cloud-a`
  - `aws-cloud-r`



## 2. Enterprise Identity Management
Team synchronization and provisioning is beyond the scope of this document; however, teams and membership are managed via the Microsoft EntraID identity provider. See [Managing team synchronization for organizations in your enterprise](https://docs.github.com/en/enterprise-cloud@latest/admin/managing-iam/using-saml-for-enterprise-iam/managing-team-synchronization-for-organizations-in-your-enterprise) for more information.

## 3. Repository creation and provisioning
The documentation in this section leverages [IssueOps](https://issue-ops.github.io/docs/) and [IssueForms](https://docs.github.com/en/communities/using-templates-to-encourage-useful-issues-and-pull-requests/configuring-issue-templates-for-your-repository#creating-issue-forms). Once an internal developer portal is configured, it will take the place of IssueOps and IssueForms, and will orchestrate and be orchestrated by GitHub Actions directly. References to IssueOps and IssueForms in this recommendation will allow MUFG to immediately implement required self-service elements in a way that can be leveraged by internal developer portals. 

### New Repository Creation request
Repositories can be managed / created through self-service options by leveraging [IssueOps](https://issue-ops.github.io/docs/) in a special admin request repository or via an internal developer portal. In either approach, automation is used to create initial repository state from a "Golden Path" template, GitHub team assignments, and assign initial topics, including GSI, language, sensitivity, etc. 

Use this [IssueForm template](./team-structure-assets/ISSUE_TEMPLATE/repo-creation.yml) and [GitHub action workflow](./team-structure-assets/workflows/create-repo-from-issue.yml) as a starting point to allow users to request creation of new repositories.  

### New GSI Creation
As application GSIs are required to be created prior to creating repositories, self-service automation must be used to request new GSIs. This [IssueForm template](./team-structure-assets/ISSUE_TEMPLATE/gsi-creation.yml) should be leveraged to update the yaml file that contains GSIs. This yaml file will be used as the source of truth for a [New Repository Creation request](#new-repository-creation-request).

The GitHub action workflow configured to handle this IssueForm will leverage the [GitHub Team REST API](https://docs.github.com/en/rest/teams/teams?apiVersion=2022-11-28#create-a-team) to create teams as specified in the [Team Naming Conventions](#1-team-naming-conventions) section each time a new GSI is created. Membership in these teams will be managed by the configured [Enterprise Identity Management](#2-enterprise-identity-management) solution.

### Adding Repository Topics
Repository [topics](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/classifying-your-repository-with-topics) enable easier discovery and grouping of repositories. While each repository should have no more than 20 topics, the following are recommended at a minumum:
   - Application prefix (GSI, ex `gsi-aws`)
   - Language (ex. `javascript`)
   - Role (ex. `front-end`)
   - Sensitivity (ex. `pci`, `pii`)

Topics will be added automatically when repositories are requested via the [self-service mechanism](#new-repository-creation-request). Internal wikis (ex. Confluence) and README.md documentation of templated repositories can view all repositories related to a given topic by using a specially crafted GitHub search URL: https://github.com/search?q=topic%3Agsi-aws&type=Repositories. In this way, application owners can easily reference the repositories associated with their application / GSI.

### Topic Management
#### **Automation**
   Key topics will be added upon repository creation as per above. 

   Only repository owners can add topics. By leveraging [IssueOps](https://issue-ops.github.io/docs/), [IssueForms](https://docs.github.com/en/communities/using-templates-to-encourage-useful-issues-and-pull-requests/configuring-issue-templates-for-your-repository#creating-issue-forms), and [Organization Webhooks](https://docs.github.com/en/webhooks/types-of-webhooks#organization-webhooks) for [Issues](https://docs.github.com/en/webhooks/webhook-events-and-payloads#issues), members of the repository admin group can be allowed to trigger automation to create topics within a repository. Note that this elevated permission would require [special handling](https://docs.github.com/en/apps/creating-github-apps/authenticating-with-a-github-app/making-authenticated-api-requests-with-a-github-app-in-a-github-actions-workflow) as it is an authenticated API request ([`gh repo edit --add-topic`](https://cli.github.com/manual/gh_repo_edit)).

#### **Documentation**
   Topics can be linked from a dashboard or other reference documentation for convenience. For example, here are public GitHub repositories:
      - Written in [go](https://github.com/search?q=topic%3Ago&type=Repositories)
      - Dealing with [containers](https://github.com/search?q=topic%3Acontainers&type=Repositories)
      - Part of [cncf](https://github.com/search?q=topic%3Acncf&type=Repositories)
      - Part of [gsi-aws](https://github.com/search?q=topic%gsi-aws&type=Repositories)

# References and Additional Reading

- [About teams](https://docs.github.com/en/organizations/organizing-members-into-teams/about-teams)
- [Best practices for organizations and teams using GitHub Enterprise Cloud](https://github.blog/enterprise-software/devops/best-practices-for-organizations-and-teams-using-github-enterprise-cloud/)
- [How to get started with GitHub Enterprise Cloud](https://assets.ctfassets.net/wfutmusr1t3h/ooXuGRtFrKHrFZ8cIdbUC/4333e8014b2e950d9381bdb102415e3a/GitHub-Enterprise-Cloud_ebook.pdf) 