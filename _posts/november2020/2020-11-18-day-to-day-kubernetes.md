---
layout: post
title: Day to Day Kubernetes Commandline Reference
date: 2020-11-18
related_image: /assets/images/2020-11-18.jpg
---
<div class="view overlay">
	<img class="card-img-top" src="{{ page.related_image }}" alt="Card image cap">
    <a href="#!">
        <div class="mask rgba-white-slight"></div>
    </a>
</div>

There are multiple resources for the Kubernetes `kubectl` command line reference. It is very good practice to learn the usage of all those commands. But, here I would like to share the most day to day commands that we use on the terminal to take care of our daily development and debugging activities with respect to Kubernetes.

To Get the Context
<pre><code class="Bash">
# To get the current context
$ kubectl config current-context

# To Display list of contexts
$ kubectl config get-contexts

# Now, create the namespace in the cluster
$ kubectl create namespace &lt;your namespace name&gt;

# Use the namespace for the subsequent kubectl operations
$ kubectl config set-context --namespace=&lt;your namespace name&gt; --current
</code></pre>

Namespace Commands 
<pre><code class="Bash">
# Describe the namespace
$ kubectl describe namespace &lt;your namespace name&gt;

# Get the namespace details either in yaml or json
$ kubectl get namespace blog-ns -o json
$ kubectl get namespace blog-ns -o yaml

# To list all the resources under a namespace
$ kubectl get all -n &lt;your namespace name&gt; 
</code></pre>
 ConfigMap and Secretes Commands 
<pre><code class="Bash">
# Create a config map
$ kubectl create configmap &lt;name of the config map&gt; --from-literal=username=user --from-literal=hostname=host

# Create a secret
# Remember Secretes are namespace specific, they can not be accessed across the namespaces
$ kubectl create secret generic &lt;secret name&gt; --from-literal=&lt;key&gt;=&lt;value&gt;

# Whenever you store a secret, you should store them in a base64 format
$ echo -n &lt;your password&gt; | base64

# To decode the value
$ echo -n base64 value | base64 -decode
</code></pre>

Deployment and Pod Commands
<pre><code class="Bash">
# List all the pods in all namespaces
$ kubectl get pods --all-namespaces 

# Create a POD quickly
$ kubectl run &lt;pod name&gt; --image=busybox --restart=Never -n &lt;your namespace&gt;

# In some cases, you just need the pod yaml
$ kubectl run &lt;pod name&gt; --image=nginx --dry-run=client --restart=Never -n &lt;your namespace&gt; -o yaml

# Rolling Update of the deployment image
$ kubectl set image deployment/&lt;deployment name&gt; &lt;containername&gt;=&lt;new image name&gt;

# Restart the deployment
$ kubectl rollout restart deployment/&lt;deployment name&gt;

# To undo the rollout
$ kubectl rollout undo deployment/&lt;deployment name&gt;

# To know the rollout history of a deployment
$ kubectl rollout history deployment &lt;deployment name&gt;

# To view the logs
$ kubectl logs &lt;podname&gt; --namespace=&lt;namespace name&gt;

# To open the interactive shell to the pod
$ kubectl exec -it &lt;pod name&gt; --namespace=&lt;namespace name&gt; -- /bin/sh

# To check the evens on the pod
$ kubectl describe pod &lt;pod name&gt; | grep -C 10 Events:

# Extract a pod definition yaml file to local file
$ kubectl get pod &lt;podname&gt; -o yaml &gt; &lt;local pod filename&gt;.yaml
</code></pre>
Searching or Listing Commands
<pre><code class="Bash">
# List the resources in a sorted order by any of the metadata
$ kubectl get services --sort-by=.metadata.name

# Get the particular fields displayed from the resources
# If it is a list then, use names[*].abc
$ kubectl get services -A -o custom-columns=NAME:metadata.name;

# List the resources with specific label
$ kubectl get pod -l &lt;label name&gt;=&lt;label value&gt;
$ kubectl get pods --selector &lt;label name&gt;=&lt;label value&gt;

# To list all of the resources matching with the label
$ kubectl get all --selector &lt;lable name&gt;=&lt;label value&gt;
</code></pre>

All the above command reference is the typical actions we do as a Cloud Developer/Engineer in our day to day activities. But for more references the ocean like documentation available at  [here](https://kubernetes.io/docs/home/) 

<div id="disqus_thread"></div>
<script>
   /*
    *  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
    *  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables    
    */
    var disqus_config = function () {
    this.page.url = "https://www.parochi.xyz/2020/11/18/day-to-day-kubernetes.html";  // Replace PAGE_URL with your page's canonical URL variable
    this.page.identifier = "20201119"; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
    };
    
    (function() { // DON'T EDIT BELOW THIS LINE
    var d = document, s = d.createElement('script');
    s.src = 'https://parochi-xyz.disqus.com/embed.js';
    s.setAttribute('data-timestamp', +new Date());
    (d.head || d.body).appendChild(s);
    })();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>