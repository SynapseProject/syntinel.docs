# AWS Lambda Resolver

The AWS Lambda Resolver forwards the ResolverRequest to an AWS Lambda Function.

## Resolver Input

Syntinel calls the lambda function named in the resolver config section by passing the [ResolverRequest](../resolvers.md#resolverrequest) into the lambda function as a JSON string. 

See [Resolver Input](../resolvers.md#resolver-input) section of the [Resolvers](../resolvers.md) page for class details.

## Resolver Config Section

### Json Schema

````json
{
  "$schema": "http://json-schema.org/draft-04/schema#",
  "type": "object",
  "required": [ "name" ],
  "properties": {
    "name": { "type": "string" },
    "region": { "type": "string" }
  }
}
````

### Field Descriptions

|Field|Type|Required|Description
|-----|----|--------|-----------
|name|String|Yes|The name of the lambda function to be called (case-sensitive)
|region|String|No|The AWS Region string where the lambda is located.  *(Example: us-east-1)*

### Example

````json
{
    "name": "MyLambdaFunction",
    "region": "us-east-1"
}
````
