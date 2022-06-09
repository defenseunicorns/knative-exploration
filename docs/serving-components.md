https://knative.dev/docs/serving/
https://knative.dev/docs/serving/knative-kubernetes-services/#before-you-begin

Deployments:
activator (https://knative.dev/docs/serving/knative-kubernetes-services/#service-activator)
The activator is responsible for receiving & buffering requests for inactive revisions and reporting metrics to the autoscaler. It also retries requests to a revision after the autoscaler scales the revision based on the reported metrics.

autoscaler(https://knative.dev/docs/serving/knative-kubernetes-services/#service-autoscaler)
The autoscaler receives request metrics and adjusts the number of pods required to handle the load of traffic.

controller(https://knative.dev/docs/serving/knative-kubernetes-services/#service-controller)
The controller service reconciles all the public Knative objects and autoscaling CRDs. When a user applies a Knative service to the Kubernetes API, this creates the configuration and route. It will convert the configuration into revisions and the revisions into deployments and Knative Pod Autoscalers (KPAs).

domain-mapping (https://knative.dev/docs/serving/services/custom-domains/)
Each Knative Service is automatically assigned a default domain name when it is created. However, you can map any custom domain name that you own to a Knative Service, by using domain mapping. You can create a DomainMapping object to map a single, non-wildcard domain to a specific Knative Service.

webhook(https://knative.dev/docs/serving/knative-kubernetes-services/#service-webhook)
The webhook intercepts all Kubernetes API calls as well as all CRD insertions and updates. It sets default values, rejects inconsitent and invalid objects, and validates and mutates Kubernetes API calls.

net-istio-controller(https://knative.dev/docs/serving/knative-kubernetes-services/#deployment-net-istio-controller)
The net-istio-controller deployment reconciles a cluster's ingress into an Istio virtual service.

net-certmanager-controller(https://knative.dev/docs/serving/knative-kubernetes-services/#deployment-net-certmanager-controller)
The certmanager reconciles cluster ingresses into cert manager objects.

