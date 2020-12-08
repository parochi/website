---
layout: post
title: Kubernetes - ConfigMaps & Secrets
date: 2020-11-19
related_image: /assets/images/2020-11-26.png
---

<div class="view overlay">
	<img class="card-img-top" src="{{ page.related_image }}" alt="Card image cap">
    <a href="#!">
        <div class="mask rgba-white-slight"></div>
    </a>
</div>

One of the features that we can leverage, when we are working with Kubernetes is the ConfigMap. This is used to store the non-confidential information required by the pods. Do remember that, It is not suggested to store the confidential or secret information in the ConfigMaps.

Let us see how to create a configmap using both imperative way and with the yaml file.

$kubectl create configmap <config map name> --from-literal=<keyname>=<value>

Common Mistakes:
kubectl create configmap demo-configmap --from-literal=mykey:myvalue
error: invalid literal source mykey:myvalue, expected key=value

Remember: You should use the = instead of :
Remember: You can not use spaces in the key

Finally, the regular expression that used to validate the configmap key is '[-._a-zA-Z0-9]+')
So, Do remember that you always give a valid key while creating the configmap in Kubernetes.

How to create multiple key, value pairs using imperative command? 

$kubectl create configmap <config map name> --from-literal=<keyname1>=<value1> --from-literal=<keyname2>=<value2> ...

Let us see, how does our demo-configmap looks in a YAML format

$ kubectl get configmap demo-configmap -o yaml
apiVersion: v1
data:
  keyname1: value1
  keyname2: value2
kind: ConfigMap
metadata:
  creationTimestamp: "2020-12-06T02:27:44Z"
  name: demo-configmap
  namespace: default
  resourceVersion: "702943"
  selfLink: /api/v1/namespaces/default/configmaps/demo-configmap
  uid: c6d1cee8-7707-4e05-95e9-e848ce65f631

Now, the next step is on how to create configmap from file (also provide json file)
$kubectl create configmap file-config --from-file=config-from-file.conf

You can also, provide multiple files to create the configmap or a directory as shown below.

$kubectl create configmap file-config --from-file=config-from-file1.conf --from-file=config-from-file2.conf

$kubectl create configmap file-config --from-file=<directory path>

Check the file, that you have created. if you do not provide the config file name, then it will take the file name as config name.
$ kubectl get configmap file-config -o yaml

apiVersion: v1
data:
  config-from-file.conf: |+
    KEY1=VALUE1
    KEY2=VALUE2
kind: ConfigMap
metadata:
  name: file-config
  namespace: default

