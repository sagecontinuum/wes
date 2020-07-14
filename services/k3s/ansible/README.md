### k3d Set-ups with Ansible

The Ansible scripts help setting up a k3d -- one k3s server and one k3s worker -- on the remote machine. Before running, please make sure to correctly set,

- ssh information in the nx.nodes
- the username of the remote machine in the scripts.


To install k3d,
```
# installing k3d requires sudo access
# use -K option and type the sudo password of the remote machine
$ ansible-playbook -K -i nx.nodes install_waggle_k3s.yaml
BECOME password: 
```

To launch a k3s cluster,
```
$ ansible-playbook -i nx.nodes setup_waggle_k3s.yaml
```
