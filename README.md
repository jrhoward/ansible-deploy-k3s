## Install k3s

Installs k3s https://k3s.io/ with ansible in a multinode set up

Review the inventory file inventory-example.ini

```

ansible-playbook -i inventory-example.ini install.yaml

```

## Limitations

- Tested on vanilla fedora29 servers with one master and multiple nodes.

- k3s does not support multiple masters yet as of v0.4.0

- Firewall is turned off
