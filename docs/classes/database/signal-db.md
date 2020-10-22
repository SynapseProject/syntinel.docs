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
             ** Insert Signal Request Json Schema Here **     
        },
        "actions": { "type": "string" },
        "_trace": { "type": "object",
          "additionalProperties": {
             "type": "object",
             "properties": {
               "cueId": {"type": "string" },
               "variables": {"type": "object",
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
|_status|choice of Status|Yes|The current status of the entire signal message.
|_time|Datetime|Yes|The time the message was originally received.
|signal|[Signal](../requests/signal-request.md#signal)|Yes|The original signal message sent from the reporter.
|actions|Dictionary of [ActionType](#actiontype)|No|Actions that have been taken against the signal message by one or more subscribers.
|_trace|Dictionary of Json Object|No|This is the "log" for all actions, updates, results associated with signal message.

#### **ActionType**
|Field|Type|Required|Description
|-----|----|--------|-----------
|cueId|String|Yes|The unique identifier generated for the action received from the subscriber.
|variables|List of [MultiValueVariable](#multivaluevariable)|The variables received from the Cue message (action specified).


#### **MultiValueVariable**
|Field|Type|Required|Description
|-----|----|--------|-----------
|name|String|Yes|The variable name
|values|List of String|No|The value(s) associated with the variable. 


### Examples

#### **Sample Signal Database Record**

Description

````json
{
  "_id": "0X86F3V1P",
  "_isActive": true,
  "_status": "Completed",
  "_time": "2020-10-07T22:28:15.8693223Z",
  "_trace": {
    "0X86F5LKV_SignalReply": {
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
    },
    "0X86GPKOM_Status": {
      "actionId": "0X86G2NAS",
      "closeSignal": false,
      "data": {
        "action": [
          "hibernate"
        ]
      },
      "id": "0X86F3V1P",
      "isValidReply": true,
      "message": "action : hibernate",
      "newStatus": "Completed",
      "sendToChannels": true
    }
  },
  "actions": {
    "0X86G2NAS": {
      "cueId": "ec2",
      "isValid": true,
      "status": "Completed",
      "time": "2020-10-07T22:29:03.4970258Z",
      "variables": [
        {
          "name": "action",
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
            "id": "action",
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
            "id": null,
            "name": "Ignore Alert",
            "type": "button",
            "values": null
          },
          {
            "defaultValue": "disable",
            "description": "Disable this alert.",
            "id": null,
            "name": "Disable Alert",
            "type": "button",
            "values": null
          }
        ],
        "arguments": null,
        "defaultAction": "Perform Action",
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
