# Here is what I learnt and the pain points

- Kubernetes .yaml have a lot of different settings. Not all of them can be learnt at once. Future practice in real life conditions will help.

- Use Kubernetes namespaces whenever possible to properly structure the cluster

- configmaps are convenient to flawlessly deploy the same app with different settings (deploy prod vs staging for example)

- It is necessary to add a configmap in the myapp-chart/templates folder, as all the files within that folder will be deployed within kubernetes.

- You shall update the version of the helm chart everytime you make an update

- You shall add a message if possible whenever you perform a helm upgrade or rollback

- One should not manually force the port of the deployed add 