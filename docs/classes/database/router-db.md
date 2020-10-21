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
	"id": "0X3N9WBKR", 
	"actionId": "0X3NB1MZS", 
	"newStatus": "Error", 
	"closeSignal": true, 
	"isValidReply": true,
    "sendToChannels": true,
    "message": "The Server Is Already Terminated.",
    "data": {
        "instance": "i-8675309JENNY",
        "something": "else",
        "more": ["data", "can", "go", "here", "too"]
    }
}
````
