# Run OPS as admission control in your cluster

> OPA Gatekeeper is a policy controller for Kubernetes. This is basic step on implementing an simple organization policy enforcement rule for Kubernetes namespace and image run.

1. Installing OPA Gatekeeper.

   create a new directory, pull the code, and then apply the yaml file.
   ```bash
   mkdir gk
   cd gk
   git clone https://github.com/open-policy-agent/gatekeeper.git
   kubectl apply -f gatekeeper/deploy/gatekeeper.yaml
   cd ..
   ```

   Check that the pod is running
   ```bash
   kubectl get pods -n gatekeeper-system
   NAME                                             READY   STATUS    RESTARTS   AGE
   gatekeeper-audit-7d97d6b7f6-zp7f9                1/1     Running   0          6m27s
   gatekeeper-controller-manager-69b557bbcf-4sscx   1/1     Running   0          6m27s
   gatekeeper-controller-manager-69b557bbcf-6dmhh   1/1     Running   0          6m27s
   gatekeeper-controller-manager-69b557bbcf-tl5cv   1/1     Running   0          6m27s
   ```

2. Create a Constraint template for label requirement

   OPA uses templates in conjunction with the actual constraints. Add a new template and constraint to only allow new namespaces which have a particular label. 
   ```bash
   kubectl create -f yaml/k8srequiredlabels.yaml
   ```

3. Create a Constraint for ns label requirement
   ```bash
   kubectl create -f yaml/ns-require-label.yaml
   ```

4. Test that the new constraint prevents new namespace creation if the namespace does not have the proper label.
   ```bash
   kubectl create ns nolabel
   ```

   You should get an output similar to
   ```bash
   Error from server ([ns-require-label] You must provide labels: {“gk-ns”}): admission webhook “validation.gatekeeper.sh” denied the request: [ns-require-label] You must provide labels: {“gk-ns”}
   ```

   Create a new namespace, this time with the required label of gk-ns.
   ```bash
   kubectl apply -f - <<EOF
   apiVersion: v1
   kind: Namespace
   metadata:
     labels:
       gk-ns: "yes"
     name: haslabel
   spec:
   EOF
   ```

5. Create another template and constraint to limit the source image repository
   ```bash
   kubectl create -f yaml/k8srequiredregistry.yaml
   ```   

   ```bash
   kubectl create -f yaml/only-quay-images.yaml
   ```   

   Create two new deployments, one of which uses a Quay.io image.  
   ```bash
   kubectl create deployment dockerhub --image=nginx
   kubectl create deployment quay.io --image=quay.io/prometheus/prometheus
   ```   

   Confirm the quay pod is running & dockerhub pod is not running. 
   ```bash
   kubectl get pods
   NAME                      READY   STATUS    RESTARTS   AGE
   quay.io-fbc5b687d-fd28s   1/1     Running   0          5m48s
   ``` 

   You will find the error message in rs 
   ```bash
   kubectl describe rs dockerhub-fccf5c68f

   Events:
     Type     Reason        Age                     From                   Message
     ----     ------        ----                    ----                   -------
     Warning  FailedCreate  3m17s (x17 over 8m45s)  replicaset-controller  Error creating: admission webhook "validation.gatekeeper.sh" denied the request: [only-quay-images] Forbidden registry: nginx
   ```
   
      