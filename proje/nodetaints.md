*Node'a taint ekleme*

kubectl taint nodes minikube-m02 env=production:NoSchedule               

*Pod'da bu tainti belirtme* 

apiVersion: v1
kind: Pod
metadata:
  name: production-pod
spec:
  containers:
  - name: nginx
    image: nginx
  tolerations:
  - key: "env"
    operator: "Equal"
    value: "production"
    effect: "NoSchedule"

*Secret oluşturma*

$ kubectl create secret generic mysql-test-secret -n test --from-file=MYSQL_ROOT_PASSWORD=mysql_root_password.txt --from-file=MYSQL_USER=mysql_user.txt --from-file=MYSQL_PASSWORD=mysql_password.txt --from-file=MYSQL_DATABASE=mysql_database.txt $

$ kubectl create secret generic mysql-prod-secret -n production --from-file=MYSQL_ROOT_PASSWORD=mysql_root_password.txt --from-file=MYSQL_USER=mysql_user.txt --from-file=MYSQL_PASSWORD=mysql_password.txt --from-file=MYSQL_DATABASE=mysql_database.txt $

