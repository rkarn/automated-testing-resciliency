# automated-testing-resciliency

Setup the Kubernetes cluster. Opensource minikube cluster can be setup from : https://github.com/kubernetes/minikube


Run these commands to setup the Hipster application on Kubernetes cluster:
1. git clone https://github.com/GoogleCloudPlatform/microservices-demo 
2. cd microservices-demo/kubernetes-manifests 
3. Deploy each service manually in this order 
    kubectl create -f redis.yaml
    kubectl create -f frontend.yaml
    kubectl create -f shippingservice.yaml
    kubectl create -f productcatalogservice.yaml
    kubectl create -f recommendationservice.yaml
    kubectl create -f paymentservice.yaml 
    kubectl create -f currencyservice.yaml
    kubectl create -f emailservice.yaml
    kubectl create -f checkoutservice.yaml
    kubectl create -f adservice.yaml
    kubectl create -f cartservice.yaml
4.Verify that each of the services nad deployments are runing. Run these commands to confirms:
    kubectl get pods
    kubectl get deployments
    kubectl get services
5. Run this command to get front-end port number.
    kubectl get services frontend-external  -o wide
    Example of the output:
    NAME                TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE    SELECTOR
    frontend-external   LoadBalancer   10.101.172.246   <pending>     80:30907/TCP   110d   app=frontend
6. Access the application with service port that is mapped with port 80 of the host. In the above example, it is port 30907. So the URL is    localhost:30907.
7.Install the automatic sidecar injection (annotate the default namespace with the label):
  kubectl label namespace default istio-injection=enabled
8.Install the Istio and apply from istio-manifests directory. (This is required only once.)
  cd ..
  kubectl apply -f ./istio-manifests
9.Verify that the application can be accessed within Istio mesh. Run this command to get port number:
   kubectl get services istio-ingressgateway -n istio-system
  Example: 
  NAME                   TYPE           CLUSTER-IP     EXTERNAL-IP   PORT(S)                                                                                                                   AGE
istio-ingressgateway   LoadBalancer   10.111.98.42   <pending>     80:31380/TCP,443:31390/TCP,31400:31400/TCP,15011:30882/TCP,8060:30602/TCP,853:31811/TCP,15030:32430/TCP,15031:32601/TCP   110d
 Locate the port number that is mapped with port 80 of the host. In above example, the port is 31380. URL of the application is: localhost:31380
