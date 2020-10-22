## Router Database Record

Syntinel uses the Router Database record to look up alternative channels (routing) for the message.  It uses the 2 fields from the Signal (routerId and routerType) and if a record is found matching that Id and Type, the channels specified here replace the defaultChannels in the reporter.

### Class Diagram
![Router Database Record](../../resources/draw.io/ClassDiagram-RouterDbRecord.png)

### JSON Schmea
````json
{
    "$schema": "http://json-schema.org/draft-04/schema#",
    "type": "object",
    "required": [ "_id", "_type" ],
    "properties": {
        "_id": { "type": "string" },
        "_type": { "type": "string" },
        "channels": { "type": "array", 
            "items": {"type": "string"} 
        }
    }
}
````

### Field Descriptions

#### **Status**
|Field|Type|Required|Description
|-----|----|--------|-----------
|_id|String|Yes|The Id to route the signal by (the signalId field in the [Signal](./signal-db.md) message)
|_type|String|Yes|A User defined "type" to route the signal by (the signalType field in the [Signal](./signal-db.md) message)
|channels|List of String|No|A list of Channel Id's to send the Signal message to when it matches the Id and Type provided.

### Examples

#### **Sample Router Database Records**

In the records below when the signal message provides the following fields ( {"signalId": "dev", "signalType", "environment}) the defaultChannels in the [reporter](./reporter.md) record will be overwritten with the "Dev" channels below.

````json
{
  "_id": "dev",
  "_type": "environment",
  "channels": [
    "DevSlackChannel",
    "DevTeamsChannel"
  ]
}
````

````json
{
  "_id": "test",
  "_type": "environment",
  "channels": [
    "TestSlackChannel",
    "TestTeamsChannel"
  ]
}
````
