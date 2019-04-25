## Install k3s

Installs k3s https://k3s.io/ with ansible in a multinode set up

## Usage

Review and update the inventory file `inventory-example.ini`

Then run
```

ansible-playbook -i inventory-example.ini install.yaml

```

## Notes/Limitations

- Tested on vanilla fedora29 servers with one master and multiple nodes.

- k3s does not support multiple masters yet as of v0.4.0

- Firewall is turned off
