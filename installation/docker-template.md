---
description: Learn more about using the Docker Template
---

# Docker Template

## Introduction

We don't teach you the installation about docker. Please read the Docker Docs specific for your environment.

### Step 1

Install the template with [degit](https://www.npmjs.com/package/degit). This tool helps you to clone the starter template

```bash
npx degit abstractFlo/atlas-starter#docker gamemode
```

### Step 2

Install the dependencies

```bash
cd gamemode
npm install
```

### Step 3

Set up the required environment variables

{% hint style="warning" %}
Rename **docker.compose.example.yml** to **docker-compose.ym**l and change this file to fit your needs.
{% endhint %}

{% tabs %}
{% tab title=".env" %}
```bash
# Rename .env.example to .env

# Change variables inside .env to your needs

BUILD_DIR_RESOURCE=dist_resources
BUILD_DIR_RETAIL=dist_retail
DOCKER_PROJECT_PATH=./
SERVER_BRANCH=rc
```
{% endtab %}

{% tab title="retail/config/server.cfg" %}
```bash
# rename _server.example.cfg to server.cfg

# Change any key to fit your needs
name : 'Atlas Dev Server'
host : 0.0.0.0
port : 7788
players : 10
#password: cometest
announce : false
#token: yourToken
gamemode : anything
website : example.com
language : en
description : 'DevServer - Play with alt:v and atlas Framework'
modules : [ js-module ]
debug : true
resources : [
    gamemode # setup the name of your resource
]
```
{% endtab %}

{% tab title="retail/config/environment.json" %}
```bash
# rename _environment.example.json to environment.json

# Change any key you want to fit your needs
{
  "database": {
    "name": "default",
    "type": "mysql",
    "host": "altv-mysql",
    "port": 3306,
    "username": "gamemode",
    "password": "demo123",
    "database": "gamemode",
    "synchronize": true,
    "logging": false
  },
  "discord": {
    "client_id": "",
    "client_secret": "",
    "bot_secret": "",
    "server_id": "",
    "redirect_url": "http://127.0.0.1:1337/auth/discord",
    "auth_url": "https://discord.com/api/oauth2/authorize",
    "auth_token_url": "https://discord.com/api/oauth2/token",
    "user_me_url": "https://discord.com/api/users/@me"
  }
}
```
{% endtab %}
{% endtabs %}

### Step 4

Start the watcher or build and boot up docker. Keep in mind that your terminals need to point to the root directory.

{% hint style="warning" %}
Keep in mind retail folder is only copied at build command. You must be run this command initial.
{% endhint %}

{% tabs %}
{% tab title="Terminal 1" %}
```bash
# If you want to rebuild after changes
npm run watch

# If you want to build the files
npm run build
```
{% endtab %}

{% tab title="Terminal 2" %}
```bash
# Inside the second terminal, bootup docker
docker-compose up

# If you want to detach the process
docker-compose up -d
```
{% endtab %}

{% tab title="Terminal 3" %}
```bash
# connect to your server container and start the server if you want
docker exec -it altv bash
./start_server.sh
```
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
There is no need to stop the docker process everytime. You can stop the server inside the container and restart it at any time you make changes in your script.
{% endhint %}

{% hint style="success" %}
Congratulation, you have successfully set up your environment. Now it's time to create your awesome gamemode.
{% endhint %}

## Explain Folder Structure

To help you understand how the whole system works, we will explain the structure and configuration options here. The template contains a good starting point to teach yourself.

{% hint style="warning" %}
There are two folders for your gamemode creation.
{% endhint %}

### **Resources**

This folder will be bundled by Rollup to ES6 on Server/Client side. Inside the directory you can create folders as many as you want. Your builded resources lives inside **BUILD\_DIR\_RESOURCE**.

#### package.json

You can create a package.json file inside your folder. This is only needed one time to tell rollup which folder is a resource and which not.

{% tabs %}
{% tab title="resources/gamemode/package.json" %}
```bash
{
  # the name for your resource
  "name": "gamemode",

  # set true, rollup will bundle it to BUILD_DIR with given name
  "isGameResource": true,

  # If you get some errors while bundle up some CJS modules
  # you can define it as external and convertedModules
  # in most cases it would be done automatic
  "server": {
    "external": [],
    "convertedModules": []
  },

  # If you get some errors while bundle up some CJS modules
  # you can define it as external and convertedModules
  # in most cases it would be done automatic
  "client": {
    "external": [],
    "convertedModules": []
  }
}
```
{% endtab %}
{% endtabs %}

You're done! In most cases there is no need to create multiple gamemode resources. You can create multiple folders inside resources and use them as shared scripts. You can import the scripts at any place inside the resource folder.

```bash
- resources
  - gamemode
    - assets
      - resource.cfg
    + server
    + client
    + shared
    - package.json
  - feature_1
    + server
    + client
    + shared
  - feature_2
    + server
    + client
    + shared
  + shared
  ...
```

{% hint style="warning" %}
Keep in mind, if you set up a package.json, you need the assets folder as well to set up the resource.cfg
{% endhint %}

### **Retail**

This folder contains all your ready to use resources like maps, cars, weapons and so on. The build process will copy all this files inside your **BUILD\_DIR\_RETAIL** and _\*\*_respects your created folder structure inside.  
You can prevent some resources for copying by adding an underscore prefix.

```bash
+ yourMap // copy to resources/yourMap
+ yourCar // copy to resources/yourCar1
+ yourWeapon // copy to resources/yourWeapon
+ _notCopied // prevent copy
```

