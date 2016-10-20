# DevOps Project

This is a repo for playing about with some DevOps concepts and tools.

The directory structure is as follows:

```
|- README.md
|- Vagrantfile (for local VM)
`- work
   |- apps
      |-api (contains the appcode for an API)
```

The general idea is this:

- The system will consist of multiple services, each in its own folder under `/apps`.
- The vagrant workstation required to work on the system will be bundled with the project, by means of a Vagranfile.
- The individual apps will be configured to run inside containers, which will be immutable.
- The containers will be configured only with shell scripts and dockerfiles (maybe?).
- The dev workstation will be as similar as possible to the production infrastructure (AWS **and** DigtalOcean), all of which will be configured with Chef, and tested with TestKitchen and ServerSpec.
- The production infrastructure will be configured with Terraform and Chef

Initially, the production infrastructure will consist of:

- A single AWS/DO instance running the containerised api, with nginx as a reverse proxy (this should allow for zero-downtime deploys by spinning up a new container and then switching the proxy over). This AWS instance will also have a MongoDB container, which should not be available externally.

In the future, I might look to add:

- The ability to scale these horizontally, with a second AWS/DO instance as a load balancer and cache. This will require some thinking about the MongoDB container, and how best to provide the caching.
- A _services_ adaptor as a microservice, which provides a predictable interface for making calls to multiple external APIs (Evernote, Dropbox, Slack etc) and handles rate limiting and [circuit-breaking](https://en.wikipedia.org/wiki/Circuit_breaker_design_pattern) etc, deployed in the same way as the api service.
- A node.js _front-end_ service, deployed in the same way as the api service.
- Some for of message queuing, just so I can have a play with it.

## Getting Started

1. Ensure you have VirtualBox and vagrant installed on your system.
2. Clone down this repo and spin up the VM with `vagrant up`.
