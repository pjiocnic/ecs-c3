# ECS-C3: GitOps with Kubernetes and Argo

Video: https://www.youtube.com/watch?v=e3oRY_OCoF0
GitHub: https://github.com/sixeyed/ecs/tree/master/episodes/ecs-c3

```bash
brew install argocd

# output
argocd: v1.8.1+c2547dc.dirty
  BuildDate: 2020-12-10T04:45:44Z
  GitCommit: c2547dca95437fdbb4d1e984b0592e6b9110d37f
  GitTreeState: dirty
  GoVersion: go1.15.5
  Compiler: gc
  Platform: darwin/amd64
FATA[0000] Argo CD server address unspecified
```

```bash
PRASHANTHs-MBP:ec3-gitops-with-kubernetes-and-argo pjoisha$ kubectl apply -n argocd -f argo/
Warning: apiextensions.k8s.io/v1beta1 CustomResourceDefinition is deprecated in v1.16+, unavailable in v1.22+; use apiextensions.k8s.io/v1 CustomResourceDefinition
customresourcedefinition.apiextensions.k8s.io/applications.argoproj.io created
customresourcedefinition.apiextensions.k8s.io/appprojects.argoproj.io created
serviceaccount/argocd-application-controller created
serviceaccount/argocd-dex-server created
serviceaccount/argocd-redis created
serviceaccount/argocd-server created
role.rbac.authorization.k8s.io/argocd-application-controller created
role.rbac.authorization.k8s.io/argocd-dex-server created
role.rbac.authorization.k8s.io/argocd-redis created
role.rbac.authorization.k8s.io/argocd-server created
clusterrole.rbac.authorization.k8s.io/argocd-application-controller created
clusterrole.rbac.authorization.k8s.io/argocd-server created
rolebinding.rbac.authorization.k8s.io/argocd-application-controller created
rolebinding.rbac.authorization.k8s.io/argocd-dex-server created
rolebinding.rbac.authorization.k8s.io/argocd-redis created
rolebinding.rbac.authorization.k8s.io/argocd-server created
clusterrolebinding.rbac.authorization.k8s.io/argocd-application-controller created
clusterrolebinding.rbac.authorization.k8s.io/argocd-server created
configmap/argocd-cm created
configmap/argocd-gpg-keys-cm created
configmap/argocd-rbac-cm created
configmap/argocd-ssh-known-hosts-cm created
configmap/argocd-tls-certs-cm created
secret/argocd-secret created
service/argocd-dex-server created
service/argocd-metrics created
service/argocd-redis created
service/argocd-repo-server created
service/argocd-server created
service/argocd-server-metrics created
deployment.apps/argocd-dex-server created
deployment.apps/argocd-redis created
deployment.apps/argocd-repo-server created
deployment.apps/argocd-server created
statefulset.apps/argocd-application-controller created
```

```bash
PRASHANTHs-MBP:ec3-gitops-with-kubernetes-and-argo pjoisha$ kubectl get crd -n argocd
NAME                       CREATED AT
applications.argoproj.io   2021-04-17T13:39:12Z
appprojects.argoproj.io    2021-04-17T13:39:12Z
```

```bash
kubectl get pods -n argocd -l app.kubernetes.io/name=argocd-server -o jsonpath='{.items[0].metadata.name}'

# output
argocd-server-547d9bb879-shvss

pwd=$(kubectl get pods -n argocd -l app.kubernetes.io/name=argocd-server -o jsonpath='{.items[0].metadata.name}')
```

```bash
argocd login localhost --insecure --username admin --password $pwd
'admin' logged in successfully
Context 'localhost' updated
```

```bash
PRASHANTHs-MBP:ec3-gitops-with-kubernetes-and-argo pjoisha$ argocd cluster add docker-desktop
INFO[0000] ServiceAccount "argocd-manager" created in namespace "kube-system" 
INFO[0000] ClusterRole "argocd-manager-role" created    
INFO[0000] ClusterRoleBinding "argocd-manager-role-binding" created 
Cluster 'https://kubernetes.docker.internal:6443' added
```

```bash
PRASHANTHs-MBP:ec3-gitops-with-kubernetes-and-argo pjoisha$ kubectl describe clusterrole argocd-manager-role -n kube-system
Name:         argocd-manager-role
Labels:       <none>
Annotations:  <none>
PolicyRule:
  Resources  Non-Resource URLs  Resource Names  Verbs
  ---------  -----------------  --------------  -----
  *.*        []                 []              [*]
             [*]                []              [*]
```

```bash
echo $pwd
argocd-server-547d9bb879-shvss
```

Check the Argo CD UI at http://localhost, sign in with admin and echo $pwd

## Demo 2 - deploy an app in Argo CD