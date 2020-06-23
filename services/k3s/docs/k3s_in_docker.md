### A k3s Cluster in Docker

k3s is containerized and available in [Docker hub](https://hub.docker.com/r/rancher/k3s). k3s in Docker, known as [k3d](https://github.com/rancher/k3d), is also available, but requires to install k3d binaries on the host OS. To strictly decouple k3s from the host OS and yet run k3s in Docker, we need to modify configurations in the k3s Docker container.

__NOTE: This document is live and may contain false information as this is written from no Kubernetes expert__

