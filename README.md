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

---

**Marley**

<a href="https://jsdev.org">jsdev.org</a>

Any questions on eng: <a href="https://jsdev.org/chat/">Telegram or Slack</a>  
Любые вопросы на русском: <a href="https://jsdev.ru/chat/">Telegram or Slack</a>
