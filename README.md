![Sync Issue to Azure DevOps work item](https://github.com/danhellem/github-actions-issue-to-work-item/workflows/Sync%20Issue%20to%20Azure%20DevOps%20work%20item/badge.svg?event=issues)

[![Git](https://app.soluble.cloud/api/v1/public/badges/57ef1b2f-b2be-425c-b5b8-58870a2a0d73.svg?orgId=650162616495)](https://app.soluble.cloud/repos/details/github.com/james-leha/github-actions-issue-to-work-item?orgId=650162616495)  

# Sync GitHub issue to Azure DevOps work item

Create work item in Azure DevOps when a GitHub Issue is created

Update Azure DevOps work item when a GitHub Issue is updated

![alt text](./assets/demo.gif "animated demo")

## Outputs

### `id`

The id of the Work Item created or updated

## Example usage

1. Add a secret named `ADO_PERSONAL_ACCESS_TOKEN` containing an [Azure Personal Access Token](https://docs.microsoft.com/en-us/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate) with "read & write" permission for Work Items

2. Add an optional secret named `GH_PERSONAL_ACCESS_TOKEN` containing a [GitHub Personal Access Token](https://help.github.com/en/enterprise/2.17/user/github/authenticating-to-github/creating-a-personal-access-token-for-the-command-line) with "repo" permissions. See optional information below.

3. Install the [Azure Boards App](https://github.com/marketplace/azure-boards) from the GitHub Marketplace

4. Add a workflow file which responds to issue events.

   - Set Azure DevOps organization and project details.
   - Set specific work item type settings (type, new state, closed state)

   Optional Env Variables

   - `ado_area_path`: To set a specific area path you want your work items created in. If providing a full qualified path such as `area\sub_area`, then be sure to use the format of: `ado_area_path: "area\\area"` to avoid parsing failures.
   - `ado_iteration_path`: To set a specific iteration path you want your work items created in. If providing a full qualified path such as `iteration\sub iteration`, then be sure to use the format of: `ado_iteration_path: "iteration\\iteration"` to avoid parsing failures.
   - `github_token`: Used to update the Issue with AB# syntax to link the work item to the issue. This will only work if the project is configured to use the [GitHub Azure Boards](https://github.com/marketplace/azure-boards) app.
   - `ado_bypassrules`: Used to bypass any rules on the form to ensure the work item gets created in Azure DevOps. However, some organizations getting bypassrules permissions for the token owner can go against policy. By default the bypassrules will be set to false. If you have rules on your form that prevent the work item to be created with just Title and Description, then you will need to set to true.

```yaml
name: Sync issue to Azure DevOps work item

on:
  issues:
    types:
      [opened, edited, deleted, closed, reopened, labeled, unlabeled, assigned]

jobs:
  alert:
    runs-on: ubuntu-latest
    steps:
      - uses: danhellem/github-actions-issue-to-work-item@master
        env:
          ado_token: "${{ secrets.ADO_PERSONAL_ACCESS_TOKEN }}"
          github_token: "${{ secrets.GH_PERSONAL_ACCESS_TOKEN }}"
          ado_organization: "ado_organization_name"
          ado_project: "your_project_name"
          ado_area_path: "optional_area_path\\optional_area_path"
          ado_iteration_path: "optional_iteration_path\\optional_iteration_path"
          ado_wit: "User Story"
          ado_new_state: "New"
          ado_active_state: "Active"
          ado_close_state: "Closed"
          ado_bypassrules: true
```
