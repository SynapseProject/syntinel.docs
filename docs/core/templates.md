# Templates

Syntinel Templates are used to simplify other objects by putting the common items into a single "template" and only exposing the parts that might need to change.

For example, if you have a reporter than sends the same signal message for every event, but the only thing different between those messages is the "server name", you can define the message as a template and then the only bit of information the reporter would have to pass in would be the server name for that specific event.

## Template Types

Currently, there are 2 types of templates supported by Syntinel :

- [CueOption](../classes/requests/signal-request.md#cueoption) (The "actionable" part of a signal message.)
- [Channel Database Record]()


## CueOption Templates

[CueOption](../classes/requests/signal-request.md#cueoption) templates let you simplify the signal message coming from the reporter by defining a portion of the signal message as a "template" and exposing only the relevant variables that need replacing.

### Example
In the signal message below, the only piece of information that would change between each event is the ec2 instance id.

**Signal Message (Without Using A Template)**
```json
{
  "name": "Utilization",
  "description": "EC2 Utilization Montior",
  "maxReplies": 1,
  "reporterId": "_default",
  "cues": {
    "ec2": {
      "name": "EC2 Usage",
      "description": "Server [i-888888888888] has been running for 7 days.  Would you like to take action against it?",
      "resolver": {
        "name": "AWSLambda",
        "notify": true,
        "config": {
          "name": "my-lambda-function",
          "instances": [
              "i-888888888888"
          ]
        }
      },
      "actions": [
        {
          "name": "Perform Action",
          "id": "action",
          "description": "Choose action to take against EC2 instances.",
          "type": "choice",
          "values": {
            "stop": "Stop Instance",
            "terminate": "Terminate Instance",
            "reboot": "Reboot Instance",
            "hibernate": "Stop and Hibernate Instance"
          },
          "defaultValue": "stop"
        },
        {
          "name": "Ignore Alert",
          "description": "Ignore this alert.",
          "type": "button",
          "defaultValue": "ignore"
        },
        {
          "name": "Disable Alert",
          "description": "Disable this alert.",
          "type": "button",
          "defaultValue": "disable"
        }
      ],
      "defaultAction": "Perform Action"
    }
  }
}
```

The signal message can be simplified to the message below when it calls an existing [CueOption](../classes/requests/signal-request.md#cueoption) template stored in Syntinel.

**Signal Message (Using a Template)**
```json
{
  "name": "Utilization",
  "description": "EC2 Utilization Montior",
  "maxReplies": 1,
  "reporterId": "_default",
  "cues": {
    "ec2": {
      "template": "ec2-utilization-template",
      "arguments": {
        "instance": "i-8675309JENNY",
        "notify": "false"
      }
    }
  }
}
```

Where the only information required is the "instance" and an optional "notify" flag if you want to overide the default notification value of "true".

**CueOption Tempate Database Record**
```json
{
  "_id": "ec2-utilization-template",
  "_type": "CueOption",
  "parameters": {
    "instance": [
      {
        "path": "description",
        "replace": "INSTANCE_ID"
      },
      {
        "path": "resolver.config.instances[0]"
      }
    ],
    "notify": [
      {
        "path": "resolver.notify"
      }
    ]
  },
  "template": {
    "name": "EC2 Usage",
    "description": "Server [INSTANCE_ID] has been running for 7 days.  Would you like to take action against it?",
    "resolver": {
    "name": "AWSLambda",
    "notify": true,
    "config": {
        "name": "my-lambda-function",
        "instances": [
            ""
        ]
    }
    },
    "actions": [
    {
        "name": "Perform Action",
        "id": "action",
        "description": "Choose action to take against EC2 instances.",
        "type": "choice",
        "values": {
        "stop": "Stop Instance",
        "terminate": "Terminate Instance",
        "reboot": "Reboot Instance",
        "hibernate": "Stop and Hibernate Instance"
        },
        "defaultValue": "stop"
    },
    {
        "name": "Ignore Alert",
        "description": "Ignore this alert.",
        "type": "button",
        "defaultValue": "ignore"
    },
    {
        "name": "Disable Alert",
        "description": "Disable this alert.",
        "type": "button",
        "defaultValue": "disable"
    }
    ],
    "defaultAction": "Perform Action"
  }
}
```

## Channel Template

Channel templates allow you to define common items that apply to all channels (for example, the "actionUrl" for Teams channels) while exposing only the parts of the channel that change.

This allows for quick updates in the event a configuration changes across all channels (if we had to re-deploy syntinel in AWS and need to update the API Gateway for example).

### Example

In the channel definition below, the actionUrl would have to be included in every single channel that uses teams, even though every teams config replies to the same URL.   This creates a maintenance nightmare if that URL ever changes.


**Channel Database Record (No Template)**

```json
{
    "_id": "_default",
    "name": "Default Teams Channel",
    "type": "teams",
    "description": "The default channel used for all Teams subscribers.",
    "isActive": true,
    "target": "https://outlook.office.com/webhook/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx@xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/IncomingWebhook/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "config": {
      "actionUrl": "https://1234567890.execute-api.us-east-2.amazonaws.com/syntinel/cue/teams"
    }
  }

```

Using a channel template instead allows you to only provide the information specific to this channel (mostly just the target).

**Channel Database Record (With Template)**

```json
{
  "_id": "Accounting-Teams",
  "template": "teams",
  "arguments": {
    "name": "Accounting",
    "description": "The Accounting Group's Teams Channel",
    "target": "https://outlook.office.com/webhook/aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa@aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa/IncomingWebhook/aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa/aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa"
  }
}
```

And lets you keep the information that is common amongst all channels of that type in a single location (actionUrl for example).

**Channel Template Database Record**

```json
{
  "_id": "teams",
  "_type": "Channel",
  "parameters": {
    "description": [
      {
        "path": "description",
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
    "name": [
      {
        "path": "name"
      }
    ]
  },
  "template": {
    "name": "Custom Team Channel",
    "type": "teams",
    "description": "A custom channel used for Teams subscribers.",
    "isActive": true,
    "target": "https://outlook.office.com/webhook/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx@xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/IncomingWebhook/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "config": {
      "actionUrl": "https://1234567890.execute-api.us-east-2.amazonaws.com/syntinel/cue/teams"
    }
  }
}
```