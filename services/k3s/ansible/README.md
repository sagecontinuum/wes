### k3d Set-ups with Ansible

The Ansible scripts help setting up a k3d -- one k3s server and one k3s worker -- on a remote machine.

Before running, change the followings,

In nx.nodes
- ssh information to target node

In install_waggle_k3s.yaml
- `user` to username of target node

In setup_waggle_k3s.yaml
- `user` to username of target node
- `ecr_registry` to address of ECR registry


To install k3d,
```
# installing k3d requires sudo access
# use -K option and type sudo password of remote machine
$ ansible-playbook -K -i nx.nodes install_waggle_k3s.yaml
BECOME password: 
```

To launch a k3s cluster,
```
$ ansible-playbook -i nx.nodes setup_waggle_k3s.yaml
```

#### Notes on ECR Registry

The k3s cluster will be able to talk to the ECR registry we set. ECR has to support TLS and any non-TLS registry setup CANNOT be used in production, but only used in development.

To set up a Docker registry, refer [this](https://docs.docker.com/registry/deploying/). To enable TLS support, see [this](https://docs.docker.com/registry/configuration/#tls).

Using k3d registry is not currently supported in v3 as of July 2020 (See [more](https://k3d.io/usage/guides/registries/#using-the-k3d-registry)).
