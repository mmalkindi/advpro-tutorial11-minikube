# Reflection: Hello Minikube

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
