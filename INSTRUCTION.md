kubectl apply -f configMap.yml
kubectl apply -f secret.yml
Then:
kubectl apply -f deployment.yml

You can check resources
kubectl get configmap
kubectl get secret
kubectl get deployments
kubectl get pods

kubectl exec -it <pod-name> -- sh
use commands:
echo $PYTHONUNBUFFERED
echo $SECRET_KEY

after first should be 
1
after second should be
@e2(yx)v&tgh3_s=0yja-i!dpebxsz^dg47x)-k&kq_3zf*9e*