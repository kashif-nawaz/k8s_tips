# k8s_tips 

## Access kube-api via curl 
https://kubernetes.io/docs/tasks/administer-cluster/access-cluster-api/

kubectl proxy --port=8080 &
curl http://localhost:8080/api/

## Delete a namespace Stuck in Terminating State 

https://www.redhat.com/sysadmin/troubleshooting-terminating-namespaces

NAMESPACE=dev-ns
kubectl get namespace ${NAMESPACE} -o json > tmp.json

vim tmp.json

 "spec": {
        "finalizers": [
            "kubernetes"
        ]
    },

remove  "kubernetes" from above

"spec": {
        "finalizers": [
        ]
    },


curl -k -H "Content-Type: application/json" -X PUT --data-binary @tmp.json http://127.0.0.1:8080/api/v1/namespaces/${NAMESPACE}/finalize

## Delete PODs Stuck in Terminating State 
https://lepczynski.it/en/k8s_en/k8s-pods-stuck-on-terminating/
