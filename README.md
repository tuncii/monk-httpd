# Monk & apache

This repository contains Monk.io template to deploy apache system either locally or on cloud of your choice (AWS, GCP, Azure, Digital Ocean).


## Start

Set up Monk - https://docs.monk.io/docs/monk-in-10/

Start `monkd` and login.

```bash
monk login --email=<email> --password=<password>
```

## Prerequisites

Install Monk - https://monk-docs-preprod.firebaseapp.com/docs/get-monk

Add Cloud Provider - https://monk-docs-preprod.firebaseapp.com/docs/cloud-provider

Run Kits in a Cluster - https://monk-docs-preprod.firebaseapp.com/docs/lifecycle/running-templates-cluster

### Monk Start & Run

Please first login to `monkd`

```bash
❯ monk login --email=<email> --password=<password>
```

### Clone Repository

```bash
❯  git clone git@github.com:tuncii/monk-httpd.git
```

### Load Template

```bash
❯  cd monk-httpd
❯  monk load manifest.yaml
```

### Verify

```bash
❯ monk list -l apache
✔ Got the list
Type      Template       Repository  Version  Tags
runnable  apache/apache  local       -        self hosted, http server, reverse proxy
group     apache/stack   local       -        -
```
## Deploy a simple httpd

```bash
❯ monk run apache/stack
✔ Starting the job: local/apache/stack... DONE
✔ Preparing nodes DONE
✔ Checking/pulling images...
✔ [================================================] 100% bitnami/apache:2.4.54-debian-11-r55 local
✔ Checking/pulling images DONE
✔ Starting containers DONE
✔ Started local/apache/stack

🔩 templates/local/apache/stack
 └─🧊 Peer local
    └─🔩 templates/local/apache/apache
       └─📦 local-80c8abf1c5cf0f37a1f2c815c2-local-apache-apache-apache
          ├─🧩 bitnami/apache:2.4.54-debian-11-r55
          ├─💾 /hostroot/Users/tunccelik/.monk/volumes/apache/data -> /app
          ├─🔌 open 31.155.19.191:8080 (0.0.0.0:8080) -> 8080
          └─🔌 open 31.155.19.191:8443 (0.0.0.0:8443) -> 8443

💡 You can inspect and manage your above stack with these commands:
	monk logs (-f) local/apache/stack - Inspect logs
	monk shell     local/apache/stack - Connect to the container's shell
	monk do        local/apache/stack/action_name - Run defined action (if exists)
💡 Check monk help for more!
```

### Check if it works

```bash
❯ curl 31.155.19.191:8080
<html><body><h1>It works!</h1></body></html>
```
### Check the logs
```bash
? Select: templates/local/apache/stack
? Select runnable: templates/local/apache/apache
apache 19:00:05.75
apache 19:00:05.75 Welcome to the Bitnami apache container
apache 19:00:05.76 Subscribe to project updates by watching https://github.com/bitnami/containers
apache 19:00:05.76 Submit issues and feature requests at https://github.com/bitnami/containers/issues
apache 19:00:05.76
apache 19:00:05.76 INFO  ==> ** Starting Apache setup **
apache 19:00:05.77 WARN  ==> The Apache configuration file '/opt/bitnami/apache/conf/httpd.conf' is not writable. Configurations based on environment variables will not be applied.
apache 19:00:05.78 INFO  ==> Found mounted virtual hosts in '/vhosts'. Copying them to '/opt/bitnami/apache/conf/vhosts'
realpath: /bitnami/apache/conf: No such file or directory
apache 19:00:05.80 INFO  ==> Configuring the HTTP port
apache 19:00:05.80 INFO  ==> Configuring the HTTPS port
apache 19:00:05.80 INFO  ==> Configuring Apache ServerTokens directive

apache 19:00:05.81 INFO  ==> ** Apache setup finished! **
apache 19:00:05.82 INFO  ==> ** Starting Apache **
[Tue Dec 20 19:00:05.844579 2022] [ssl:warn] [pid 1] AH01909: www.example.com:8443:0 server certificate does NOT include an ID which matches the server name
[Tue Dec 20 19:00:05.849252 2022] [ssl:warn] [pid 1] AH01909: www.example.com:8443:0 server certificate does NOT include an ID which matches the server name
[Tue Dec 20 19:00:05.850541 2022] [mpm_prefork:notice] [pid 1] AH00163: Apache/2.4.54 (Unix) OpenSSL/1.1.1n configured -- resuming normal operations
[Tue Dec 20 19:00:05.850683 2022] [core:notice] [pid 1] AH00094: Command line: '/opt/bitnami/apache/bin/httpd -f /opt/bitnami/apache/conf/httpd.conf -D FOREGROUND'
192.168.127.1 - - [20/Dec/2022:19:02:40 +0000] "GET / HTTP/1.1" 200 45
192.168.127.1 - - [20/Dec/2022:19:02:54 +0000] "GET / HTTP/1.1" 200 45
192.168.127.1 - - [20/Dec/2022:19:02:55 +0000] "GET /favicon.ico HTTP/1.1" 404 196
192.168.127.1 - - [20/Dec/2022:19:03:46 +0000] "-" 408 -
```

## Configuration

You can add/remove configuration of the template.

The current variables can be found in `stack.yaml/variables` section

```yaml
  variables:
    apache-image-tag: "latest"
```

### apache configuration file

You can find configuration file (apache.xml) in `/files` directory in repository and can edit before the running kit.


| Configuration File	 | Format Used | Directory in Container | Purpose 
|----------|-------------|------|---------|
| **apache.xml** | XML | `/opt/apache/server/apache/apache.xml` | The apache.xml file defines some global configuration options that apply to all or many cores.


##  Template variables

| Variable | Description | Type | Example |
|----------|-------------|------|---------|
| **apache-image-tag** | apache image version. | string | latest |





### Cloud Deployment

```bash
❯ monk cluster provider add
? Cloud provider aws
AWS config /Users/tunccelik/.aws/credentials detected, importing "default" credentials.
✔ Provider added successfully
❯ monk cluster grow -p  aws
? Name of the new instance httpd
? Tags (split by whitespace) httpd
? Region us-east-2
? Instance type t3.large
? Number of instances (or press ENTER to use default = 1) 2
? Default disk type for aws is HDD (standard). Would you like to change it? Yes
? Disk type SSD (gp3)
? Disk size (or press ENTER to use default = 250 GBs) 50
✔ Start creation of new instance(s) on aws... DONE
✔ Creating node: httpd-1 DONE
✔ Initializing node: httpd-1 DONE
✔ Creating node: httpd-2 DONE
✔ Initializing node: httpd-2 DONE
✔ Connecting: httpd-1 DONE
✔ Syncing peer: httpd-1 DONE
✔ Connecting: httpd-2 DONE
✔ Syncing peer: httpd-2 DONE
✔ Syncing nodes DONE
✔ Cluster grown successfully
```
## Stop and clean workload

```bash
❯ monk purge
✔ Got the list
? Select template: apache/stack
WARNING! This will remove:
Templates:
 - apache/stack

Runnables:
  - running-group templates/local/apache/stack:
	  - running-runnable templates/local/apache/apache:
			Containers:
			 - local-80c8abf1c5cf0f37a1f2c815c2-local-apache-apache-apache
? Purge? Yes
✔ apache/stack purged
```

```bash
cloud-nuke aws
```