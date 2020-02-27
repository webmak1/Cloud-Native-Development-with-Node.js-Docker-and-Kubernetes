# [Linkedin] Cloud Native Development with Node.js, Docker, and Kubernetes

https://www.lynda.com/Node-js-tutorials/Cloud-Native-Development-Node-js-Docker-Kubernetes/808675-2.html

http://cloudnativejs.io/

<br/>

## 02. Build application

<br/>

### 01. Build your Node.js app

    $ npm install -g express-generator
    $ express myapp
    $ cd myapp
    $ npm install
    $ npm start

http://localhost:3000/

<br/>

### 02. Add a Dockerfile

    $ docker build -t nodeserver -f Dockerfile .

    $ docker run -i -p 3000:3000 -t nodeserver

<br/>

### 03. Create a Dockerfile dev and debug

    $ docker build -t nodeserver-tools -f Dockerfile-tools .

<br/>

    $ docker run -i -v "$PWD"/myapp/package.json:/tmp/package.json -v "$PWD"/myapp/node_modules_linux:/tmp/node_modules -w /tmp -t node:10 npm install

<br/>

    $ docker run -i -p 3000:3000 -v "$PWD"/myapp/:/app -v "$PWD"/myapp/node_modules_linux:/app/node_modules -t nodeserver-tools /bin/run-dev


<br/>

    $ docker run -i --expose 9229 -p 9229:9229 -p 3000:3000 -v "$PWD"/myapp/:/app -v "$PWD"/myapp/node_modules_linux:/app/node_modules -t nodeserver-tools /bin/run-debug


<br/>

### 04. Making a product

    $ docker build -t nodeserver-run -f Dockerfile-run .
    $ docker run -i -p 3000:3000 -t nodeserver-run

<br/>

### 05. Version labeling and control

    $ docker tag nodeserver-run nodeserver:1.0.0

    $ docker tag nodeserver-run webmakaka/nodeserver:1.0.0

    $ docker login
    $ docker push webmakaka/nodeserver:1.0.0

    $ docker rmi webmakaka/nodeserver:1.0.0

    $ docker run -i -p 3000:3000 webmakaka/nodeserver:1.0.0


<br/>

## 03. Deployment on Kubernetes

<br/>

**install helm 2x**

    $ helm init

    $ helm repo list

<br/>

hub.helm.sh

<br/>

    $ git clone https://github.com/CloudNativeJS/helm.git

    $ cd helm

    $ cp -r chart/ <project_folder>

<br/>

    $ vi values

<br/>

    repository: webmakaka/nodeserver


<br/>

    $ cd <project_folder>

    $ helm install --name nodeserver chart/nodeserver

    $ helm status nodeserver

localhost:32278  
OK

<br/>

    $ helm upgrade --name nodeserver chart/nodeserver

    $ helm history nodeserver

    $ helm rollback nodeserver 1

    $ helm delete --purge nodeserver

<br/>

## 04. Adding Support for Health

    $ cd myapp
    $ npm install --save @cloudnative/health-connect

    $ npm run start

 http://localhost:3000/live

 http://localhost:3000/ready

<br/>

    $ docker build -t nodeserver-run -f Dockerfile-run .
    $ docker tag nodeserver-run webmakaka/nodeserver:1.1.0

<br/>

    $ docker login
    $ docker push webmakaka/nodeserver:1.1.0

<br/>

    $ helm upgrade --install nodeserver chart/nodeserver

    $ helm status nodeserver


http://192.168.99.166:30973/ready/

<br/>

## 05. Add Support for Metrics

<br/>

### 19 - Introduction to Prometheus

    $ cd myapp
    $ npm install --save appmetrics-prometheus

<br/>

http://localhost:3000/metrics


<br/>

    $ docker build -t nodeserver-run -f Dockerfile-run .
    $ docker tag nodeserver-run webmakaka/nodeserver:1.2.0

<br/>

    $ docker login
    $ docker push webmakaka/nodeserver:1.2.0

<br/>

    $ helm upgrade --install nodeserver chart/nodeserver

    $ helm status nodeserver

<br/>

http://192.168.99.166:30973/metrics

<br/>

### 20 - Deploy Prometheus to Kubernetes

    $ helm install --name prometheus stable/prometheus --namespace prometheus

    $ helm status prometheus

    $ export POD_NAME=$(kubectl get pods --namespace prometheus -l "app=prometheus,component=server" -o jsonpath="{.items[0].metadata.name}")

    $ kubectl --namespace prometheus port-forward $POD_NAME 9090


localhost:9090

http://localhost:9090/targets

<br/>

![Application](../img/pic-01.png?raw=true)

<br/>

http://localhost:9090/graph


os_cpu_used_ratio

http_request_duration_microseconds

<br/>

![Application](../img/pic-02.png?raw=true)


<br/>

### 21 - Deploy Grafana to Kubernetes

    $ helm install --name grafana stable/grafana --namespace grafana --set adminPassword=PASSWORD

    $ helm status grafana

    $ export POD_NAME=$(kubectl get pods --namespace grafana -l "app=grafana,release=grafana" -o jsonpath="{.items[0].metadata.name}")

    $ kubectl --namespace grafana port-forward $POD_NAME 3000

http://localhost:3000/

admin/PASSWORD


Add data source -->

Url http://prometheus-server.prometheus.svc.cluster.local

Dashboard 

import --> 1621

Options:  
Prometheus --> Prometheus --> Import

<br/>

![Application](../img/pic-03.png?raw=true)

<br/>
<br/>

---

**Marley**

<a href="https://jsdev.org">jsdev.org</a>

Any questions on eng: <a href="https://jsdev.org/chat/">Telegram or Slack</a>  
Любые вопросы на русском: <a href="https://jsdev.ru/chat/">Telegram or Slack</a>
