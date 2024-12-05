# **K8s backend Unit**

## **Helm Chart structure**

```bash
backend
├── Chart.yaml
├── README.md
├── .helmignore
├── values.yaml 
└── templates
   ├── _helpers.tpl
   ├── deployment.yaml
   ├── hpa.yaml
   ├── ingress.yaml
   ├── service.yaml
   ├── serviceaccount.yaml
   └── NOTES.txt
```


## **Deploy Helm Chart**

### **Steps to deploy**

#### Before deploy
Note that the backend chart uses a dependency on a PostgreSQL chart, which can be for example one of Bitnami or another. It is necessary to add the environment variables that came from the .env.postgres file in the public repository of the backend. It could be added as a ConfigMap or an additional Secret to the deployment of the official or Bitnami chart.
The correct configuration of a values.yaml file of the PostgreSQL subchart must be taken into account. This is achieved by testing the different configurations and verifying that the PostgreSQL pod(s) are lifting correctly and the backend can connect successfully.

#### **1. Clone project in the k8s cluster's master node**
   - `git clone https_url_to_clone `
   - `cd backend`

#### **2. Configure deploy**
   -   Check the values set in file values.yaml (See default values in table below)
   -   Check configs (included in configmaps)


#### **3. Launch Helm Chart**
   - Run the command `helm install backend . -f values.yaml -n craftech-ex2 --create-namespace craftech-ex2`

#### **4. Verify all components have been successfully deployed**
   - Run the command `kubectl get all -n craftech-ex2`

## **Remove Helm Chart**
   - `helm uninstall ccm -n craftech-ex2`

## **Debugging**
   Execute the following command for the pod of interest:
   - `kubectl logs POD_NAME -n craftech-ex2`

## **References**
- https://helm.sh/docs/intro/install/ 
- https://kubernetes.io/docs/home/