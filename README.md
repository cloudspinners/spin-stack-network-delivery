
I'm using this project to explore ways to structure infrastructure projects. See https://cloudspin.io for background. This isn't intended to be usable, it's purely for me to play around.

The idea of this "delivery" project is that it doesn't define any stacks, but has the configuration to assemble stacks used to deliver a particular stack. In this case, the stack is the "network" stack.

The separate `spin-stack-network` project has the definition code for the network stack, and is used to generate an artefact. This artefact can be used to create instances of the stack in different environments.

So this project needs to orchestrate the creation of some supporting stacks:

- Statebucket (or one statebucket per environment?)
- Pipeline (the stages for testing and delivering the network stack)
- IAM roles (for managing the pipeline, statebucket, and network stack instances)

Each of these stacks is defined in its own project.
