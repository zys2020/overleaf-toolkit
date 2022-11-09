# Quick-Start Guide

## Prerequisites

The Overleaf Toolkit depends on the following programs:

- bash
- docker
- docker-compose

We recommend that you install the most recent version of docker and docker-compose that 
are available on your system.
```
$ yum install bash docker docker-compose -y
```

## Install

First, let's clone this git repository to your machine:

```sh
$ git clone git@github.com:zys2020/overleaf-toolkit.git ./overleaf-toolkit
```

Next let's move into this directory:

```sh
$ cd ./overleaf-toolkit
```

For the rest of this guide, we will assume that you will run all subsequent commands from this directory.


## Take a Look Around

Let's take a look at the structure of the repository:

```sh
$ ls -l
```

Which will print something like this:

```
    bin
    CHANGELOG.md
    config
    data
    doc
    lib
    LICENSE
    README.md
```

The `README.md` file contains some useful information about the project, while the `doc` directory contains all of the documentation you will need to use the toolkit. The `config` directory will contain your own local configuration files (which we will create in just a moment), while the `bin` directory contains a collection of scripts that manage your overleaf instance.


## Initialize Configuration


Let's create our local configuration, by running `bin/init`:

```sh
$ bin/init
```

Now check the contents of the `config/` directory

```sh
$ ls config
overleaf.rc     variables.env     version
```

These are the three configuration files you will interact with:

- `overleaf.rc` : the main top-level configuration file
- `variables.env` : environment variables loaded into the docker container
- `version` : the version of the docker images to use

## **Modify Configuration**

Some default values in the original project conflicting with the deployment host server have to be modified into other feasible values. Therefore, files in the [directory](../lib/config-seed/) are modified as follows
```txt
#### config/overleaf.rc ####
SHARELATEX_LISTEN_IP=0.0.0.0  # open for all host
SHARELATEX_PORT=12000  # free port 
```

**The process of installation takes a long time.**
```sh
#### docker-compose.overleaf.yml ####
# install all kinds of packages used in the academic writing in the TeXLive 
command: tlmgr install scheme-full
```
### **Integrated Docker-Compose File**
- The `docker-compose.overleaf.yml` integrated with `docker-compose.mongo.yml, docker-compose.redis.yml, docker-compose.nginx.yml, docker-compose.base.yml` except `docker-compose.sibling.yml` is utilized to start the local overleaf project.
- refer to: `https://yeasy.gitbook.io/docker_practice/compose`

```sh
#### The latest version ####
cd lib
docker-compose -p overleaf -f docker-compose.overleaf.yml up  # front end for debugging
docker-compose -p overleaf -f docker-compose.overleaf.yml up -d  # backward end for deploying 

#### The old version ####
docker-compose -p overleaf -f docker-compose.base.yml -f docker-compose.redis.yml -f docker-compose.mongo.yml -f docker-compose.sibling-containers.yml -f docker-compose.nginx.yml up
```
### **Deployment**

```sh
git clone git@github.com:zys2020/overleaf-toolkit.git ./overleaf-toolkit
cd overleaf-toolkit/lib
docker-compose -p overleaf -f docker-compose.overleaf.yml up -d  # backward end 
```

## Starting Up

The Overleaf Toolkit uses `docker-compose` to manage the overleaf docker containers. The toolkit provides a set of scripts which wrap `docker-compose`, and take care of most of the details for you.

Let's start the docker services:

```sh
$ bin/up
```

You should see some log output from the docker containers, indicating that the containers are running. 
If you press `CTRL-C` at the terminal, the services will shut down. You can start them up again (without attaching to the log output) by running `bin/start`. More generally, you can run `bin/docker-compose` to control the `docker-compose` system directly, if you find that the convenience scripts don't cover your use-case.


## Create the first admin account

In a browser, open <http://localhost/launchpad>. You should see a form with email and password fields.
Fill these in with the credentials you want to use as the admin account, then click "Register".

Then click the link to go to the login page (<http://localhost/login>). Enter the credentials.
Once you are logged in, you will be taken to a welcome page.

Click the green button at the bottom of the page to start using Overleaf. 


## Create your first project

On the <http://localhost/project> page, you will see a button prompting you to create your first
project. Click the button and follow the instructions.

You should then be taken to the new project, where you will see a text editor and a PDF preview.


## (Optional) Check the logs

Let's look at the logs inside the container:


```sh
$ bin/logs -f web
```


You can also look at the logs for multiple services at once:

```sh
$ bin/logs -f filestore docstore web clsi
```

## TLS Proxy

The Overleaf Toolkit includes optional configuration to run an NGINX proxy, which presents Server Pro over HTTPS. Initial configuration can be generated by running
```
bin/init --tls
```
This creates minimal NGINX config in `config/nginx/nginx.conf` and a sample TLS certificate and private key in `config/nginx/certs/overleaf_certificate.pem` and `config/nginx/certs/overleaf_key.pem` respectively. If you already have a signed TLS certificate for use with Server Pro, replace the sample key and certificate with your key and certificate.

 In order to run the proxy, change the value of the `NGINX_ENABLED` variable in `config/overleaf.rc` from `false` to `true` and re-run `bin/up`.

 Further information about the TLS proxy can be found in the [docs](tls-proxy.md).


## Consulting the Doctor

The Overleaf Toolkit comes with a handy tool for debugging your installation: `bin/doctor`

Let's run the `bin/doctor` script:

```sh
$ bin/doctor
```

We should see some output similar to this:

```
====== Overleaf Doctor ======
- Host Information
    - Linux
    ...
- Dependencies
    - bash
        - status: present
        - version info: 5.0.17(1)-release
    - docker
        - status: present
        - version info: Docker version 19.03.6, build 369ce74a3c
    - docker-compose
        - status: present
        - version info: docker-compose version 1.24.0, build 0aa59064
    ...
====== Configuration ======
    ...
====== Warnings ======
- None, all good
====== End ======
```

First, we see some information about the host system (the machine that the toolkit is being run on), then some information about dependencies. If any dependencies are missing, we will see a warning here. Next, the doctor checks our local configuration. At the end, the doctor will print out some warnings, if any problems were encountered.

When you run into problems with your toolkit, you should first run the doctor script and check it's output. 


## Getting Help

Users of the free Community Edition should [open an issue on github](https://github.com/overleaf/toolkit/issues). 

Users of Server Pro should contact `support@overleaf.com` for assistance.

In both cases, it is a good idea to include the output of the `bin/doctor` script in your message.
