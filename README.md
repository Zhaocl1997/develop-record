开发记录文档
===========================

<h1>目录</h1>

* [一.安装与配置](#1)

   - [1.1 node](#1.1)
   - [1.2 mongoDB](#1.2)
   - [1.3 VScode](#1.3)
   - [1.4 Studio3T](#1.4)
   - [1.5 Postman](#1.5)
   - [1.6 Git](#1.6)
   - [1.7 Express](#1.7)
   - [1.8 Vue/VueCli](#1.8)
   - [1.9 Raspberry Pi](#1.9)

* [二.云主机部署](#2)

   - [2.1 node](#2.1)
   - [2.2 mongoDB](#2.2)
   - [2.3 PM2](#2.3)
   - [2.4 nginx](#2.4)

* [三.相关文档](#3)


<h1>正文</h1>

<h2 id ="1">一.安装与配置</h2>

 - <h3 id ="1.1">1.1 node</h3>

    - [安装地址](https://nodejs.org/en/)

    - 验证

    ```
    node -v

    npm -v
    ```

    - 镜像

    ```
    npm config set registry https://registry.npm.taobao.org
    ```
    
 - <h3 id ="1.2">1.2 mongoDB</h3>

    - [安装地址](https://www.mongodb.com/)

 - <h3 id ="1.3">1.3 VScode</h3>

    - [安装地址](https://code.visualstudio.com/)

    - 插件

    ```
    Chinese || Vetur || Auto Rename Tag || Improt Cost || DotENV || vscode-icons
    ```

    - 设置

    ```
    Auto save: onFocusChange || WordWrap: on
    ```

 - <h3 id ="1.4">1.4 Studio3T</h3>

    - [安装地址](https://studio3t.com/)

 - <h3 id ="1.5">1.5 Postman</h3>

    - [安装地址](https://www.getpostman.com/)

    - 配置

        - 右上角设置里配置生产(prod)和开发(dev)两种环境
        - 文件夹 -> edit -> Authorization -> Bearer Token -> {{token}}
        - Login和register的 Authorization 设置为No Auth
        - Login里配置token为postman环境变量

        ```
        if (pm.response.code === 200) {
         pm.environment.set('token', pm.response.json().data.token)
        }
        ```

 - <h3 id ="1.6">1.6 Git</h3>

    - [安装地址](https://git-scm.com)

    - 验证

    ```
    git --version
    ```

    - 问题

    提示git没找到的话，VS用户配置里加一条
    
    windows写法:  "git.path": "C:\\Program Files\\Git\\bin\\git.exe" 

    linux写法: "git.path": "C:/Program Files/Git/bin/git.exe"

    - 常用命令

    ```
    git init

    git status

    git add .

    git commit –m "Init commit"

    git config --global user.email + github邮箱地址
    ```

    - 配置SSH

        - 查看SSH

        ```
        ls -a -l ~/.ssh
        ```

        - 生成SSHKEY

        ```
        ssh-keygen -b 4096 -t rsa -C + github邮箱地址
        ```

        - 启用代理

        ```
        eval $(ssh-agent -s)
        ```

        - 注册SSH

        ```
        ssh-add ~/.ssh/id_rsa
        ```

        - pub复制粘贴到github设置里的SSH key里

        ```
        cat ~/.ssh/id_rsa.pub
        ```

        - 测试连接

        ```
        ssh –T git@github.com
        ```

 - <h3 id ="1.7">1.7 Express</h3>

    - app 中间件顺序

      - express 内嵌中间件

      - HTTP保护/压缩/日志中间件

      - 跨域请求中间件

      - session中间件

      - 路由中间件
    
      - 404/500捕获中间件


 - <h3 id ="1.8">1.8 Vue/VueCli</h3>
 - <h3 id ="1.9">1.9 Raspberry Pi</h3>

    - [BalenaEtcher](https://www.balena.io/etcher/) 写镜像

    - 默认账号和密码为pi和raspberry

    - 改密码 -> 启用摄像头 -> 启用SSH

    ```
    sudo raspi-config
    ```

    - 启用wifi

    ```
    sudo nano /etc/wpa_supplicant/wpa_supplicant.conf
    ```
    ```
    country=cn
    update_config=1
    ctrl_interface=/var/run/wpa_supplicant
    network={
        ssid="wifi名"
        psk="wifi密码"
    }
    ```

    - 设置阿里云镜像

    ```
    sudo nano /etc/apt/sources.list

    deb http://mirrors.aliyun.com/raspbian/raspbian/ buster main contrib non-free rpi
    ```
      备用 可不改
    ```
    sudo nano /etc/apt/sources.list.d/raspi.list

    deb http://mirrors.aliyun.com/ raspbian/raspbian/ buster main 
    ```

    - 安装node

    ```
    wget  https://nodejs.org/dist/latest-v10.x/node-v10.17.0-linux-armv7l.tar.gz

    tar -xzf node-v10.17.0-linux-armv7l.tar.gz

    cd node-v10.17.0-linux-armv7l/

    sudo cp -R * /usr/local/
    ```
    - 批量ping局域网IP

    ```
    for /L %i in (0,1,255) do ping -n 1 -w 60 169.254.129.%i | find "回复" >>pingall.txt
    ```

<h2 id ="2">二.云主机部署</h2>

 - <h3 id ="2.1">2.1 node</h3>

    - 安装
    ```
    curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash –

    sudo apt-get install -y nodejs
    ```
    - 验证@10.x
    ```
    node -v 

    npm -v
    ```

 - <h3 id ="2.2">2.2 mongoDB</h3>

    - 安装
        - 官网安装 4.2
        ```
        wget -qO - https://www.mongodb.org/static/pgp/server-4.2.asc | sudo apt-key add –

        echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/4.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.2.list

        sudo apt-get update

        sudo apt-get install -y mongodb-org
        ```

        - package安装 3.6.3
        ```
        sudo apt-get update

        sudo apt-get install -y mongodb
        ```

    - 验证
    ```
    sudo systemctl status mongodb
    ```


 - <h3 id ="2.3">2.3 pm2</h3>

    - 安装

    ```
    sudo npm install pm2 –g    
    ```

    - 验证@4.1.2

    ```
    pm2 --version
    ```

    - 配置
    文件名： pm2.conf.json

    ```
    {
    "apps": {
        "name": "pi-camera", 
        "script": "index.js", 
        "watch": true, 
        "ignore_watch": [ 
            "node_modules",
            "logs"
        ],
        "instances": 1, 
        "error_file": "logs/err.log", 
        "out_file": "logs/out.log", 
        "log_date_format": "YYYY-MM-DD HH:mm:ss"
        }   
    }
    ```

    - 命令

    ```
    sudo pm2 start index.js

    sudo pm2 list

    sudo pm2 stop 0
    ```

 - <h3 id ="2.4">2.4 nginx</h3>

    - 安装

    ```
    echo "deb http://nginx.org/packages/ubuntu `lsb_release -cs` nginx" | sudo tee /etc/apt/sources.list.d/nginx.list

    wget -qO - https://nginx.org/keys/nginx_signing.key | sudo apt-key add -

    sudo apt-key fingerprint ABF5BD827BD9BF62

    sudo apt update

    sudo apt install nginx
    ```

    - 验证@1.16.1

    ```
    nginx -v nginx
    ```

    - 语言校验

    ```
    nginx -t

    service nginx restart
    ```

    - 配置

        - 权限修改

        ```
        sudo nano /etc/nginx/nginx.conf

        user nginx ->user root
        ```

        - 配置和代理

        ```
        cd /etc/nginx/conf.d

        cp default.conf 地址.conf
        ```

        - 阿里云下载证书配置地址HTTPS(HTTP自动跳转至HTTPS)

        ```
        server {
            listen 80;
            server_name 地址;
            rewrite ^(.*) https://$server_name$1 permanent;
         }

        server {
            listen 443  ssl;
            server_name  地址;

            ssl_certificate   /root/前台文件夹名/cert/.pem;
            ssl_certificate_key  /root/前台文件夹名/cert/.key;
            ssl_session_timeout 5m;
            ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
            ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
            ssl_prefer_server_ciphers on;

            #charset koi8-r;
            access_log  /var/log/nginx/地址.access.log  main;

            location / {
                root   /root/前台文件夹名;
                index  index.html index.htm;
                try_files $uri $uri/ /index.html;
            }
         }
        ```

        - 反向代理api

        ```
        server {
            listen       80;
            listen       443  ssl;
            server_name  api地址;

            ssl_certificate   /root/api文件夹名/cert/.pem;
            ssl_certificate_key  /root/api文件夹名/cert/.key;
            ssl_session_timeout 5m;
            ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
            ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
            ssl_prefer_server_ciphers on;

            location / {
                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header X-Forwarded-For   $proxy_add_x_forwarded_for;
                proxy_set_header Host $http_host;
                proxy_set_header X-Nginx-Proxy true;
                proxy_http_version 1.1;
                proxy_set_header Connection "";
                proxy_pass http://localhost:3000;
            }
        }
        ```