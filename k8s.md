在pod中执行命令但不进入pod

`kubectl exec pod-name cat /etc/downward/podname`

同一个pod中的运行多个容器，想进入其中一个容器执行命令 -c

`kubectl exec -it pod-name -c container-name bash`

执行滚动升级

`kubectl rolling-update kubia-v1 kubia-v2 --image=luksa/kubia:v2`(过时的)

kubia-v1升级到kubia-v2,镜像使用`luksa/kubia:v2`

滚动更新时，使用--v可以打开详细日志

`kubectl rolling-update kubia-v1 kubia-v2 --image=luksa/kubia:v2 --v 6`

使用--v 6会提高日至级别使得所有kubectl发起的所有到API服务器的请求都会被输出



使用--record会记录历史版本号

`kubectl create -f kubia-deployment-v1.yaml --record`

查看部署状态

`k rollout status deployment kubia`

直接改变deployment的属性

`k patch deployment kubia -p '{"spec":{"minReadySeconds": 10}}'`

取消，会滚最后一次的升级

`k rollout undo deployment kubia`

undo命令可以在升级过程中运行，会直接停止滚动升级，已创建的新pod会被旧pod代替

显示滚动升级会滚过程中的历史版本

`k rollout history deployment kubia`

如果想回滚到特定的版本

`k rollout undo deployment kubia --to-revision=1`

暂停滚动升级

`k rollout pause deployment kubia`

恢复升级

`k rollout resume deployment kubia`

通过为deployment添加就绪探针（配合mineadyeconds）可以在升级出错时停止升级，配置ProgresseadlineSeconds指定在多长时间内升级未完成便判定为升级失败

------

`kubectl get pods --watch`

每当创建，修改，删除pod时会通知，即实时显示pod的状态

或者打印整个监听事件的yaml文件

`kubectl get pods -o yaml --watch`

------

更新集群

`gcloud beta container clusters update cxiang-k8s-cluster --enable-pod-security-policy`

