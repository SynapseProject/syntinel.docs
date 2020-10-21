## xxxxxxxx Database Record

Description

### Class Diagram
![xxxxxxxx Database Record](../../resources/draw.io/ClassDiagram-xxxxxxxDbRecord.png)

### JSON Schmea
````json
{
    "$schema": "http://json-schema.org/draft-04/schema#",
    "type": "object",
    "required": [ "id" ],
    "properties": {
        "id": { "type": "string" },
        "actionId": { "type": "string" },
        "newStatus": { "type": "string" },
        "closeSignal": { "type": "boolean" },
        "isValidReply": { "type": "boolean" },
        "sendToChannels": { "type": "boolean" },
        "customMessage": { "type": "object"},
        "message": { "type": "string"},
        "data": { "type": "object" }
    }
}
````

### Field Descriptions

#### **Status**
|Field|Type|Required|Description
|-----|----|--------|-----------
|id|String|Yes|Blah Blah Blah.

### Examples

#### **Sample xxxxxxxx Database Record**

Description

````json
{
  "_id": "_default",
  "defaultChannels": [
    "OpsTeamChannel",
    "OpsSlackChannel"
  ],
  "description": "The default reporter for any signals with no reporter id specified.",
  "isActive": true,
  "name": "Default"
}
````
