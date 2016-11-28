# Overview: Ansible tagging and filters
[![License](https://img.shields.io/badge/license-MIT-brightgreen.svg)](LICENSE)

To me, it's really damn difficult to figure out how to bootstrap a cluster of servers with Ansible, such as [Consul](https://www.consul.io/). Consul needs at least 3 server nodes to form an HA production cluster. Sure, you can run it just fine with 1 node, but then if it dies you're SOL. Sure, "Just put in the IPs of the boxes you start up.". Well Mr/Ms Smarty Pants, I don't know what those IPs will be when I spin up the servers in AWS so I use tags. I've chosen a possibly stupid way of tagging by using comma separated values, but it works for me. We don't need a consul server joining itself, so we have to remove the current node from the aggregated list of consul servers. This will look as follows.

        Server 1
            Server 2
            Server 3

        Server 2
            Server 1
            Server 3

        Server 3
            Server 1
            Server 2

To get our nodes, we make use of the `ec2_remote_facts` module rather than using the `ec2.py` [dynamic inventory](https://aws.amazon.com/blogs/apn/getting-started-with-ansible-and-dynamic-amazon-ec2-inventory-management/). So far it feels cleaner to use than the full force `ec2.py` and `ec2.ini`. There are some idiosyncrasies when using comma separated tags on AWS.

An example of a comma separated tag list would be as follows.

        ROLES=app,web,db

An example of a non-comma separated role tag list

        ROLES=lb

- - - -
# Usage

Match a set of tags exactly and get the returned instance(s)

        ansible-playbook playbook.yml -e cli_env=prod -e cli_roles=app,web,db

Glob all boxes with role of `web` somewhere in the list and get the returned instance(s). This is where this method gets weird and you can potentially match too many boxes. Be careful. Always run with --list-hosts first.

        ansible-playbook playbook.yml -e cli_env=prod -e cli_roles=*web* --list-hosts
        ansible-playbook playbook.yml -e cli_env=prod -e cli_roles=*web*

Glob all boxes with role of `jenkins*` and get the returned instance(s)

        ansible-playbook playbook.yml -e cli_env=prod -e cli_role=jenkins*

I hope you can see where this is going.

- - - -
# References

1. [Master Ansible](https://books.google.com/books?id=bvSoCwAAQBAJ&pg=PA67&lpg=PA67&dq=ansible+nested+filters&source=bl&ots=Nfj9uX6i84&sig=eLmypWMp8ZVLbKkZVFJbZtdDi8w&hl=en&sa=X&ved=0ahUKEwjuhdKmpMrQAhVs5IMKHdhCDa44FBDoAQgaMAA#v=onepage&q=ansible%20nested%20filters&f=false)
1. [Ansible documentation on filters](http://docs.ansible.com/ansible/playbooks_filters.html)
1. [Imil's blog](https://imil.net/blog/2016/08/05/Ansible_and_AWS_ASG/)
1. [Bonovoxly's blog](https://bonovoxly.github.io/2016-02-11-ansible-stuffs-ec2_remote_facts_instead_of_ec2_py)

- - - -
# Theme Music

[The Specials - Ghost Town](https://www.youtube.com/watch?v=jqZ8428GSrI)

- - - -
# Author and License Information

MIT

[Phil Porada](http://philporada.com)
