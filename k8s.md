在pod中执行命令但不进入pod

`kubectl exec pod-name cat /etc/downward/podname`

同一个pod中的运行多个容器，想进入其中一个容器执行命令 -c

`kubectl exec -it pod-name -c container-name bash`

