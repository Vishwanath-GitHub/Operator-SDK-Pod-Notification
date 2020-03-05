# Operator-SDK-Pod-Notification
This is a project for getting notifications from logs of Kubernetes Operator that when a new pod is deployed.

## Operator-SDK
Operator SDK is a tool provided by Operator Framework that allows us to deploy Kubernetes Operators. These operators are used to deploy workloads via Kubernetes API i.e. it helps in deploying workloads by using services of Kubernetes. Read more from the actual link of Operator-SDK.

This project has a 'podset-operator' that contains in own file structure. Now, the main aim of this project is to get a notification in form of text in the logs of the operator pod when any new pod is added to the cluster. This notification is hardcoded in the [podset_controller.go](https://github.com/Vishwanath-GitHub/Operator-SDK-Pod-Notification/blob/master/pkg/controller/podset/podset_controller.go) file in pkg/controller/podset/. It displays a custom message, name of the pod and details of the pod including its name, namespace and labels.

Please read the official documentation of Operator-SDK to understand its file structure with Go files, how to build the operator with step-by-step process and deploy it.

Changes made in the Operator:
* In [podset_types.go](https://github.com/Vishwanath-GitHub/Operator-SDK-Pod-Notification/blob/master/pkg/apis/app/v1alpha1/podset_types.go) file, the contents of the struct{} is changed so that it we can get Pod Names. This file is under pkg/apis/app/v1alpha1/.
* In [operator.yaml](https://github.com/Vishwanath-GitHub/Operator-SDK-Pod-Notification/blob/master/deploy/operator.yaml), the image name is changed to the [custom image](https://github.com/Vishwanath-GitHub/Operator-SDK-Pod-Notification/blob/6886453c33ebcf45cf52f59b456858f7b6794b73/deploy/operator.yaml#L19) we have built and pushed to "quay.io" via Docker.
* In [podset_controller.go](https://github.com/Vishwanath-GitHub/Operator-SDK-Pod-Notification/blob/master/pkg/controller/podset/podset_controller.go) file, a new log variable is made named as [`globalLog`](https://github.com/Vishwanath-GitHub/Operator-SDK-Pod-Notification/blob/6886453c33ebcf45cf52f59b456858f7b6794b73/pkg/controller/podset/podset_controller.go#L22). This variable is then used where the pod actually gets created. So, this notification in logs of operator is written in [`Reconcile()`](https://github.com/Vishwanath-GitHub/Operator-SDK-Pod-Notification/blob/6886453c33ebcf45cf52f59b456858f7b6794b73/pkg/controller/podset/podset_controller.go#L84) function of this Go file. While creation of the pod, the custom text, name of pod and details of it are also shown in the logs of the operator pod along with other in-built logs. Use `kubectl logs` with the operator pod after deploying operator.yaml. 

IMP: `&coreV1.Pod{}` contains all the basic details of the pod that can be consumed by other functions.
IMP: The log variable `globalLog` takes compulsorily 3 arguments i.e. the custom text string, a key and a value. Not mentioning any of these arguments will result in this error: "odd number of arguments passed as key-value pairs for logging"

Actual Operator SDK Practical Documentation: https://docs.okd.io/latest/operators/osdk-getting-started.html

Operator SDK Installation Links: (step-by-step)
1. https://www.howtoforge.com/how-to-install-go-programming-language-on-linux-ubuntu-debian-centos/ (install golang from here)
2. https://tecadmin.net/install-go-on-ubuntu/ (install GOPATHs from here)
3. https://www.linode.com/docs/development/go/install-go-on-ubuntu/ (install GOPATHs from here)
4. https://golang.github.io/dep/docs/installation.html
5. https://www.atlassian.com/git/tutorials/install-git
6. https://github.com/operator-framework/operator-lifecycle-manager/releases
7. curl -s "https://raw.githubusercontent.com/\
kubernetes-sigs/kustomize/master/hack/install_kustomize.sh"  | bash

Kubenetes Operator links:
1. https://medium.com/faun/writing-your-first-kubernetes-operator-8f3df4453234
2. https://www.katacoda.com/openshift/courses/operatorframework/go-operator-podset
3. https://docs.okd.io/latest/operators/osdk-getting-started.html
4. https://medium.com/@shubhomoybiswas/writing-kubernetes-operator-using-operator-sdk-c2e7f845163a
5. https://github.com/operator-framework/operator-sdk/blob/master/doc/user/logging.md
6. https://github.com/operator-framework/operator-sdk/issues/1758
7. https://www.youtube.com/watch?v=Ko_WfXRAypY

Operator SDK Logging: https://github.com/operator-framework/operator-sdk/blob/master/doc/user/logging.md
