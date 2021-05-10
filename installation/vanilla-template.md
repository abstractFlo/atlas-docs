---
description: Learn more about the Vanilla Template
---

# Vanilla Template

## Introduction

If you want to work without docker this template is perfect for you. It bundles all required files in one folder. The only thing you need to do is copy and paste the folder to your server and run `npm install`

{% hint style="warning" %}
This template only provides the build process for your gamemode. You need to download the server files from [alt:V](https://altv.mp/#/downloads) yourself.
{% endhint %}

### Step 1

Install the template with [degit](https://www.npmjs.com/package/degit). This tool helps you to clone the starter template

```bash
npx degit abstractFlo/atlas-starter#vanilla gamemode
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
Rename **retail/server.example.cfg** folder to **retail/server.cfg** and **.env.example** to **.env** as well.
{% endhint %}

{% tabs %}
{% tab title=".env" %}
```bash
# Describe which branch you want to use for serverfiles only needed if 
# you use docker, otherwise it is ignored
SERVER_BRANCH=rc

# Where to store the builded files
ATLAS_BUILD_OUPUT=dist

# Clear the build dir before new build is triggered
# only for build not for watch
ATLAS_CLEAR_BEFORE_BUILD=true

# Define which files would be preserve for clean up
# Base server files already preserved
ATLAS_CLEAR_PRESERVE=null

# Path to this project, only needed if you using the docker starter
# Otherwise it is ignored
ATLAS_PROJECT_PATH=.

# Set atlas to production, this means the build files would be minimized
ATLAS_PRODUCTION=true

# Describe the folder where your resources live for building
ATLAS_RESOURCE_FOLDER=resources

# Describe where your static resources live
ATLAS_RETAIL_FOLDER=retail

# Setup your database stuff
DB_CONNECTION=mysql
DB_HOST=altv-mysql
DB_USER=gamemode
DB_PORT=3306
DB_PASS=demo123
DB_DATABASE=gamemode
DB_SYNCRONIZE=true
DB_LOGGING=false

# Setup all variables to your needs to start api-server
DISCORD_API_PORT=null
DISCORD_CLIENT_ID=null
DISCORD_CLIENT_SECRET=null
DISCORD_REDIRECT_URL=http://127.0.0.1:1337

# Setup the discord bot stuff
DISCORD_BOT_SECRET=null
DISCORD_SERVER_ID=null
```
{% endtab %}

{% tab title="retail/server.cfg" %}
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
{% endtabs %}

### Step 4

Start the watcher or build

{% tabs %}
{% tab title="Terminal 1" %}
```bash
# If you want to rebuild after changes
npm run watch

# If you want to build the files
npm run build
```
{% endtab %}
{% endtabs %}

{% hint style="success" %}
Congratulation! You have successfully set up your environment. Now it's time to create your awesome gamemode.
{% endhint %}

## Explain Folder Structure

To help you understand how the whole system works, we will explain the structure and configuration options here. The template contains a good starting point to teach yourself.

{% hint style="warning" %}
There are two folders for your gamemode creation.
{% endhint %}

### **Resources**

This folder will be bundled by Rollup to ES6 on Server/Client side. Inside the directory you can create folders as many as you want.

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

  # Define some externals if you want
  # this is most time not needed
  "external": [],

  # Define all modules they would converted to default import
  # this is most time not needed
  "convert": []
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

This folder contains all your ready to use resources like maps, cars, weapons and so on. The build process will copy all this files inside your **BUILD\_DIR\_RETAIL** and respects your created folder structure inside. You can prevent resources from being copied by adding an underscore prefix.

```bash
+ yourMap // copy to resources/yourMap
+ yourCar // copy to resources/yourCar1
+ yourWeapon // copy to resources/yourWeapon
+ _notCopied // prevent copy
```

