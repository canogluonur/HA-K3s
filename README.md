# HA-K3s Installation

This document provides a step-by-step guide for load balancing Kubernetes Master using HAProxy. Below are the necessary steps for configuring HA-K3s with 3 machines (haproxy, master1, master2).

## Requirements

- 3 machines: `haproxy`, `master1`, `master2`
- Ubuntu operating system

## Installation Steps

Execute the following commands in order to complete the installation:

1. **HAProxy Installation** (on the machine where HAProxy is installed):

    ```bash
    sudo apt-get update && sudo apt-get install haproxy -y
    ```

2. **Master1 Installation**:

    ```bash
    curl -sfL https://get.k3s.io | K3S_TOKEN=SECRET sh -s - server --cluster-init --tls-san=<haproxyIP>
    ```

3. **Get Node Token** (on Master1):

    ```bash
    sudo cat /var/lib/rancher/k3s/server/node-token
    ```

4. **Master2 Installation**:

    ```bash
    curl -sfL https://get.k3s.io | K3S_TOKEN=<node-token> sh -s - server --server https://<master1IP>:6443 --tls-san=<haproxyIP>
    ```

5. **Get K3s Configuration File** (on Master1):

    ```bash
    cat /etc/rancher/k3s/k3s.yaml
    ```

6. **Install Kubectl on HAProxy Server**:

    ```bash
    # Kubectl installation steps (add your system-specific commands here)
    ```

7. **Configure Kubectl**:

    ```bash
    curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
    chmod +x kubectl
    sudo mv kubectl /usr/local/bin
    mkdir ~/.kube
    cd ~/.kube
    vi config
    ```

    Replace the `server` address in the `k3s.yaml` file with `haproxyIP`.

## Conclusion

After completing these steps, you should be able to access your Kubernetes cluster through HAProxy. If you encounter any issues, you can seek support from documentation or community forums.
