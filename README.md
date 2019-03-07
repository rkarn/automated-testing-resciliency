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
    2. Failover: An example of failover of network between checkout and payment service is given in "Resiliency folder". Run the command on both  service one by one. 
    kubectl create -f <YAML-file-name>
    3. CircuitBreaker: Run the below command to create circuit breaker. Yaml file is given.
    kubectl create -f circuit-breaker.yaml


Optional:

1. Use cadvisor monitoring tool to trace the CPU, memory and network profile of each container:
    
        cadvisor monitoring:
         docker run \
          --volume=/:/rootfs:ro \
          --volume=/var/run:/var/run:ro \
          --volume=/sys:/sys:ro \
          --volume=/var/lib/docker/:/var/lib/docker:ro \
          --volume=/dev/disk/:/dev/disk:ro \
          --publish=8080:8080 \
          --detach=true \
          --name=cadvisor \
          google/cadvisor:latest
          
          Then open the URL http://localhost:8080 
          
 2. Fault can also be injected by defining network policy given in kubernetes: 
 
        https://kubernetes.io/docs/concepts/services-networking/network-policies/#the-networkpolicy-resource
 
 3. Circuit breaker explained:
        
        https://github.com/IBM/resilient-java-microservices-with-istio
        https://techblog.constantcontact.com/software-development/circuit-breakers-and-microservices/
        https://redthunder.blog/2018/07/30/circuit-breaker-in-service-mesh-istio-envoy/
  
 4. Few useful links:
        
        Gremlin: https://www.computer.org/csdl/proceedings/icdcs/2016/1483/00/1483a057-abs.html
        sourcecode: https://github.com/ResilienceTesting/gremlinsdk-python
                    https://github.com/ResilienceTesting/gremlinproxy
                    
5. Tutorial on microservice:

        https://www.youtube.com/watch?v=1xo-0gCVhTU
