# Open CMD and connect through SSH
```
$ ssh -i "C:\Users\Eldest Pasirula\.ssh\delafteruse" root@128.199.177.185

$ apt update -y && apt upgrade -y
$ apt install curl git -y
```

## Get ghost configuration from git
```
$ rm -rf ghost; git clone https://github.com/aknutman/ghost-cms ghost

$ cd ghost
```

## Check this file
- docker-compose.yml
- nginx/default.conf
- config.production.json

And modify data of
- domain >> idecomp.com
- MySql Server Password

## Install certbot to manage certificate & Install cert from letsencrypt
```
$ sudo apt install software-properties-common -y
$ sudo add-apt-repository ppa:certbot/certbot -y
$ sudo apt update -y
$ sudo apt install certbot -y
$ sudo certbot certonly --standalone -d idecomp.com
```

## Install docker
```
$ apt install apt-transport-https ca-certificates curl software-properties-common gnupg -y
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add -
$ apt-key fingerprint 0EBFCD88
$ add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" -y
$ apt update -y
$ apt-get install docker-ce docker-ce-cli containerd.io -y
```

## Install docker-compose
```
$ curl -L https://github.com/docker/compose/releases/download/1.26.2/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
$ chmod +x /usr/local/bin/docker-compose
```

## Create required folder
```
$ mkdir ./content; mkdir ./mysql; mkdir -p /usr/share/nginx/html;
```

# To create scheduller to update certificate
```
$ echo "0 23 * * * certbot certonly -n --webroot -w /usr/share/nginx/html -d idecomp.com --deploy-hook='docker exec ghost_nginx_1 nginx -s reload'" >> mycron
$ crontab mycron; rm mycron
```

# Compose the docker build
```
$ docker-compose up -d
```
