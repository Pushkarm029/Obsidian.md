- Kyverno (Greek for “govern”) is a policy engine designed specifically for Kubernetes.
- "policies" refer to a set of rules and configurations that define how the cluster and its resources should behave or be managed.
- validate, mutate, generate, or cleanup (remove) any resource
- verify container images for software supply chain security
- inspect image metadata
- match resources using label selectors and wildcards
- validate and mutate using overlays (like Kustomize!)
- synchronize configurations across Namespaces
- block non-conformant resources using admission controls, or report policy violations
- self-service reports (no proprietary audit log!)
- self-service policy exceptions
- test policies and validate resources using the Kyverno CLI, in your CI/CD pipeline, before applying to your cluster
- manage policies as code using familiar tools like `git` and `kustomize`

### How Kyverno works
- Kyverno runs as a [dynamic admission controller](https://kubernetes.io/docs/reference/access-authn-authz/extensible-admission-controllers/) in a Kubernetes cluster. Kyverno receives validating and mutating admission webhook HTTP callbacks from the Kubernetes API server and applies matching policies to return results that enforce admission policies or reject requests.
- [K8s]Admission control in Kubernetes is a crucial step in the resource creation and update process. It involves validating and mutating resource requests before they are persisted to the cluster's etcd data store. Dynamic Admission Control lets you add, remove, or modify these admission control mechanisms based on your specific requirements.
- match resources using the resource kind, name, label selectors, and much more.
- Policy enforcement is captured using Kubernetes events. For requests that are either allowed or existed prior to introduction of a Kyverno policy, Kyverno creates Policy Reports in the cluster which contain a running list of resources matched by a policy, their status, and more.![[Pasted image 20231017145120.png]]
- The **Webhook** is the server which handles incoming AdmissionReview requests from the Kubernetes API server and sends them to the **Engine** for processing. It is dynamically configured by the **Webhook Controller** which watches the installed policies and modifies the webhooks to request only the resources matched by those policies.
- The **Cert Renewer** is responsible for watching and renewing the certificates, stored as Kubernetes Secrets, needed by the webhook. 
- The **Background Controller** handles all generate and mutate-existing policies by reconciling UpdateRequests, an intermediary resource.  
- the **Report Controllers** handle creation and reconciliation of Policy Reports from their intermediary resources, Admission Reports and Background Scan Reports.

### Policies and Rules
- A Kyverno policy is a collection of rules. Each rule consists of a [`match`](https://kyverno.io/docs/writing-policies/match-exclude/) declaration, an optional [`exclude`](https://kyverno.io/docs/writing-policies/match-exclude/) declaration, and one of a [`validate`](https://kyverno.io/docs/writing-policies/validate/), [`mutate`](https://kyverno.io/docs/writing-policies/mutate/), [`generate`](https://kyverno.io/docs/writing-policies/generate), or [`verifyImages`](https://kyverno.io/docs/writing-policies/verify-images) declaration. Each rule can contain only a single `validate`, `mutate`, `generate`, or `verifyImages` child declaration.
- ![[Pasted image 20231018002919.png]]
- Policies can be defined as cluster-wide resources (using the kind `ClusterPolicy`) or namespaced resources (using the kind `Policy`.) As expected, namespaced policies will only apply to resources within the namespace in which they are defined while cluster-wide policies are applied to matching resources across all namespaces. Otherwise, there is no difference between the two types.

### Writing Policies

- [Policy Settings](https://kyverno.io/docs/writing-policies/policy-settings/)
- [Validate Rules](https://kyverno.io/docs/writing-policies/validate/) -> Anchors
- 