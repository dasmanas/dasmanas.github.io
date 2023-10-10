---
layout: post
title:  "Access Kubernetes Dashboard of Docker Desktop"
date:   2023-09-25 19:30:00 +0800
categories: cloud container docker kubernetes dashboard
mermaid: true
---
Docker desktop comes with kubernetes pre-packaged. It is also expected we should be able to access Kubernetes Dashboard. It requires few tweaks to view your Docker's pre-packaged Kube Cluster using Kubernetes Dashboard UI. Here are the steps.

Deploy the Dashboard UI in you Kubernetes Cluster following [link](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/)

Most of the users face the problem of login into cluster using Kubernetes Dashboard UI. The reason is how to get the token. It requires to create a Service Account and Cluster Role Binding for the service account.

Here is an yaml to create the service account along with a cluster binding role to `cluster-admin` which will require to connect to the dashboard.
```
cat >sacrb.yaml <<EOF
apiVersion: v1
kind: ServiceAccount
metadata:
  name: admin-user
  namespace: kubernetes-dashboard
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: admin-user
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: admin-user
  namespace: kubernetes-dashboard
EOF
```

Apply the service account + cluster role bindings
```
kubectl apply -f sacrb.yaml
```


Using the below statements you will be able to see an user has been created named `admin-user` and a Cluster Role has been assigned to the user.
```
kubectl get serviceaccount -n kubernetes-dashboard
kubectl get clusterrolebindings | grep admin-user
```

Now you need to create temporary token for the admin-user to use it at Kubernetes Dashboard UI.
```
kubectl -n kubernetes-dashboard create token admin-user
```

You are now all set to use the token. You can even use [JSON Web Tokens](https://jwt.io/) to decode the the token for further details.