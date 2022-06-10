https://knative.dev/docs/serving/
https://knative.dev/docs/serving/knative-kubernetes-services/#before-you-begin

Deployments:
activator (https://knative.dev/docs/serving/knative-kubernetes-services/#service-activator)
The activator is responsible for receiving & buffering requests for inactive revisions and reporting metrics to the autoscaler. It also retries requests to a revision after the autoscaler scales the revision based on the reported metrics.
images gcr.io/knative-releases/knative.dev/serving/cmd/activator@sha256:0fdd0f9e6a7698cf3ecec9494c46509109345476a99f21f4cc5117dff5c9dda4

autoscaler (https://knative.dev/docs/serving/knative-kubernetes-services/#service-autoscaler)
The autoscaler receives request metrics and adjusts the number of pods required to handle the load of traffic.
images gcr.io/knative-releases/knative.dev/serving/cmd/autoscaler-hpa@sha256:4263db1da5cb7955faad3c290a64b33f6a505e9894e7addb44f67bff4574e229
       gcr.io/knative-releases/knative.dev/serving/cmd/autoscaler@sha256:f36d1a5e3b6bc385398224acf1ee14b3b93f4fa8c6ea8c9126e606a9e67092a0

controller (https://knative.dev/docs/serving/knative-kubernetes-services/#service-controller)
The controller service reconciles all the public Knative objects and autoscaling CRDs. When a user applies a Knative service to the Kubernetes API, this creates the configuration and route. It will convert the configuration into revisions and the revisions into deployments and Knative Pod Autoscalers (KPAs).
images gcr.io/knative-releases/knative.dev/serving/cmd/controller@sha256:18ea69e9afc42a9a79d6959499fa5adfa880f3a0f0ffde55a0ea6814bd1bb8b6

domain-mapping (https://knative.dev/docs/serving/services/custom-domains/)
Each Knative Service is automatically assigned a default domain name when it is created. However, you can map any custom domain name that you own to a Knative Service, by using domain mapping. You can create a DomainMapping object to map a single, non-wildcard domain to a specific Knative Service.
images gcr.io/knative-releases/knative.dev/serving/cmd/domain-mapping-webhook@sha256:5d537e5c85a07173c495fff2984208c267a1a545910b6abb881c1f0016ff2038
       gcr.io/knative-releases/knative.dev/serving/cmd/domain-mapping@sha256:3189aa2179c2d199cd31328813ebae4507bc12d94bf21b9b0bc231d6cf09f83c

webhook (https://knative.dev/docs/serving/knative-kubernetes-services/#service-webhook)
The webhook intercepts all Kubernetes API calls as well as all CRD insertions and updates. It sets default values, rejects inconsitent and invalid objects, and validates and mutates Kubernetes API calls.
images gcr.io/knative-releases/knative.dev/serving/cmd/webhook@sha256:e66b4851894e92898b0b3dc6d6fa74ac766ed1d1f85f4b55ac8076f8ae14301a
       gcr.io/knative-releases/knative.dev/operator/cmd/webhook@sha256:bcb52df48b96280209ae16eab953fc42e4cccbb00db09a6209101345b4e9fb63

net-istio-controller (https://knative.dev/docs/serving/knative-kubernetes-services/#deployment-net-istio-controller)
The net-istio-controller deployment reconciles a cluster's ingress into an Istio virtual service.

net-certmanager-controller (https://knative.dev/docs/serving/knative-kubernetes-services/#deployment-net-certmanager-controller)
The certmanager reconciles cluster ingresses into cert manager objects.

