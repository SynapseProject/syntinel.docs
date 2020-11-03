# Amazon Web Services

The Amazon Web Services (AWS) implementation of Syntinel utilizes a combination of API Gateway endpoints, Lambda functions (written in DotNet Core 3.1) and DynamoDB tables.

## Technical Architecture

![Technical Architecture Diagram](../resources/draw.io/TechnicalArchitecture-RelayTier.png)

## Installation

Syntinel can be installed into an AWS account by using one or many cloud formation templates included in the release zip file, located on [GitHub](https://github.com/SynapseProject/syntinel.core.net/releases).

The cloud formation template "cft-syntinel.yaml" creates all the necessary resources required to run Syntinel in your AWS account.  It calls the templates located in the "stacks" directory, passing the require variables into each template.  It assumes you are executing the template with a policy that has the required [permissions](#permissions) to create the resources.  The policy document can be found in the "policies" directory.

### Permissions

Below is a list of the minimum required permissions needed to install Syntinel : 

````yaml
- apigateway:
    - DELETE
    - GET
    - PATCH
    - POST
    - PUT
- cloudformation:
    - CreateStack
    - CreateStackInstances
    - DeleteStack
    - DeleteStackInstances
    - DescribeChangeSet
    - DescribeStackEvents
    - DescribeStacks
    - GetTemplate
    - GetTemplateSummary
    - ListStackInstances
    - ListStackResources
    - ListStacks
    - ListStackSetOperations
    - UpdateStack
    - UpdateStackInstances
    - ValidateTemplate
- dynamodb:
    - CreateTable
    - DeleteTable
    - DescribeTable
    - UpdateTable
- iam:
    - AttachRolePolicy
    - CreatePolicy
    - CreatePolicyVersion
    - CreateRole
    - DeletePolicy
    - DeletePolicyVersion
    - DeleteRole
    - DetachRolePolicy
    - GetPolicy
    - GetRole
    - ListAttachedRolePolicies
    - ListPolicies
    - ListPolicyVersions
    - ListRoles
    - PassRole
    - UpdateRole
- lambda:
    - AddPermission
    - CreateFunction
    - DeleteFunction
    - GetAccountSettings
    - GetFunction
    - GetFunctionConfiguration
    - ListFunctions
    - RemovePermission
    - UpdateFunctionCode
    - UpdateFunctionConfiguration
- s3:
    - GetObject
````

### Running Templates Separately

If you are unable (or unwilling) to execute the single template to deploy all parts of the Syntinel application (due to BoundryPolicies, or the permissions being spread across multiple roles) you can run each template in the "stacks" directory individually in the following order (with the approprite permissions for each): 

- cft-syntinel-init
- cft-syntinel-iam
- cft-syntinel-database
- cft-syntinel-lambda
- cft-syntinel-apigateway

### Template Variables

Most of the variables in the templates allow for customization when certain naming standards are required to be maintained.  The majority of the variables can be left as their default value with the following excpetions : 

- **S3BucketName :** The bucket name where these templates and the lambda source code zip file reside.  Must be in the same region as you intend to deploy Syntinel.
- **S3BucketPrefix :** If you have the Syntinel deployment artifacts in a sub-directory of a bucket, add that directory here.
- **PolicyPermissionBoundry :** If you are required to add a permission boundry on your IAM roles, include that value here.
- **xxxxxStackName :** If running each template separately, some of the templates require the stack name of the previously run templates.  

## DynamoDb

DynamoDb is a No-SQL database offering from Amazon Web Services.  Syntinel creates 5 tables within DynamoDb and stores the objects for each as a JSON document.

### Channels Table

- [Overview](../classes/database/channel-db.md)
- [Json Schema](../classes/database/channel-db.md#json-schmea)
- [Channel Types](../classes/database/channel-db.md#channeltype)
- [Examples](../classes/database/channel-db.md#examples)

### Reporters Table

- [Overview](../classes/database/reporter-db.md)
- [Json Schema](../classes/database/reporter-db.md#json-schmea)
- [Examples](../classes/database/reporter-db.md#examples)

### Router Table

- [Overview](../classes/database/router-db.md)
- [Json Schema](../classes/database/router-db.md#json-schmea)
- [Examples](../classes/database/router-db.md#examples)

### Signals Table

- [Overview](../classes/database/signal-db.md)
- [Json Schema](../classes/database/signal-db.md#json-schmea)
- [Examples](../classes/database/signal-db.md#examples)

### Templates Table

- [Overview](../classes/database/template-db.md)
- [Json Schema](../classes/database/template-db.md#json-schmea)
- [Examples](../classes/database/template-db.md#examples)
