## MODULE 11
### REFLECTION 1
1.  Compare the application logs before and after you exposed it as a Service. Try to open the app several times while the proxy into the Service is running.
What do you see in the logs? Does the number of logs increase each time you open the app?
![alt text](logs.png)

Before exposing the application as a service, the logs only showed startup messages indicating that the HTTP and UDP servers had started. After running kubectl expose deployment hello-node --type=LoadBalancer --port=8080, similar startup logs appeared, suggesting the application restarted. Additionally, the logs then included HTTP GET requests, indicating access attempts. While the proxy into the service was running, each time the app was opened, the logs showed a corresponding increase in the number of HTTP GET request entries. This confirmed that the service was successfully routing traffic to the application and logging each access.

2. Notice that there are two versions of `kubectl get` invocation during this tutorial section. The first does not have any option, while the latter has `-n` option with value set to `kube-system`.
What is the purpose of the `-n` option and why did the output not list the pods/services that you explicitly created?

The -n option in the kubectl command is used to specify a particular namespace in a Kubernetes cluster. In Kubernetes, namespaces are used to separate objects within the cluster so that resources are more organized and isolated. If you use the -n kube-system option, the kubectl get command will only display objects that are within the kube-system namespace. The reason the output does not explicitly display pods/services is that those resources are likely created in another namespace.

### REFLECTION 2
1. What is the difference between Rolling Update and Recreate deployment strategy?
> Hint: Read the Deployments documentation.

In Kubernetes, the Rolling Update and Recreate deployment strategies offer different methods for updating applications. The Rolling Update strategy incrementally replaces old pods with new ones, ensuring continuous availability. It uses parameters like maxSurge and maxUnavailable to control the number of pods updated at a time, making it ideal for applications that require zero or minimal downtime and can handle old and new versions running simultaneously​ (Production-Grade Container Orchestration)​​ (BlueMatador)​. In contrast, the Recreate strategy stops all existing pods before starting new ones, resulting in a period of downtime. This approach is suitable for applications that cannot run multiple versions at the same time or when downtime is acceptable or can be scheduled during off-peak hours​ (DEV Community)​​ (BlueMatador)​. Each strategy has its use cases, depending on the application's tolerance for downtime and compatibility with concurrent versioning.

2. Try deploying the Spring Petclinic REST using Recreate deployment strategy and document
your attempt.
First, I ran kubectl edit deployments spring-petclinic-rest and edited the version in the output text file. Kubernetes then automatically applied the Recreate deployment strategy. This method involves shutting down all existing pods before creating new ones, ensuring that there is no overlap between old and new versions. 


3. Prepare different manifest files for executing Recreate deployment strategy.
It has been exported as `deploymentrecreate.yaml`

4. What do you think are the benefits of using Kubernetes manifest files? Recall your experience
in deploying the app manually and compare it to your experience when deploying the same app
by applying the manifest files (i.e., invoking `kubectl apply -f` command) to the cluster.
Using Kubernetes manifest files provides consistency, version control, and easier automation compared to manual deployment. Manifest files ensure the same configuration is applied across different environments, reducing human error and inconsistencies. They integrate well with CI/CD pipelines, enabling automated deployments and updates. The declarative approach of manifest files abstracts the complexity of underlying operations, simplifying management. Additionally, manifest files facilitate easier rollbacks and environment-specific customizations. In contrast, manual deployment is time-consuming, error-prone, and challenging to automate, especially for complex applications. Overall, manifest files streamline deployment processes, making them more efficient and reliable.

5. (Optional) Do the same tutorial steps, but on a managed Kubernetes cluster (e.g., GCP). You
need to provision a Kubernetes cluster on Google Cloud Platform. Then, re-run the tutorial steps
(Hello Minikube and Rolling Update) on the remote cluster. Document your attempt and highlight
the differences and any issues you encountered.