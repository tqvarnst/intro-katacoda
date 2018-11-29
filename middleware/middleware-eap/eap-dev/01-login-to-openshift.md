**Red Hat OpenShift Container Platform** is the preferred cloud runtime for JBoss EAP
OpenShift Container Platform is based on **Kubernetes** which is the most used Orchestration for containers running in production. We assume you understand the basics of OpenShift, if not, please complete [Introduction to OpenShift]( https://learn.openshift.com/introduction/) course and come back here.


**1. Login to OpenShift Container Platform**

In order to login, we will use the **oc** command and then specify the server that we
want to authenticate to:

```oc login -u developer -p developer```{{execute}}


Congratulations, you are now authenticated to the OpenShift server.


**2. Create project**


For this scenario, you will build a REST service that manages products that we deploy on OpenShift.

```
oc new-project product-service --display-name="Product Service"
```{{execute}}

**3. Open the OpenShift Web Console**

As you probably know, OpenShift ships with a web-based console that will allow users to
perform various tasks via a browser. To get a feel for how the web console
works, click on the "OpenShift Console" tab next to the "Local Web Browser" tab.

![OpenShift Console Tab](/openshift/assets/middleware/rhoar-getting-started-eap/openshift-console-tab.png)

The first screen you will see is the authentication screen. Enter your username and password and
then log in:

![Web Console Login](/openshift/assets/middleware/rhoar-getting-started-eap/login.png)

After you have authenticated to the web console, you will be presented with a
list of projects that your user has permission to work with.

TODO: REPLACE

![Web Console Projects](https://raw.githubusercontent.com/vblagoje/intro-katacoda/master/assets/middleware/rhoar-getting-started-rhdg/projects.png)


Click on your new project name to be taken to the project overview page
which will list all of the routes, services, deployments, and pods that you have
running as part of your project:

![Web Console Overview](https://raw.githubusercontent.com/vblagoje/intro-katacoda/master/assets/middleware/rhoar-getting-started-rhdg/overview.png)

There's nothing there now, but that's about to change.
