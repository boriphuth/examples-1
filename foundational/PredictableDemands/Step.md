$ minikube start --mount --mount-string="$(pwd)/logs:/example"
$ kubectl apply -f pod.yml
$ kubectl get pods
NAME               READY   STATUS    RESTARTS   AGE
random-generator   0/1     Pending   0          27m

$ kubectl describe pod random-generator
Events:
  Type     Reason            Age                    From               Message
  ----     ------            ----                   ----               -------
  Warning  FailedScheduling  2m27s (x153 over 27m)  default-scheduler  persistentvolumeclaim "random-generator-log" not found

  $ kubectl apply -f pv-and-pvc.yml
persistentvolume/example created
persistentvolumeclaim/random-generator-log created

$ kubectl describe pod random-generator
Events:
  Type     Reason            Age                  From               Message
  ----     ------            ----                 ----               -------
  Warning  FailedScheduling  89s (x183 over 31m)  default-scheduler  persistentvolumeclaim "random-generator-log" not found
  Normal   Pulling           89s                  kubelet, minikube  pulling image "k8spatterns/random-generator:1.0"
  Normal   Pulled            6s                   kubelet, minikube  Successfully pulled image "k8spatterns/random-generator:1.0"
  Warning  Failed            5s (x2 over 6s)      kubelet, minikube  Error: configmap "random-generator-config" not found
  Normal   Pulled            5s                   kubelet, minikube  Container image "k8spatterns/random-generator:1.0" already present on machine

$ kubectl apply -f configmap.yml
configmap/random-generator-config created

$ kubectl get pods
NAME               READY   STATUS    RESTARTS   AGE
random-generator   1/1     Running   0          33m
