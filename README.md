Step 1: Setup the Kubernetes cluster and deploy Hipster application as mentioned in "Hipster-Application-Setup" file.


Step 2: Fault Injection

Go to fault injection folder. Then inject fault one by one by feeding each yaml file in this command
    
    kubectl create -f <yaml-file-name>. 
 
Confirm that fault has been injected successfully through this command:
    
    kubectl get virtualservices
