# Welcome to Syntinel

Syntinel is a notification engine that communicates messages to a variety of endpoints and can take actions based on replies to those messages.

![Architecture Components](./images/Architecture-Components.png)

## Glossary

**Reporters** - Any script, program, service, etc...  that is responsible for sending messages into Syntinel.

**Signal** - The message sent from Reporters into Syntinel.

**Subscribers** - Client applications (Slack, Teams, Email, etc..) that receive Signals from Syntinel and display them to end clients for review or request actions to be taken.

**Cue** - A reply to a Signal, send from the Subscriber back into Syntinel.  This can contain information on which (if any) actions to take based on the Signal.

**Resolvers** - Appliations, programs or services that receive the Signal and Cue messages and take action based on those messages. (Ex: stop a service, restart a server, etc...)

**Signal Id** - A unique, 10-character string that identifies the original Signal message received.

**Action Id** - A unique, 10-character string that identifies an individual reply (Cue) to a Signal message from a subscriber.

