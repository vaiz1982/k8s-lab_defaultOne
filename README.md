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









### What you actually built today

**1. A real Kubernetes cluster, from scratch**  
You took a raw Ubuntu server (EC2) and turned it into a working Kubernetes cluster using `kubeadm`. This involved installing:

- **CRI-O** — the container runtime (the thing that actually runs containers, like Docker does, but built for Kubernetes)
- **kubelet, kubeadm, kubectl** — the core Kubernetes tools
- **Flannel** — the networking plugin (CNI) that lets pods talk to each other

This is the "hard mode" way to get a cluster running — most people use managed services (EKS, GKE) that hide all of this. You did it manually, which is why you hit real bumps (Flannel image bug, taint blocking scheduling, kubeconfig permission issue) — and fixed every single one.

**2. Namespaces**  
You created `dev` and `prod` — logical dividers inside one cluster, so you can run the same app twice without name collisions.

**3. Pods**  
You deployed nginx as a raw Pod into both namespaces. A **Pod** is the smallest unit in Kubernetes — one or more containers sharing a network/storage. You debugged real YAML mistakes along the way (indentation, typos, wrong units) — normal, everyone does this.

**4. Labels**  
You tagged your pods with `app: app-1` so other things (like Services) could *find* them by label instead of by name.

**5. Services**  
You created a `Service` (`app-1-svc`) that gives your pod(s) a stable address. You proved it worked by curling it from inside the cluster and getting the real nginx page back — this showed the whole chain working: pod → label → service selector → routing → DNS.

**6. ArgoCD**  
You installed ArgoCD, a GitOps tool (deploys automatically from a Git repo instead of manual `kubectl apply`). It's installed and running, though you haven't actually connected it to a Git repo yet — that's a future step.

**7. Concepts, via your own sketches**  
We covered the mental model of:

- Deployment → ReplicaSet → Pod → Containers (how self-healing/scaling actually works under the hood)
- Deployment vs StatefulSet vs DaemonSet (three ways to run different kinds of workloads)
- The open question of exposing a Service *outside* the cluster (NodePort/LoadBalancer/Ingress) — this is the one piece we haven't done yet

### Where you are right now

You have a **live, working single-node Kubernetes cluster** with:

- Namespaces (`dev`, `prod`, `argocd`, plus system ones)
- A running nginx pod in each of `dev` and `prod`, reachable internally via Service
- ArgoCD installed and running

### What's genuinely still missing / next

- Bare Pods aren't self-healing yet — you'd want a **Deployment** wrapping them
- Nothing is reachable from **outside** the cluster yet (that `?` from your last diagram)
- ArgoCD isn't connected to any Git repo yet — it's installed but idle

Given you said you're brand new to this — you've actually covered a huge amount of ground for one session. Want me to slow down and just walk through **one concept at a time** from here (starting with Deployments, since that's the natural next step), rather than jumping around?
