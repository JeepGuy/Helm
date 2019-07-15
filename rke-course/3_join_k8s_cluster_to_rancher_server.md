# Join cluster to Rancher.

Navigate to the URL for the Rancher Server


import cluster into rancher.

> Add Cluster  > Existing Cluster

Add the name of the cluster:   <your_name>  
Press create.

Copy the insecure curl command 

curl --insecure -sfL https://54.88.91.121/v3/import/wx4k77cdw45d9j4dv46s8gjsmj7h9svdmzbdkmhs9j9n7h6n86v5f7.yaml | kubectl apply -f -


ADD The ckubeconfig file to use to the curl command  (path to )  --kubeconfig=kube_config_cluster.yml

curl --insecure -sfL https://3.82.98.63/v3/import/wx4k77cdw45d9j4dv46s8gjsmj7h9svdmzbdkmhs9j9n7h6n86v5f7.yaml | kubectl  --kubeconfig=kube_config_cluster.yml apply  -f -



Paste into ssh session on the cluster master:


kubectl --kubeconfig=kube_config_cluster.yml  apply -f https://3.82.98.63/v3/import/wx4k77cdw45d9j4dv46s8gjsmj7h9svdmzbdkmhs9j9n7h6n86v5f7.yaml