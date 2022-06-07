# Native EKS Ingress & LoadBalancer: AWS Load Balancer Controller & 5 Examples (Insynce mode vs native)

You can find tutorial [here]().

I think the biggest problem people face with installing controller, that's why I'll show you both ways YAML & Helm
YAML & Helm





aws eks update-kubeconfig --name demo --region us-east-1


kubectl apply --validate=false \
-f https://github.com/jetstack/cert-manager/releases/download/v1.5.3/cert-manager.yaml

kubectl get pods -n cert-manager

wget https://github.com/kubernetes-sigs/aws-load-balancer-controller/releases/download/v2.4.1/v2_4_1_full.yaml

update cluster name in deployment

add annotation to sa
```
  annotations:
    eks.amazonaws.com/role-arn: arn:aws:iam::424432388155:role/aws-load-balancer-controller
```

kubectl apply -f v2_4_1_full.yaml

kubectl get pods -n kube-system

(keep it open)
kubectl logs -f -n kube-system \
-l app.kubernetes.io/name=aws-load-balancer-controller

curl -d '{"name": "anton"}' localhost:8080/

kubectl get ingressclass

you can get this error
```
{"level":"error","ts":1654499339.4368656,"logger":"controller-runtime.manager.controller.ingress","msg":"Reconciler error","name":"echoserver","namespace":"default","error":"couldn't auto-discover subnets: unable to discover at least one subnet"}
```

if you missed --cluster-name

kubectl get ing

curl http://echo.devopsbyexample.io



## Deploy with helm

helm repo add eks https://aws.github.io/eks-charts

kubectl apply -f service-account.yaml

helm install aws-load-balancer-controller eks/aws-load-balancer-controller \
-n kube-system \
--set clusterName=demo \
--set serviceAccount.create=false \
--set serviceAccount.name=aws-load-balancer-controller
