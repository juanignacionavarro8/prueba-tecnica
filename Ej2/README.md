## Deployment types

### Deployment using Docker Compose
Inside a Linux machine with Ubuntu, Docker installed correctly, access to the image registry, you can use the “docker compose up” command to deploy the services found in the docker-compose.yml file.

This deployment is simple and compose does not add any functionality other than allowing you to deploy various services within a docker-compose.yml file. In this case I used the database service that consumes the backend, the backend itself and the frontend. In case you need to mount volumes either for configuration files or for data volumes, they should be added in the docker-compose.yml under the volumes section.

It is necessary to have in the backend the file with the database connection data, which is not very secure.

### Deployment using Helm Charts and Kubernetes

It is decided to create a Helm chart for the frontend and another one for the backend to take advantage of benefits such as:
- Microservice independence: updates or failures are isolated affecting only the backend or frontend component, not both at the same time. 
- Possibility of simple component roll-back, allowing greater granularity in the configurations of each application component.

Both Helm Charts have interesting features to highlight:
- Possibility to configure customized securityContexts, allowing to define best practices such as not running container with root user, that the FileSystem is ReadOnly, and that each application runs with the corresponding UID and GID allowing to exercise the minimum privileges for each participant of the application lifecycle.
- Possibility of defining the probes according to the design of the microservices themselves, being able to choose different probes strategies according to the characteristics and design of our container.
- Possibility to define autoScaling based on metrics allowing the design to support varying loads.
- Possibility to define serviceAccount to improve the security of the cluster and the communications of the different components of the cluster.
- Possibility to define an Ingress resource so that the frontend service (or backend if required) can be accessed from outside the cluster. It is necessary to have extra considerations such as the installation of an Ingress Controller and the necessary configuration to resolve on the client machine to resolve the URL defined in the Ingress resource.
- Possibility to define tolerations, nodeSelector, affinity which are all native Kubernetes features to define the scheduling that the pods of each Deployment will have. They can be used to define high availability strategies such as single point of failure avoidance. In that case, the use of Inter-Affinity between pods allows not to assign two backend pods to the same node. This results in the benefit of knowing that if a node goes down, the backend service will not be offline because it will have another instance running on a different node. This is assuming the case that more than one replica was defined in the Deployment.