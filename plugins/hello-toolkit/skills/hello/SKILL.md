---
name: hello
description: Greets the user by name and briefly lists what the hello-toolkit plugin contains.
argument-hint: <name>
disable-model-invocation: true
---

# Hello

The user invoked the command with arguments: $ARGUMENTS

1. Greet the user by the name from the arguments (if there is no name, just say "Hello!").
2. Briefly list what the hello-toolkit plugin contains:
   - skill `commit-message` - helps write commit messages;
   - command `/hello-toolkit:hello` - this command;
   - subagent `code-explainer` - explains code in plain language;
   - hook `SessionStart` - a greeting when a session starts.
