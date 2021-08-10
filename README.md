# Enable simple mTLS on OpenShift ServiceMesh

1. Create istio-system project
```shell
oc create ns istio-system
```
2. Apply manifests to deploy istio control plane and enable projects to use istio
```shell
#Note write your own project name as list
oc apply -f deploy-istio-control-plane/enable-istio-on-project.yaml

#Deploy istio control plane
oc apply -f deploy-istio-control-plane/install-istio-control-plane.yaml
```
3. Inject to your igressgateway Certificates, necessary only if mTLS and Gateway https is used
```shell
oc create -n istio-system secret tls istio-ingressgateway-certs --key <PATH_TO_KEY>/tls.key --cert <PATH_TO_CERT>/tls.crt
```
4. Admit to your needs application related gateway, virtual service etc. and apply
```shell
oc apply -n <YOUR_PROJECT_FROM_MEMBER_LIST> -f app-mtls-gateway-virtual-service/destination-rule.yaml
oc apply -n <YOUR_PROJECT_FROM_MEMBER_LIST> -f app-mtls-gateway-virtual-service/gateway.yaml
oc apply -n <YOUR_PROJECT_FROM_MEMBER_LIST> -f app-mtls-gateway-virtual-service/peer-auth.yaml
```
>Note: You can apply all manifests together just simply use -

><b>Info:</b> Please note that Istio provides more features, which are not shown here,
> let me know about particular feature, and I will prepare demo manifest and use case.
> 

Now only basic simple feature demonstrated.