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

## 2. SCIM / Membership Management
GitHub Teams should be [managed from an identity provider](https://docs.github.com/en/enterprise-cloud@latest/admin/managing-iam/provisioning-user-accounts-with-scim/managing-team-memberships-with-identity-provider-groups) using identity provider groups (ex. Microsoft Entra ID) and provisioned / updated automatically using SCIM. This means that all of the required teams as specified in [Team Naming Conventions](#1-team-naming-conventions) above must first exist in the corresponding IDP.

## 3. Repository creation and provisioning

### New Repository Creation
Repositories can be managed / created through self-service options by leveraging [IssueOps](https://issue-ops.github.io/docs/) in a special admin request repository or via an internal developer portal. In either approach, automation is used to create initial repository state from a "Golden Path" template, GitHub team assignments, and initial topics. 

### Adding Repository Topics
Repository [topics](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/classifying-your-repository-with-topics) enable easier discovery and grouping of repositories. While each repository should have no more than 20 topics, the following are recommended at a minumum:
   - Application prefix (GSI, ex `aws`)
   - Language (ex. `javascript`)
   - Role (ex. `front-end`)
   - Sensitivity (ex. `pci`, `pii`)

### Topic Management
#### **Automation**
   Only repository owners can add topics. By leveraging [IssueOps](https://issue-ops.github.io/docs/), [IssueForms](https://docs.github.com/en/communities/using-templates-to-encourage-useful-issues-and-pull-requests/configuring-issue-templates-for-your-repository#creating-issue-forms), and [Organization Webhooks](https://docs.github.com/en/webhooks/types-of-webhooks#organization-webhooks) for [Issues](https://docs.github.com/en/webhooks/webhook-events-and-payloads#issues), members of the repository admin group can be allowed to trigger automation to create topics within a repository. Note that this elevated permission would require [special handling](https://docs.github.com/en/apps/creating-github-apps/authenticating-with-a-github-app/making-authenticated-api-requests-with-a-github-app-in-a-github-actions-workflow) as it is an authenticated API request ([`gh repo edit --add-topic`](https://cli.github.com/manual/gh_repo_edit)).

#### **Documentation**
   Topics can be linked from a dashboard or other reference documentation for convenience. For example, here are public GitHub repositories:
      - Written in [go](https://github.com/search?q=topic%3Ago&type=Repositories)
      - Dealing with [containers](https://github.com/search?q=topic%3Acontainers&type=Repositories)
      - Part of [cncf](https://github.com/search?q=topic%3Acncf&type=Repositories)

# Key Decisions and Recommendations

| Decision ID | Decision Description | Rationale / Justification |
| --- | --- | --- |
| 1 | Use lowercase with hyphens for team names | Improves readability and consistency across the organization |
| 2 | Use repository topics for grouping | Enables efficient repository discovery and management |
| 3 | Create and enforce team naming standards | Ensures consistency and clarity in team organization |
| 5 | Implement topic automation | Allows for self-service while maintaining least privilege |

# References and Additional Reading

- [About teams](https://docs.github.com/en/organizations/organizing-members-into-teams/about-teams)
- [Best practices for organizations and teams using GitHub Enterprise Cloud](https://github.blog/enterprise-software/devops/best-practices-for-organizations-and-teams-using-github-enterprise-cloud/)
- [How to get started with GitHub Enterprise Cloud](https://assets.ctfassets.net/wfutmusr1t3h/ooXuGRtFrKHrFZ8cIdbUC/4333e8014b2e950d9381bdb102415e3a/GitHub-Enterprise-Cloud_ebook.pdf) 