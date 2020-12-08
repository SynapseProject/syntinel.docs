# AWS Step Function Resolver

The AWS Step Function Resolver forwards the ResolverRequest to an AWS Step Function.

## Resolver Input

Syntinel calls the step function named in the resolver config section by passing the [ResolverRequest](./overview.md#resolverrequest) into the step function as a JSON string. 

See [Resolver Input](./overview.md#resolver-input) section of the [Resolvers](./overview.md) page for class details.

## Resolver Config Section

### Json Schema

````json
{
  "$schema": "http://json-schema.org/draft-04/schema#",
  "type": "object",
  "required": [ "arn" ],
  "properties": {
    "arn": { "type": "string" },
    "useDefaultName": { "type": "bool" },
    "executionName": { "type": "string" }
  }
}
````

### Field Descriptions

|Field|Type|Required|Description
|-----|----|--------|-----------
|arn|String|Yes|The Amazon Resource Name (ARN) for the step function that will be called.
|useDefaultName|Boolean|No|Flag to indicate that the default execution name from Amazon should be used for this execution.  (*Default Value = false*)
|executionName|String|No|Name to be used as the execution name for this step function execution.  NOTE:  This value must be unique.  When provided, it is up to the caller to ensure uniqueness.

### Execution Names

Each run of an Amazon Step Function is given a unique execution name.  By default, Syntinel provides a name in the format **syntinel-*SIGNALID*-*ACTIONID***, which should be unique for each action sent in.

The config option *useDefaultName* tells Syntinel to use the default value from Amazone for the execution name, which is a GUID.

Finally, if you want to provide your own execution name for each run of a step function, you can provide that name in the *executionName* config option.    If you use this option, it is up to you to ensure uniqueness, as provididing a duplicate execution name to a step function call will result in an error.

### Examples

````json
{
    "arn": "arn:aws:states:us-east-1:123456789012:stateMachine:HelloWorld"
}
````

````json
{
    "arn": "arn:aws:states:us-east-1:123456789012:stateMachine:HelloWorld",
    "useDefaultName": true
}
````

````json
{
    "arn": "arn:aws:states:us-east-1:123456789012:stateMachine:HelloWorld",
    "executionName": "MyCustomExecutionName-001"
}
````
