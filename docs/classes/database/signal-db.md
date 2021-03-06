## Signal Database Record

The Signal Database Record is created by Syntinel to track all actions and statuses associated with an individual [Signal](../requests/signal-request.md) message received from a reporter.

### Class Diagram
![Signal Database Record](../../resources/draw.io/ClassDiagram-SignalDbRecord.png)

Referenced Classes: 

- [Signal Message](../requests/signal-request.md#class-diagram)

### JSON Schmea
````json
{
    "$schema": "http://json-schema.org/draft-04/schema#",
    "type": "object",
    "required": [ "_id" ],
    "properties": {
        "_id": { "type": "string" },
        "_isActive": { "type": "boolean" },
        "_status": { "type": "string", "pattern": "^New|Sent Received|SentToResolver|Completed|Error|Invalid$" },
        "_time": { "type": "string", "format": "date" },
        "signal": { "type": "object"
            *** Insert Signal Schema Here ***
        },
        "trace": { "type": "array",
          "required": ["type"],
          "properties": {
           	"type": { "type": "string"},
            "object": { "type": "object" }
          }
        },
        "actions": { "type": "object",
          "additionalProperties": {
             "type": "object",
             "properties": {
               "cueId": {"type": "string" },
               "variables": {"type": "array",
                 "required": [ "name", "values" ],
                 "properties": {
                   "name": { "type": "string" },
                   "values": { 
                     "type": "array",
                     "items": { "type": "string" }
                   }
                 }
               },
               "isValid": {"type": "boolean" },
               "status": {"type": "string", "pattern": "^New|Sent Received|SentToResolver|Completed|Error|Invalid$" },
               "statusMessage": {"type": "string" },
               "time": {"type": "string", "format": "date"}
             }
          }
        }
    }
}


````

### Field Descriptions

#### **SignalDbRecord**
|Field|Type|Required|Description
|-----|----|--------|-----------
|_id|String|Yes|The unique identifier generated for the original signal message sent from the reporter.
|_isActive|Boolean|Yes|Inidicates whether or not a signal message can still have actions performed against it.
|_status|choice of [StatusType](../requests/status-request.md#statustype)|Yes|The current status of the entire signal message.
|_time|Datetime|Yes|The time the message was originally received.
|signal|[Signal](../requests/signal-request.md#signal)|Yes|The original signal message sent from the reporter.
|actions|Dictionary of [ActionType](#actiontype)|No|Actions that have been taken against the signal message by one or more subscribers.
|_trace|List of [TraceObject](#traceobject)|No|This is the "log" for all actions, updates, results associated with signal message.

#### **ActionType**
|Field|Type|Required|Description
|-----|----|--------|-----------
|cueId|String|Yes|The unique identifier generated for the action received from the subscriber.
|variables|List of [MultiValueVariable](#multivaluevariable)|Maybe|The variables received from the Cue message (action specified).


#### **MultiValueVariable**
|Field|Type|Required|Description
|-----|----|--------|-----------
|name|String|Yes|The variable name
|values|List of String|No|The value(s) associated with the variable. 

#### **TraceObject**
|Field|Type|Required|Description
|-----|----|--------|-----------
|type|String|Yes|The object's type.
|object|JSON Object|No|The Json Serialized version of the object. 


### Examples

#### **Sample Signal Database Record**

The message below represents a resolver that monitors server uptime and asks subscribers if they would like to take action against the server.  Things of note : 

- The "_trace" item "SignalReply" is the response sent back to the reporter.  The "Status" item is the status reported back from the resolver  (in this case, a simple "[echo](../../resolvers/echo.md)" of the variables sent in.)
- The "actions" section shows a single action of "hibernate" was specified against the server.
- The "signal" item is the original signal message sent into Sytinel.  If the original message was using router info and/or templates, this will show the "final" message after those were applied.


````json
{
  "_id": "0X86F3V1P",
  "_isActive": true,
  "_status": "Completed",
  "_time": "2020-10-07T22:28:15.8693223Z",
  "_trace": [
    {
      "type": "SignalReply",
      "object": {
          "id": "0X86F3V1P",
          "results": [
            {
              "channelId": "DevSlackChannel",
              "channelType": "teams",
              "code": "Success",
              "message": "Success"
            },
            {
              "channelId": "DevTeamsChannel",
              "channelType": "slack",
              "code": "Success",
              "message": "Success"
            }
          ],
          "statusCode": "Success",
          "time": "2020-10-07T22:28:08.4267512Z"
      }
    },
    {
      "type": "Status",
      "object": {
          "actionId": "0X86G2NAS",
          "closeSignal": false,
          "data": {
            "action": [
              "hibernate"
            ]
          },
          "id": "0X86F3V1P",
          "isValidReply": true,
          "message": "ec2-action : hibernate",
          "newStatus": "Completed",
          "sendToChannels": true
      }
    }
  ],
  "actions": {
    "0X86G2NAS": {
      "cueId": "ec2",
      "isValid": true,
      "status": "Completed",
      "time": "2020-10-07T22:29:03.4970258Z",
      "variables": [
        {
          "name": "ec2-action",
          "values": [
            "hibernate"
          ]
        }
      ]
    }
  },
  "signal": {
    "cues": {
      "ec2": {
        "actions": [
          {
            "defaultValue": "stop",
            "description": "Choose action to take against EC2 instances.",
            "id": "ec2-action",
            "name": "Perform Action",
            "type": "choice",
            "values": {
              "hibernate": "Stop and Hibernate Instance",
              "reboot": "Reboot Instance",
              "stop": "Stop Instance",
              "terminate": "Terminate Instance"
            }
          },
          {
            "defaultValue": "ignore",
            "description": "Ignore this alert.",
            "id": "signal-action",
            "name": "Ignore Alert",
            "type": "button",
            "values": null
          },
          {
            "defaultValue": "disable",
            "description": "Disable this alert.",
            "id": "signal-action",
            "name": "Disable Alert",
            "type": "button",
            "values": null
          }
        ],
        "arguments": null,
        "defaultAction": "ec2-action",
        "description": "Server [i-888888888888] has been running for 7 days.  Would you like to take action against it?",
        "inputs": [

        ],
        "name": "EC2 Usage",
        "resolver": {
          "config": {
            "instances": [
              "i-888888888888"
            ]
          },
          "name": "Echo",
          "notify": true
        },
        "template": null
      }
    },
    "defaultCue": null,
    "defaultCueTimeout": 0,
    "description": "EC2 Utilization Montior",
    "includeId": true,
    "maxReplies": 1,
    "name": "Utilization",
    "reporterId": "_default",
    "routerId": "dev",
    "routerType": "environment"
  }
}
````
