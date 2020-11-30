---
layout: post
title: Kubernetes - ConfigMaps
date: 2020-11-19
related_image: /assets/images/2020-11-26.png
---

<div class="view overlay">
	<img class="card-img-top" src="{{ page.related_image }}" alt="Card image cap">
    <a href="#!">
        <div class="mask rgba-white-slight"></div>
    </a>
</div>

ConfigMaps in kubernetes are a flexible way of storing the non confidential information in key value pairs, that are needed by the pods during runtime. If you are a Kubernetes Developer, you have already aware of the configmaps and how to use them in the pod definition yaml files. This article is about to share the information what If we configured the configmaps in a wrong way or what are runtime errors we may come across when accessing the configmap values.

Now, let us start with creating the configmap using the kubectl command [its called the imperative way of creating resources in kuberntes]

kubectl create configmap name-of-config --from-literal=keyname=value --from-literal=keyname=value ...

kubectl create configmap name-of-config --from-file=file-path-here.properties

You can reference the confimap in one of the below ways depending on your requirement

# Reference from the env
envFrom:
  - configMapRef:
      name: app-config

# Reference from Single env
env:
  - name: APP_COLOR
    valueFrom:
      configMapKeyRef:
        name: app-config
        key: APP_COLOR

# Reference from a volume
volumes:
- name: app-config-volume
  configMap:
    name: app-config

1. When you create the configmap in an imperative way, if you make a mistake as below

kubectl create configmap webapp-config-mapp --from-literal APP_COLOR:darkblue

Then, it will throw an error "error: invalid literal source APP_COLOR:darkblue, expected key=value"

2. Create a configmap without a name, 
	
	kubectl create configmap --from-literal=APP_COLOR=green

	then it will throw the below error
	
	error: exactly one NAME is required, got 0


