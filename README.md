# Overview: Ansible tagging and filters
[![License](https://img.shields.io/badge/license-MIT-brightgreen.svg)](LICENSE)

Make use of the `ec2_remote_facts` module rather than using the `ec2.py` [dynamic inventory](https://aws.amazon.com/blogs/apn/getting-started-with-ansible-and-dynamic-amazon-ec2-inventory-management/). So far it feels cleaner to use than the full force `ec2.py` and `ec2.ini`. There are some idiosyncrasies when using comma separated tags on AWS.

An example of a comma separated tag list would be as follows.

        ROLES=app,web,db

An example of a non-comma separated role tag list

        ROLES=lb

- - - -
# Usage

Match a set of tags exactly and get the returned instance(s)

        ansible-playbook playbook.yml -e cli_env=prod -e cli_roles=app,web,db

Glob all boxes with role of `web` and get the returned instance(s)

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
