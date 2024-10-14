# Authentication
**Authentication** konusuyla ilgili dosyalara buradan erişebilirsiniz.



**Key ve CSR oluşturma**
```
$ openssl genrsa -out ozgurozturk.key 2048 

$ openssl req -new -key ozgurozturk.key -out ozgurozturk.csr -subj "/CN=ozgur@ozgurozturk.net/O=DevTeam"
```

**CertificateSigningRequest oluşturma**

```
$ cat <<EOF | kubectl apply -f -
apiVersion: certificates.k8s.io/v1
kind: CertificateSigningRequest
metadata:
  name: nuray
spec:
  groups:
  - system:authenticated
  request: $(cat nuray.csr | base64 | tr -d "\n")
  signerName: kubernetes.io/kube-apiserver-client
  usages:
  - client auth
EOF
```

**CSR onaylama ve crt'yi alma**

```
$ kubectl get csr

$ kubectl certificate approve nuray

$ kubectl get csr nuray -o jsonpath='{.status.certificate}' | base64 -d >> nuray.crt 
```

**kubectl config ayarları**

```
$ kubectl config set-credentials bayrakdarnuray@gmail.com --client-certificate=nuray.crt --client-key=nuray.key

$  
$ kubectl config use-context nuray-context
```