# Knative Exploration
(Definitely not pronounced "kah-native".)

The intention of this repo is to give a starting point for exploring development with [Knative](https://knative.dev/docs/) and provide a location to compile docs and questions.

Knative is a kubernetes operator that enables "serverless" deployments and event-driven apps.

Useful links:
* [Docs](https://knative.dev/docs/)
* [Quickstart guide](https://knative.dev/docs/getting-started/quickstart-install/)
* [Github](https://github.com/knative/docs)
* [Serving code samples](https://knative.dev/docs/samples/serving/)

## Prerequisites
Setting up the local development environment requires the following to be installed and configured on your workstation:
* [k3d](https://k3d.io/v5.4.3/#installation)
* [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl-macos/)
* [kustomize CLI](https://kubectl.docs.kubernetes.io/installation/kustomize/)
* [helm CLI](https://helm.sh/docs/intro/install/)
* [knative CLI](https://knative.dev/docs/getting-started/quickstart-install/)
* [Local clone of the BB repository](https://repo1.dso.mil/platform-one/big-bang/bigbang)
* Kubernetes version 1.22.0+

## Local Development Setup (k3d)
Getting started...
1. Deploy K3d
   * `k3d cluster create -c k3d-config.yaml`
1. Check that kubectl is connected to localhost
   * `kubectl cluster-info`
1. Deploy Bigbang
   1. Export the bigbang cloned repo location as `$BIGBANG_REPO`
      * ex: `export BIGBANG_REPO=/home/ablanchard/Source/platform-one/BigBang/bigbang`
   2. Install flux:
      * `$BIGBANG_REPO/scripts/install_flux.sh -u <user> -p <pass>`
      > user: platform1 username

      > pass: [harbor](https://registry1.dso.mil/harbor/projects) token
   3. Create a `~/.bigbang/local-credentials.yaml` file (using the same username and token) for passing to the bigbang chart:
   ```
    registryCredentials:
    - registry: registry1.dso.mil
      username: <user>
      password: <pass>
   ```
   1. Create the `bigbang` namespace:
      *  `kubectl create ns bigbang`
   2. Deploy the helm chart, using the credentials file and the minimal values file (adjust as needed):
      *  `helm upgrade -i bigbang $BIGBANG_REPO/chart -n bigbang -f ~/.bigbang/local-credentials.yaml -f $BIGBANG_REPO/chart/ingress-certs.yaml -f ./minimal-bb.yaml`
   3. Wait for the deployments...
      * `watch kubectl get hr -A`
2. Install knative:
   1. [knative Operator](https://knative.dev/docs/install/operator/knative-with-operators/):
      * `kubectl apply -f https://github.com/knative/operator/releases/download/knative-v1.5.0/operator.yaml`
   1. Server CR (configured to use istio):
      * `kubectl apply -f ./custom-resources/serving.yaml`
   1. Verify the Knative Serving deployment:
      * `kubectl get deployments -n knative-serving`
3. Deploy podinfo knative service (quickstart instructions... but with podinfo):
   * `kustomize build apps/podinfo | k apply -f -`
   * `kubectl get all -n podinfo`
4. ...
5. Profit!

## Local Development setup (Zarf):
???

## Additional Notes
* Upgrade the bigbang deployment values by adjusting any of the passed value files and re-running the helm upgrade command in 3.2.1 above
* Document all the things!
