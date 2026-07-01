# k8s-lab

Personal Kubernetes learning lab — single-node cluster set up manually with kubeadm + CRI-O.

## Contents

- `k8s-setup.sh` — bootstrap script for control-plane/worker nodes (kubeadm, CRI-O, Flannel CNI)
- `k8s-manifest/ns.yaml` — dev/prod namespaces
- `k8s-manifest/deployment.yaml` — nginx Deployment in the dev namespace
- `k8s-manifest/pods.yaml` — early bare-pod experiments (superseded by deployment.yaml)

## Cluster setup

\`\`\`bash
sudo NODE_ROLE=master ./k8s-setup.sh   # first node
sudo NODE_ROLE=node ./k8s-setup.sh     # additional worker nodes
\`\`\`
