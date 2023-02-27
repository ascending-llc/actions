# actions

## Contains all the GitHub composite actions for reuse by ASCENDING

### **Getting Started**

- [Cloudformation|cft](#cloudformation)
- [Ecr Deploy](#ecr-deploy)
- [Stacksets](#stacksets)

## Cloudformation

Deploys a cloudformation template holding a OIDCProvider.
This allows the GitHub tokens OIDCProvider to assume pre-specified roles in an AWS Account.

## Ecr deploy

This uses the OIDC Provider deployed in the cloudformation template to package and deploy and new changes to the specified ecr repo.

## Stacksets

This assumes a role to deploy any stacksets required for the change set.
