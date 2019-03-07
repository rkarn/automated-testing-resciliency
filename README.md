Step 1: Setup the Kubernetes cluster and deploy Hipster application as mentioned in "Hipster-Application-Setup" file.


Step 2: Fault Injection

Go to fault injection folder. Then inject fault one by one by feeding each yaml file in this command
    
    kubectl create -f <yaml-file-name>
 
Confirm that fault has been injected successfully through this command:
    
    kubectl get virtualservices
    
To delete the injected fault, run this command:

    kubectl delete -f <yaml-file-name>
Step 3: Locust Integration and Load testing

Install Locust from https://docs.locust.io/en/stable/installation.html

Quick Start guide is available at https://docs.locust.io/en/stable/quickstart.html 

Run this command for load testing:

    locust -f locustfile-complete.py  --host=http://localhost:31380 --print-stats

    http://localhost:31380 is the frontend-url. Filename "locustfile-complete.py" is provided.
    
    Open the Locust GUI from : http://localhost:8089/ 
    
    Enter the number of virtual user (e.g. 100) and the swarming rate (e.g.10).  

Step 4 : Resiliency

Open the Kubernetes dashboard through URL:
   
    kubectl proxy
    
    Open the URL:   http://localhost:8001/api/v1/namespaces/kube-system/services/kubernetes-dashboard/proxy 
    
    1. Scaling: Go to Deployment tab and select a deployment to scale. Click on the right hand side menu and click on scale. Set the number you want replicas and click OK.
    2. Failover: 
