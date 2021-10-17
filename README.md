## Решение
1. Устанавливаем calico через kubespray
``` git clone https://github.com/kubernetes-sigs/kubespray.git
cd kubespray/
pip3 install -r requirements.txt
ansible-playbook -i inventory/local/hosts.ini cluster.yml
kubectl get po --all-namespaces
```
2. Убедились, что калико запустился ![getpo.png](https://github.com/Probablyall/12.5-CNI/blob/main/getpo.PNG)
3. По [этой](https://docs.projectcalico.org/security/tutorials/kubernetes-policy-basic) ссылке настраиваем политику доступа. В тело решения решил не копипастить, делал все команды один в один.
Результат выполнения когда есть доступ:
```
root@node1:~# kubectl run --namespace=policy-demo access --rm -ti --image busybox /bin/sh
If you don't see a command prompt, try pressing enter.
/ # wget -q --timeout=5 nginx -O -
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
/ # 
```
5. Результат выполнения когда нет доступа:
```
root@node1:~# kubectl run --namespace=policy-demo cant-access --rm -ti --image busybox /bin/sh
If you don't see a command prompt, try pressing enter.
/ # wget -q --timeout=5 nginx -O -
wget: download timed out
```
6. Под также отображается ![getpo2.png](https://github.com/Probablyall/12.5-CNI/blob/main/getpo2.PNG)
7. Вывод команд, который сказали сделать на лекции:
```
root@node1:~# calicoctl get nodes
NAME
node1

root@node1:~# calicoctl get ipPool
NAME           CIDR             SELECTOR
default-pool   10.233.64.0/18   all()
