$ minikube start --mount --mount-string="$(pwd)/logs:/example"
$ kubectl apply -f pod.yml
$ kubectl get all
