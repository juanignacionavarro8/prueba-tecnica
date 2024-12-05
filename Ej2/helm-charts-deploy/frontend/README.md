# **K8s frontend Unit**

## **Helm Chart structure**

```bash
frontend
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

#### **1. Clone project in the k8s cluster's master node**
   - `git clone https_url_to_clone `
   - `cd frontend`

#### **2. Configure deploy**
   -   Check the values set in file values.yaml (See default values in table below)
   -   Check configs (included in configmaps)


#### **3. Launch Helm Chart**
   - Run the command `helm install frontend . -f values.yaml -n craftech-ex2 --create-namespace craftech-ex2`

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