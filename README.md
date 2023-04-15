# python-http-daemon
### Highlights/comments

1. The Ansible was chosen as the utility for configuring the target system.
2. Template of the http daemon located [here](python-daemon-role/templates/python-daemon-http.py.j2).
3. The service ```python-daemon-http.service``` that starts the daemon is running from the ```python-daemon``` user.
4. Since port 80 is reserved for the root user the the daemon is running on port 8080 and traffic is redirecting from port 80 to port 8080.
5. The respond "329d4feb-c5c0-4de5-b10c-701b44fbec4f" to GET request at the /test URI(http://192.241.210.61/test) should be defined in the ```respond_to_test_uri``` variable. The playbook will fail if this variable will be not defined(it is already defined inside the python-daemon.yml file).

### How to run the playbook

1. The host ```192.241.210.61``` is already added to the [inventory](inventory) file.
2. Just apply your ssh key for the 192.241.210.61 server with the command ```eval "$(ssh-agent -s)" && ssh-add [path_to_the_private_key]``` and run the playbook with the command **```ansible-playbook python-daemon.yml```**

2.1 As an alternative you can add your public key to file ```~/.ssh/authorized_keys``` at the 192.241.210.61 server.

##### python-daemon.yml:
```yaml
---
- name: Deploy Python HTTP Daemon
  hosts: python-daemon
  become: true
  roles:
    - role: python-daemon-role
      vars:
        respond_to_test_uri: "329d4feb-c5c0-4de5-b10c-701b44fbec4f"
```
