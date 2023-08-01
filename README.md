# pavel12513_platform
pavel12513 Platform repository
1. Команда валида, но могут быть случаи, когда процесс nginx запущен. Если процесс запущен, контейнер перейдет в статус running, но страничка может и не отдаваться в этот момент.

2. Создал Service type ClusterIP (по label присваивает pod ip адрес, который доступен только внутри kubernetes. С наруши, данный адрес не доступен):
pavel@pavel-desktop:~/Рабочий стол/Otus_kuber/pavel12513_platform/kubernetes-networks$ kubectl get svc | grep web
web-svc-cip              ClusterIP   10.99.176.237    <none>        80/TCP         3m19s

Сделал ping на данный адрес(не проходит):
# ping 10.99.176.237           
PING 10.99.176.237 (10.99.176.237): 56 data bytes
^C
--- 10.99.176.237 ping statistics ---
1 packets transmitted, 0 packets received, 100% packet loss
# 

3. Установил MetaILB:
pavel@pavel-desktop:~/Рабочий стол/Otus_kuber/pavel12513_platform$ kubectl --namespace metallb-system get all
NAME                              READY   STATUS    RESTARTS   AGE
pod/controller-5fd797fbf7-sbxkn   1/1     Running   0          38s
pod/speaker-rqfpf                 1/1     Running   0          38s

NAME                      TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)   AGE
service/webhook-service   ClusterIP   10.104.55.250   <none>        443/TCP   38s

NAME                     DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR            AGE
daemonset.apps/speaker   1         1         1       1            1           kubernetes.io/os=linux   38s

NAME                         READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/controller   1/1     1            1           38s

NAME                                    DESIRED   CURRENT   READY   AGE
replicaset.apps/controller-5fd797fbf7   1         1         1       38s

