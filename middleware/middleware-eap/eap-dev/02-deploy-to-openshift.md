Now that you've logged into OpenShift, let's deploy a single JBoss EAP instance.
The deployed JBoss EAP will in this step not be clustered or connected to any database, but we will see example of that in the following scenarios. 

**1. Deploy JBoss EAP image**

In this example we will use JBoss EAP CD 14, where CD stands for Continuous Development. For cloud use Red Hat offer images with JBoss EAP that are updated much more frequent than the traditional JBoss EAP release stream. The main reason for having a slow release stream on the main product is that users that are manually installing and configuring their application servers don't want to upgrade to often. However, when using a cloud deployment like OpenShift upgrades can be automated and does not take a lot of effort.

To deploy our application we first have to create a container. JBoss EAP comes with a S2I image that packages all the binaries that we need and by adding 

```oc new-build registry.access.redhat.com/jboss-eap-7-tech-preview/eap-cd-openshift:14 --binary=true --name=product-service```{{execute}}

Now, we need to build the application that we are going to deploy.

We have already provided a simple rest application that we can use as starting point. 

First move to the project directory:

```cd ~/projects/product-service```{{execute}}

Now, build the application it by executing the following maven command.

```mvn clean package```{{execute}}

We are now ready to upload our artifact (a WAR file) to the build "product-service" process that we created earlier. The build process will add the artifact to the eap-cd-openshift image and create a new container consisting of EAP and the war file.

```oc start-build product-service --from-file=target/greeting-javaee.war --wait``{{execute}}

We are now ready to create our application.

```oc new-app product-service```{{execute}}

Now, go over to the web console and create 


```oc new-app registry.access.redhat.com/jboss-eap-7-tech-preview/eap-cd-openshift:14 --name rest-app```{{execute}}



The command new-app will pull a Docker image from internal RedHat repository and create a service named rest-app. You'll see the following output:

```console

--> Creating resources ...
    imagestream "rest-app" created
    deploymentconfig "rest-app" created
    service "rest-app" created
--> Success
    Run 'oc status' to view your app.
```





**2. Expose RHDG service route**

Now that we have a rhdg service created, we need to expose it to the outside world using OpenShift expose command.
Let's do that next.

```oc expose svc rhdg --name=rhdgroute```{{execute}}

Let's look at the details of rhdg route route resource. We'll use OpenShift describe command:

```oc describe route rhdgroute```{{execute}}

You'll see the output similar to this:

```console
Name:                   rhdgroute
Namespace:              example
Created:                5 minutes ago
Labels:                 app=rhdg
Annotations:            openshift.io/host.generated=true
Requested Host:         jdgroute-example.2886795312-80-simba02.environments.katacoda.com
                          exposed on router router 5 minutes ago
Path:                   <none>
TLS Termination:        <none>
Insecure Policy:        <none>
Endpoint Port:          8080-tcp

Service:        rhdg
Weight:         100 (100%)
Endpoints:      172.20.0.3:8080, 172.20.0.3:11333, 172.20.0.3:8443 + 3 more...
```

Now that we have a single RHDG instance running and exposed to the outside world, let's make some use of it.
