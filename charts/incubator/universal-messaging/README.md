# Universal Messaging
This chart deploys a [Universal Messaging](https://www.softwareag.com/corporate/products/az/universal_messaging/default.html) instance or number of instances on a Kubernetes cluster using Helm package manager.

## Prerequisites
- Kubernetes version x.x (TBD)
- Helm 3 as it does not need Tiller for installing the charts
- Persistent volume support

## Installing the chart
### Adding Software AG repo to local helm
As of now charts ara available in incubator only, no stable versions. This will be done in short term.
```bash
$ helm repo add sag-helm-repo https://softwareag.github.io/webmethods-helm-collection/charts-repo/incubator
```
One should update the repository contents by running:
```bash
$ helm repo update
```
### Checkig out the product in [dockerhub](https://hub.docker.com)
Since Universal Messaging image the chart is using is hosted on [dockerhub](https://hub.docker.com) there is a short procedure that should be followed:
- Log in to [dockerhub] (https://hub.docker.com)
- Find the Universal Messaging [image here](https://hub.docker.com/_/softwareag-universalmessaging-server)
- click on *proceed to Checkout* and agree with the licenses and terms
- Then one should either prepare a yaml file with docker credentials
### simple chart installation
After that installing the Universal Messaging is as simple as running:
```bash
helm install um1 --set imageCredentials.username="<dockerhub_username>" --set imageCredentials.password="<dockerhub_password>"  sag-helm-repo/universal-messaging
```
Where <dockerhub_username> and <dockerhub_password> are the credentials used in the steb above 
This will spin off a single instance of Universal Messaging with some initial configuration.
### More advanced scenarios (options can be combined)
- If one does not want to expose the credential details in command line it is possible to create an yaml file (i.e. "~/docker-credentials.yaml" in advanse with the following contents:
```yaml
imageCredentials:
  registry: "https://index.docker.io/v1/"
  username: "<dockerhub_username>"
  password: "<dockerhub_password>"
```
and then use it when installing the chart like that:
```bash
helm install um1 -f ~/docker-credentials.yaml sag-helm-repo/universal-messaging
```
- Spinnig more then one replica
``` bash
helm install um1 -f ~/docker-credentials.yaml --set replicaCount=3  sag-helm-repo/universal-messaging
```
- Providing a custom license. The one included in the docker image is time limited. i.e. the custom license is located and named "~/my-custom-license.xml"
``` bash
helm install um1 -f ~/docker-credentials.yaml --set-file externalFiles.licenseFile=~/my-custom-license.xml   sag-helm-repo/universal-messaging
```
- Providing a custom configuration as UM realm export. i.e. the custom config file is located and named "~/my-custom-config.xml"
``` bash
helm install um1 -f ~/docker-credentials.yaml --set-file externalFiles.configFile="~/my-custom-config.xml" sag-helm-repo/universal-messaging
```