
[Linkerd Demo](https://linkerd.io/2.16/getting-started/#step-4-install-the-demo-app)

**Step 1: Install the CLI**
$ curl --proto '=https' --tlsv1.2 -sSfL https://run.linkerd.io/install-edge | sh

$ export PATH=$HOME/.linkerd2/bin:$PATH

$ linkerd version

**Step 2: Install Linkerd onto your cluster** 

$ linkerd install --crds | kubectl apply -f -

$ linkerd install | kubectl apply -f -

$ linkerd check

linkerd-existence
-----------------
× 'linkerd-config' config map exists
    configmaps "linkerd-config" not found
    see https://linkerd.io/2/checks/#l5d-existence-linkerd-config for hints

Status check results are ×

Bu hata, Kubernetes node'larınızın Docker container runtime'ını kullandığını ve Linkerd proxy-init container'ının root olarak çalışması gerektiğini belirtiyor.

Solution: Linkerd'yi proxy-init container'ını root kullanıcısı olarak çalıştıracak şekilde kurmanız gerekiyor.

$ linkerd install --set proxyInit.runAsRoot=true | kubectl apply -f -

**Step 3:Install the Demo App**

Install Emojivoto into the emojivoto namespace by running:

$ curl --proto '=https' --tlsv1.2 -sSfL https://run.linkerd.io/emojivoto.yml \
  | kubectl apply -f -

Forward web-svc locally to port 8080 by running and check emojivoto app:

$ kubectl -n emojivoto port-forward svc/web-svc 8080:80


Bu komut, emojivoto namespace'indeki deployment'ları alır, bunlara Linkerd sidecar proxy'sini enjekte eder ve ardından bu güncellenmiş deployment'ı Kubernetes'e apply eder:

$ kubectl get -n emojivoto deploy -o yaml \
  | linkerd inject - \
  | kubectl apply -f -

**Step 4: Explore Linkerd**

Bu komut, Kubernetes cluster'ınıza Linkerd Viz bileşenini kurarak Linkerd'nin gözlemleme ve metrik toplama özelliklerini aktif hale getirir:

$ linkerd viz install | kubectl apply -f -

$ linkerd check

Access the dashboard with:

$ linkerd viz dashboard &

http://localhost:50750/namespaces

Linkerd Viz Neleri Sağlar: 
- Grafana ve Prometheus tabanlı metrik görselleştirme.
- Uygulamalar arasındaki isteklerin gecikme, hata oranları ve başarı oranları gibi metriklerini toplar.
- Uygulama ağ trafiğini detaylı olarak takip edebilme ve performans izleme.
- Web dashboard aracılığıyla bu metriklerin görselleştirilmesi ve daha kolay takip edilmesi.

**Example**

https://bdemirpolat.medium.com/linkerd-ile-service-mesh-5b96769a36e4