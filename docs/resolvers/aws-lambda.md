# AWS Lambda Resolver

The AWS Lambda Resolver forwards the ResolverRequest to an AWS Lambda Function.

## Resolver Input

Syntinel calls the lambda function named in the resolver config section by passing the [ResolverRequest](./overview.md#resolverrequest) into the lambda function as a JSON string. 

See [Resolver Input](./overview.md#resolver-input) section of the [Resolvers](./overview.md) page for class details.

## Resolver Config Section

### Json Schema

````json
{
  "$schema": "http://json-schema.org/draft-04/schema#",
  "type": "object",
  "required": [ "arn" ],
  "properties": {
    "arn": { "type": "string" }
  }
}
````

### Field Descriptions

|Field|Type|Required|Description
|-----|----|--------|-----------
|arn|String|Yes|The Amazon Resource Name (ARN) for the lambda function that will be called.

### Example

````json
{
    "arn": "arn:aws:lambda:us-east-1:123456789012:function:echo"
}
````
