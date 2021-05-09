# Execution Context

When JavaScript engine runs your code, it creates an execution context.

The first execution context that gets created is called **`global execution context`**.
Even there is no code to be executed, JavaScript engine still creates it.
Inside of the global execution context, has two properties: `window`(if running in Node environment, it's just called `global`) which points to the `global object`  and `this` which points to the `window` object.

Each execution context is going to have two phases: the `creation` phase and then later on the `execution` phase.

