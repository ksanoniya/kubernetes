🔥 Step-by-Step: Install Metrics Server on Kind Cluster
🧩 Step 1: Verify Your Cluster

Check if Kind cluster is running:

kind get clusters

If not running:

kind create cluster --name devops-lab

Verify nodes:

kubectl get nodes

⚠️ Step 2: Check Metrics Server (not installed by default)

Run:

kubectl top nodes

⚙️ Step 3: Install Metrics Server (Official Manifest)

Run this command to apply the latest metrics-server YAML:
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml


⚠️ Step 4: Patch Deployment for Kind (Important!)

By default, Kind runs without proper TLS certificates for node communication,
so metrics-server will fail with:

unable to fully collect metrics: no metrics known for node


We fix that by adding args to the metrics-server container.

Run this:

kubectl -n kube-system edit deployment metrics-server

and past 
  args:
  - --kubelet-insecure-tls
  - --kubelet-preferred-address-types=InternalIP,Hostname,InternalDNS,ExternalDNS,ExternalIP4

⚙️ Step 5: Restart the Deployment
kubectl -n kube-system rollout restart deployment metrics-server

✅ Step 6: Verify Metrics Server

After 20–30 seconds, try:

kubectl top nodes
kubectl top pods -A

