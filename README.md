# conversation-starter

Bits and pieces that might be useful for training a Watson Conversation service. This
is just an experiment to see if there are any reusable chunks that would speed up
development of projects that use the Conversation service.

## Training data

### Intents

It seems likely that Conversation based chat bots will want some common elements. Since
the easiest subset of a workspace to share are intents, due to the CSV import, those
seemed like an obvious place to start. Plus there are already some overlaps in the
sample projects out there to borrow from!

The following rough and ready `jq` command will reverse the CSV import process and
extract intents from an exported workspace JSON file.

```
jq --raw-output '.intents[] | { "intent": (.intent), "example": .examples[].text} | [.example, .intent] | @csv' < exported-workspace.json > intents.csv
```

### Template workspaces

For now there is just a basic starter workspace that just contains a common set of intents and an
example conversation start dialog node to provide a simple starting point.

Possible additional workspaces:
 - small hello world example
 - chit chat example

I used the following `jq` command to remove some of the unnecessary content from exported
workspaces ready for sharing:

```
jq 'recurse(.intents[]?, .examples[]?, .entities[]?, .dialog_nodes[]?) |= del(.created, .created_by, .metadata, .modified, .modified_by, .workspace_id)' < exported-workspace.json > workspace.json
```