# actions

## Contains all the GitHub composite actions for reuse by ASCENDING

### **Getting Started**

- [Using a Composite Action](#using-a-composite-action)
    - [Full Example](#full-example)
- [Cloudformation|cft](#cloudformation)
- [SAM](#sam)
    - [Parameters](#parameters)
- [Ecr Deploy](#ecr-deploy)
- [Stacksets](#stacksets)
    - [Parameters](#parameters-1)

## Using a Composite Action

The key to adding a composite action is the `uses:` key. This will tell GitHub to look into the specified repository for an action.yaml. If like this repo there are multiple action.yaml files, specify the directory as well. Use the `with:` key to add whatever required parameters there are.

### **Marketplace Action (One action.yaml file)**
```yaml
uses: aws-actions/configure-aws-credentials@v1
```
### **Personal Action (Multiple action.yaml files)**
```yaml
uses: ascending-llc/actions/sam@main
```

### Full Example

Just add this to `.github/workflows` under any uniquely named yaml file. Ex: `.github/workflows/main.yaml`

```yaml
name: "Update stacksets on pushes to the main branch"

on:
  push:
    branches:
      - master

jobs:
  buildstackset:
    runs-on: ubuntu-latest
    permissions:
      id-token: write
      contents: read
    steps:
        - name: Checkout
          uses: actions/checkout@v3
        - name: Deploy Stackset
          uses: ascending-llc/actions/stacksets@main
          with:
            stackset_name: "stackset"
            role_to_assume: arn:aws:iam::{account_id}:role/GitHubStackSetAssumeRole
            template_body: "cft/stacksets/some.yaml"
```

## Cloudformation

Deploys a cloudformation template.

## SAM

Deploys a SAM template using the specified parameters. Requires OIDC connection.

### Parameters

- location *
    - Directory of the sam template
- sam_package_config
    - All params for packaging the template
    - Only required if sam_config is set to `false`
- sam_config
    - All required params for deploying the template
    - Default: `true`
- region
    - AWS Region to deploy to
    - Default: `us-east-1`
- role_to_assume *
    - ARN of the role to use for deployment
- python_v
    - Python version to use when deploying the template
    - Default: `3.8`
- config_env
    - Environment in samconfig.toml to deploy to
    - Only required if samconfig.toml doesn't contain default parameters

## Ecr deploy

This uses the OIDC Provider deployed in the cloudformation template to package and deploy and new changes to the specified ecr repo.

## Stacksets

This assumes a role to deploy any stacksets required for the change set.

### Parameters

- stackset_name *
    - Name of the stackset you're deploying
- role_to_assume *
    - Arn of the role to use when deploying the stackset
- region
    - Region to deploy to
    - Default: `us-east-1`
- template_body *
    - Path to the template body
- params
    - Parameters to load into the stackset
    - In `ParameterKey={},ParameterValue={}` format

\* Required