# Module 11 - Deployment on Kubernetes

## Reflection: Hello Minikube

> Q: Compare the application logs before and after you exposed it as a Service.
> Try to open the app several times while the proxy into the Service is running.
> What do you see in the logs? Does the number of logs increase each time you open the app?

A: `hello-node` logs before and after the app is exposed as a Service:

![logs before after service](/img/before_after_service.jpg)

After being exposed as a service, we can now access the app from a local URL.
Every interaction we do with this service will be logged, such as opening the app (`GET` request to `/`).
We can see it happen by comparing the second and the third image above.

> Q: Notice that there are two versions of `kubectl get` invocation during this tutorial section.
> The first does not have any option, while the latter has `-n` option with value set to `kube-system`.
> What is the purpose of the `-n` option and why did the output not list the pods/services that you explicitly created?

A: From `kubectl options`, we can see that the `-n` option is short for `--namespace`

![Kubectl options](/img/kubectl_options.png)

We can infer that the `-n` option is used to specify which `namespace` to run the command on.
In Kubernetes, namespaces provide a way to organize groups of resouces and isolate them within a single cluster.

When running `kubectl get` with `-n kube-system` option, the output did not list the pods/services that we have created during the tutorial
because those were created under the `default` namespace. The command will run on the `default` namespace unless it's specified (like in the latter invocation).

## Reflection: Rolling Update & Kubernetes Manifest File

> Q: What is the difference between Rolling Update and Recreate deployment strategy?

A:

1. Rolling Update: New deployment is applied to new pods while also killing existing pods in a step-by-step manner (hence the name Rolling update).
This allows the app to have no downtime because there will always available pods to use even if it's not up to date with the latest revision.
2. Recreate: All existing pods are killed before new ones are created. This causes the app to have some downtime (when zero pods are fully running yet),
but guarantees that every access to the app uses the latest revision.

> Q: Try deploying the Spring Petclinic REST using Recreate deployment strategy and document your attempt

A:

Changing the manifest file to use `.spec.strategy.type==Recreate` instead of `.spec.strategy.type==RollingUpdate`

![Change manifest file](/img/change_manifest_file.png)

Delete and Start Minikube to ensure a fresh slate

![Delete start minikube](/img/delete_start_minikube.png)

Apply the edited deployment and service manifest files

![Apply edited manifest file](/img/apply_manifest.png)

Set image to use version 3.0.2 and monitor the deployment rollout

![Monitor rollout logs](/img/rollout_logs.png)
![Monitor pods status](/img/get_pods.png)

> Q: Prepare different manifest files for executing Recreate deployment strategy.

![Recreate manifest files](/img/recreate_manifest_files.png)

> Q: What do you think are the benefits of using Kubernetes manifest files?
> Recall your experience in deploying the app manually and compare it to your experience when deploying the same app
> by applying the manifest files (i.e., invoking `kubectl apply -f` command) to the cluster.

A: Using Kubernetes manifest files makes it easy to implement a specific configuration with one single command instead of redoing all of the deployment commands.
This is especially important in a deployment environment where we will want to automate the process via GitHub Actions or similar CI/CD pipelines.
The manifest file will also be able to participate in the project's Version Control System (e.g: git) which will help with documentation and tracing changes.
