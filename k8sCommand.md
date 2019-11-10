# 14. 计算机资源管理

`k top node`

显示了节点上运行的所有pod当前的CPU和内存的实际使用量

`k describe node`

显示节点CPU和内存的request和limits

`k top pod` or `k top pod --all-namespaces`

查看每个pod使用了多少资源

以上是通过heapster中获取的指标，heapster是从cAdvisior中统一收集的，cAdvisior是从每个pod中收集的

`kubectl top pod POD_NAME --containers`

查看容器的资源使用情况

直接命令创建autoscale

`k autoscale deployment kubia --cpu-percent=30 --min=1 --max=5`

直接命令创建service

`k expose deployment kubia --port=80 --target-port=8080`

并行观察多个资源

`watch -n 1 kubectl get hpa,deployment`

运行一个临时的pod

`k run -it --rm --restart=Never loadgenator --image=busybox -- sh -c "while true; do wget -O - -q http://kubia.default;done"`

--rm: ctrl+C后会删除这个pod

------

`k cordon <node>`

标记某个节点为不可调度，但对其上的pod不做任何事

`k drain <node>` 

标记节点为不可调度，随后疏散其上所有的pod

两种情形下，在你用`k uncordon <node>`解除节点的不可调度状态之前，不会有新的pod被调度到该节点

------

`k create pdb kubia-pdb --selector=app=kubia --min-avaliable=3`

创建一个PodDisruptionBudget资源，确保具有标签app=kubia的pod总有3个示例在运行