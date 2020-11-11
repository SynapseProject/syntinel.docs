## Template Database Record

The Template Database Record stores all the various type of templates that can be used throughout Syntinel.  For more information on how Templates are used, see the [Templates](../../core/templates.md) page.

### Class Diagram
![Template Database Record](../../resources/draw.io/ClassDiagram-TemplateDbRecord.png)

### JSON Schmea
````json
{
    "$schema": "http://json-schema.org/draft-04/schema#",
    "type": "object",
    "required": [ "_id", "_type" ],
    "properties": {
      "_id": { "type": "string" },
      "_type": { "type": "string" },
      "template": { "type": "object" },
      "parameters" : { "type": "object",
        "additionalProperties": { "type": "array",
          "required": ["path"],
          "properties": {
            "path": { "type": "string" },
            "replace": { "type": "string" }
          }          
        }
      }

    }
}
````

### Field Descriptions

#### **Status**
|Field|Type|Required|Description
|-----|----|--------|-----------
|_id|String|Yes|The unique identifier (per type) for the template to be used.
|_type|String|Yes|The type of template this record represents.
|template|Json Object|Yes|The template itself.  See the [Templates](../../core/templates.md) page for a description of each type and the proper Json format.
|parameters|List of [TemplateVariableType](#templatevariabletype)|No|List of parameters the can replace values within the template itself.

#### **TemplateVariableType**
|Field|Type|Required|Description
|-----|----|--------|-----------
|path|String|Yes|The J-Path to the part of the template to replace with the value passed. 
|replace|String|No|The sub-string at the J-Path location to replace (instead of replacing the entire value).

### Examples

#### **Sample Signal Template Database Record**

A Signal template that can be used to query what actions to take on an Amazon EC2 instance.  The only fields that would change between alerts would be the Amazon Instnace Id and an optional override of the "notify" flag if you didn't want the status reported back to the original channels.

````json
{
  "_id": "ec2-utilization",
  "_type": "Signal",
  "parameters": {
    "instance": [
      {
        "path": "cues.ec2.description",
        "replace": "INSTANCE_ID"
      },
      {
        "path": "cues.ec2.resolver.config.instances[0]"
      }
    ],
    "notify": [
      {
        "path": "cues.ec2.resolver.notify"
      }
    ]
  },
  "template": {
    "name": "Utilization",
    "description": "EC2 Utilization Montior",
    "includeId": true,
    "maxReplies": 1,
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
            "name": "Ignore Alert",
            "type": "button"
          },
          {
            "defaultValue": "disable",
            "description": "Disable this alert.",
            "name": "Disable Alert",
            "type": "button"
          }
        ],
        "defaultAction": "Perform Action",
        "description": "Server [INSTANCE_ID] has been running for 7 days.  Would you like to take action against it?",
        "inputs": [
          {
            "defaultValue": "Default Comment Value Here",
            "description": "Choose action to take against EC2 instances.",
            "id": "comment",
            "name": "Enter Comment",
            "type": "text"
          }
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
        }
      }
    },
    "defaultCue": "ec2-template",
    "defaultCueTimeout": 4320
  }
}
````

#### **Sample CueOption Template Database Record**

A simple CueOption that will use the "Name" and "Description" field of a CueOption to send a simple alert.

````json
{
  "_id": "alert",
  "_type": "CueOption",
  "parameters": {
    "body": [
      {
        "path": "description"
      }
    ],
    "title": [
      {
        "path": "name"
      }
    ]
  },
  "template": {
    "description": "Message Body",
    "name": "Message Title"
  }
}
````

#### **Sample Channel Template Database Record**

A Microsoft Teams Channel template that contains the "reply to" url in the config section and allows the signal to override the target URL as well as other variables in the signal message.

````json
{
  "_id": "teams",
  "_type": "ChannelDbType",
  "parameters": {
    "channelName": [
      {
        "path": "description",
        "replace": "CHANNEL_NAME"
      },
      {
        "path": "name",
        "replace": "CHANNEL_NAME"
      }
    ],
    "isActive": [
      {
        "path": "isActive"
      }
    ],
    "target": [
      {
        "path": "target"
      }
    ],
    "teamName": [
      {
        "path": "description",
        "replace": "TEAM_NAME"
      },
      {
        "path": "name",
        "replace": "TEAM_NAME"
      }
    ]
  },
  "template": {
    "config": {
      "actionUrl": "https://xxxxxxxxxx.execute-api.us-east-2.amazonaws.com/syntinel/cue/teams"
    },
    "description": "The [CHANNEL_NAME] channel in the [TEAM_NAME] team.",
    "isActive": true,
    "name": "CHANNEL_NAME - TEAM_NAME",
    "target": "https://outlook.office.com/webhook/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx0@xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxx/IncomingWebhook/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "type": "teams"
  }
}
````