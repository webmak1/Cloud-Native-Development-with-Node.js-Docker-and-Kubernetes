# [Linkedin] Cloud Native Development with Node.js, Docker, and Kubernetes

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

---

**Marley**

<a href="https://jsdev.org">jsdev.org</a>

Any questions on eng: <a href="https://jsdev.org/chat/">Telegram or Slack</a>  
Любые вопросы на русском: <a href="https://jsdev.ru/chat/">Telegram or Slack</a>
