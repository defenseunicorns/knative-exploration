> [Knative AutoScaling docs](https://knative.dev/docs/serving/autoscaling/)


## AutoScaling
Knative uses a Knative Pod Autoscaler (KPA) by default, but can be configured to use a k8s Horizontal Pod Autoscaler (HPA) instead.
  - Using a HPA instead will limit some of the cool autoscaling features of Knative, like scale-to-zero

- The autoscaler defaults to measuring concurrent requests as the metric to scale by (but can also use requests-per-second as a metric).
- The autoscaler defaults to scaling from the range of 0 to infinity.
- 

## Default Values (and a brief explanation)
By default ([source](https://knative.dev/docs/serving/autoscaling/autoscaler-types/#global-settings)), the knative autoscaler configmap has the following values:

NOTE: These are all defaults that get applied cluster wide. Each application can add annotations to its Service to override these defaults for whatever specific use case they have.
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
 name: config-autoscaler
 namespace: knative-serving
data:
 container-concurrency-target-default: "100"          # Number of requests each pod should be able to handle.
                                                      # NOTE: Pods will actually handle this value * container-concurrency-target-percentage before scaling up.


 container-concurrency-target-percentage: "0.7"       # Based on the maximum number of requests a container can handle, this is the target percentage
                                                      # of requests all containers will try to maintain in a stable state.
                                                      # NOTE: This field accept fractional or whole numbers


 enable-scale-to-zero: "true"                         # Toggles the ability to scale pods down to zero if no requests are received for a period of time.



 max-scale-up-rate: "1000"                            # Limits the rate at which the autoscaler will increase pod counts.
                                                      # I.e. if N is current pods; with a value of 2 the number of pods can at most go N to 2N but at least N to N+1


 max-scale-down-rate: "2"                             # Limits the rate at which the autoscaler will decrease pod counts.
                                                      # I.e. if N is current pods; with a value of 2 the number of pods can go N to N/2 but at least N to N-1

 panic-window-percentage: "10"                        # Autoscaler triggers a panic if the observed concurrency is panic-window-percentage percent away from the
                                                      # container-concurrency-target-percentage value.
                                                      # NOTE: This field does not accept fractional numbers


 panic-threshold-percentage: "200"                    # Percentage of requests greater than container-concurrency-target-percentage that will trigger a panic.


 scale-to-zero-grace-period: "30s"                    # The time an inactive pod is left running before it is scaled to zero.


 scale-to-zero-pod-retention-period: "0s"             # The minimum amount of time the last pod will remain after the Autoscaler has decided to scale-to-zero.


 stable-window: "60s"                                 # The window of time to average requests across to get the concurrent request metrics


 target-burst-capacity: "200"                         # Specifies the size of burst in concurrent requests that the engineer expects the system to receive occasionally


 requests-per-second-target-default: "200"            # Target to use if the metric is requests-per-second instead of concurrent-requests.


 min-scale: "0"                                       # Minimum number of pods to scale down to
                                                      # NOTE: Can be overridden by the "autoscaling.knative.dev/minScale" annotation.

 max-scale: "0"                                       # Maximum number of pods to scale up to
                                                      # NOTE: Can be overridden by the "autoscaling.knative.dev/minScale" annotation.

 scale-down-delay: "0s"                               # Amount of time that must pas at a reduced concurrency before a scale down decision is made.
```