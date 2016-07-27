# conversation-starter

Bits and pieces that might be useful for training a Watson Conversation service. This
is just an experiment to see if there are any reusable chunks that would speed up
development of projects that use the Conversation service.

## Training data

It seems likely that Conversation based chat bots will want some common elements. Since
the easiest subset of a workspace to share are intents, due to the CSV import, those
seemed like an obvious place to start. Plus there are already some overlaps in the
sample projects out there to borrow from!

The following rough and ready `jq` command will reverse the CSV import process and
extract intents from an exported workspace JSON file.

```
jq --raw-output '.intents[] | { "intent": (.intent), "example": .examples[].text} | [.example, .intent] | @csv' < workspace.json > intents.csv
```