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








wget https://github.com/kubernetes-sigs/aws-load-balancer-controller/releases/download/v2.4.2/v2_4_2_full.yaml





## IAM Roles for Service Accounts via AWS Console
terraform apply
copy OpenID Connect provider URL
create identety provider (sts.amazonaws.com)
show how to get policy (https://github.com/kubernetes-sigs/aws-load-balancer-controller)
- switch to version
- go to docs/install/
- copy iam_policy.json
- give it a name AWSLoadBalancerController

cretae iam role (web identety)
add aws load balancer controller policy
call it aws-load-balancer-controller
update trust relationship
sub: system:serviceaccount:kube-system:aws-load-balancer-controller

## IAM Roles for Service Accounts via Terraform
- create 8-iam-oidc.tf
- create 9-iam-controller.tf
- copy AWSLoadBalancerController.json
- terraform init
- terraform apply

## Deploy AWS Load Balancer Controller with YAML
Install cert-manager
kubectl apply --validate=false \
-f https://github.com/jetstack/cert-manager/releases/download/v1.5.3/cert-manager.yaml
kubectl get pods -n cert-manager
wget https://github.com/kubernetes-sigs/aws-load-balancer-controller/releases/download/v2.4.1/v2_4_1_full.yaml
update:
- service account annotation (579), get arn from the aws console
```
  annotations:
    eks.amazonaws.com/role-arn: arn:aws:iam::424432388155:role/aws-load-balancer-controller
```
- clustre name (825) get name from the console
- image tag (827)


kubectl apply -f v2_4_1_full.yaml

## Deploy AWS Load Balancer Controller with HELM

helm repo add eks https://aws.github.io/eks-charts

kubectl apply -f service-account.yaml

helm install aws-load-balancer-controller eks/aws-load-balancer-controller \
-n kube-system \
--set clusterName=demo \
--set serviceAccount.create=false \
--set serviceAccount.name=aws-load-balancer-controller




## Deploy AWS Load Balancer Controller with Terraform & Helm Provider



## 1-example

https://github.com/kubernetes-sigs/aws-load-balancer-controller/issues/1695
Instance mode will require at least NodePort service type. With NodePort service type, kube-proxy will open a port on your worker node instances to which the ALB can route traffic.
Doc references -
https://kubernetes-sigs.github.io/aws-load-balancer-controller/latest/guide/ingress/spec/
https://kubernetes-sigs.github.io/aws-load-balancer-controller/latest/guide/ingress/annotations/
https://kubernetes-sigs.github.io/aws-load-balancer-controller/v2.2/guide/ingress/annotations/

kubectl logs -f -n kube-system \
-l app.kubernetes.io/name=aws-load-balancer-controller

kubectl get ingressclass

kubectl apply -f k8s/1-example.yaml

create CNAME record

curl http://echo.devopsbyexample.io

show aws alb load balancer in the console
get listner