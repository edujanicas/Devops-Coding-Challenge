# Planning
// Still just topics, I will go deeper on them soon

## Security
The app will run on AWS servers. Different instances of the cloud are in different security layers, inside a private cloud.

The developer can access the private cloud through a proxy with a public IP. This is the only machine with administration privileges inside our network that is accessible through the outside. To access it through ssh you will need a ssh key.

Different developers and sys-admins have different roles in the system

The regular user accesses the app through a load balancer in a public subnet, that connects afterwards the necessary services inside the private subnets.

## Scalability

Processes are first class citizens. Different processes should run on different machines. Each component of of the system also can run in 2 or more machines, to assure redundacy. For example, There are two instances responsible for the core, two instances responsible for the database, two for the web server, and so on. The user endpoint will connect to the service via a load balancer (two) that contacts the necessary machines inside the system.

## Logging

Each machine has a cache with its own logs, that each process writes to stdout. From time to time, this file will be redirected to a central log server, that manages all logs. This connection should be made asynchronously, avoiding to slow down the service when there's to much traffic.

## Monitoring

We should monitor our app through the outside and through the inside. Emulate the user and send information to a monitoring machine, and test from inside the private network the connectivity of all machines. Each fail should trigger an event to handle automatically the fail. For example, if an instance stops, it can be terminated and a new one is started to assume it's position.

## Automation

Each deploy is handled with an Ansible playbook that performs the necessary steps to do it. Test the deploy, copy it to the machine inside the private network, reload services.
