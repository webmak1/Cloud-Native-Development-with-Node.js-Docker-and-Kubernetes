# [Linkedin] Cloud Native Development with Node.js, Docker, and Kubernetes

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

### Helm

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

## 04. Add support for health

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
<br/>

---

**Marley**

<a href="https://jsdev.org">jsdev.org</a>

Any questions on eng: <a href="https://jsdev.org/chat/">Telegram or Slack</a>  
Любые вопросы на русском: <a href="https://jsdev.ru/chat/">Telegram or Slack</a>
