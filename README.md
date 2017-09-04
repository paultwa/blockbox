# blockbox

perfect-age-151414|clever-basis-151506|sinuous-set-151506|precise-formula-151506|empyrean-kit-151506|splendid-tower-151506|thematic-gift-151506|fabled-buckeye-151614|my-project-5-151614|my-project-6-151614|my-project-7-151614|my-project-8-151614

广式蜜制叉烧

https://www.youtube.com/watch?v=udOnBPDkIOM

https://www.youtube.com/watch?v=nlY_fQA9C7Y&t=600s

https://www.youtube.com/watch?v=quypIiF1Z9A





https://tyk.io/blog/ip-rate-limiter-middleware/





Creating an IP-based rate-limiter with Tyk and JavaScript middleware

We’ve recently had a user ask if it was possible to have Tyk act as a rate limiter based on IP address instead of by token, this isn’t possible out of the box, but it is with our Middleware API and some simple JS So, you may ask yourself how we achieve this feat?

It’s actually really straightforward. First off, we’ll need to set up our API in a very specific way, we’ll use a file-based definition here as we’re assuming this is a simple deployment of Tyk. The way we will do this is by creating a custom JS middleware function that will run before any other Tyk processing takes place, this includes all rate limiting. The middleware will:

    Catch the request
    Extract the IP address from the header
    Create a key based on the IP address with a specific rate limit
    Inject the IP address as the authorization header for Tyk to use

This means that rate limits are on a per-IP basis, which is great, and also means that Tyk can just work through the request normally once the middleware processing is done.

We will start off by defining our API appropriately, we basically want an API that will invoke the middleware as a pre-processor, and then also ensure that any keys that are created do not last forever. The Tyk API Definition will look something like this:

    {
    "name": "Tyk Test API",
    "slug": "test-name",
    "api_id": "ip-ratelimit-api",
    "org_id": "1",
    "use_keyless": false,
    "use_oauth2": false,
    "auth": {
        "auth_header_name": "x-tyk-authorization"
    },
    "definition": {
        "location": "header",
        "key": "version"
    },
    "version_data": {
        not_versioned: true,
        versions: {
            "Default": {
                "name": "Default",
                "expires": "3011-02-02 00:00",
                "use_extended_paths": true,
                "extended_paths": {}
            }
        }
    },
    "proxy": {
        "listen_path": "/b605a6f03cc14f8b74665452c263bf19/",
        "target_url": "http://httpbin.org",
        "strip_listen_path": true
    },
    "custom_middleware": {
        "pre": [
            {
                "name": "ipRateLimiter",
                "path": "middleware/ipRateLimiter.js",
                "require_session": false
            }
        ]
    },
    "session_lifetime": 172800,
    "active": true
}

The key things of note here are the `custom_middleware` section, here we’ve defined where to find our files and what the objects are called. Next up is the `session_lifetime` value, this sets a default key lifetime for every key created on this API, in this case we’ve set it to 48 hours. Now that we are ready with the actual API, we can set up the middleware, it’s actually quite short – this is the entire IP rate limiter:

    // ---- A Middleware based IP Rate limiter -----
var ipRateLimiter = new TykJS.TykMiddleware.NewMiddleware({});

ipRateLimiter.NewProcessRequest(function(request) {
    // Get the IP address
    var thisIP = request.Headers["X-Real-Ip"][0];

    // Set auth header
    request.SetHeaders["x-tyk-authorization"] = thisIP;

    var keyDetails = {
        "allowance": 100,
        "rate": 100,
        "per": 1,
        "expires": 0,
        "quota_max": 100,
        "quota_renews": 1406121006,
        "quota_remaining": 100,
        "quota_renewal_rate": 60,
        "access_rights": {
            "ip-ratelimit-api": {
                "api_name": "Test API",
                "api_id": "ip-ratelimit-api",
                "versions": [
                    "Default"
                ]
            }
        },
        "org_id": "53ac07777cbb8c2d53000002"
    }

    TykSetKeyData(thisIP, JSON.stringify(keyDetails), 1)

    return ipRateLimiter.ReturnData(request);
});

// Ensure init with a post-declaration log message
log("IP rate limiter JS initialised");

The above code should go into a file called `ipRateLimiter.js` in the `middleware/` folder of your Tyk installation. If you start Tyk now, the IP rate limiter will be in place.

What is actually going on here? It’s exactly as we said above, we extract the IP address, here we are assuming that `”X-Real-Ip”` is a real header, since we are using NGinX to manage our domain, we have set a rule in the server block to make sure that the IP address is included as a header on each request:

location / {
    rewrite /(.*) /b605a6f03cc14f8b74665452c263bf19/$1 break;

    proxy_pass_header Server;
    proxy_set_header Host $http_host;
    proxy_redirect off;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Scheme $scheme;
    proxy_pass http://tyk;
} 

The thing of note here is `proxy_set_header X-Real-IP $remote_addr;`, this is important, as otherwise you won’t have access tot he IP address from the header. Once we have the IP address, we set this as the authorisation header for the request.

In the next section we have a template for a session object, here we are setting the rate limits manually, we could also use a policy file to do this and just attach the policy ID.

In this case we have set the rate limit to 100 requests per second with an overall quota of 100 per minute. We now use the built-in method `TykSetKeyData` to create the key, notice the last input to this function is `1`, this forces Tyk to not reset the quota for this transaction, which is the default behaviour – we don’t want Tyk re-setting the quotas and rate limiters every time the IP shows up, we just want to ensure the key is present.

When the middleware completes, Tyk processes the now-modified request as if it had an authorisation token (in this case the IP address) and the rate limits are enforced. And that’s it, this key will last for 48 hours, at which point it will be re-created. This means that your Redis instance won’t get flooded with infinite IP addresses as traffic grows. Enjoy!









https://www.digitalocean.com/community/tutorials/how-to-install-java-on-ubuntu-with-apt-get

豫剧 花木兰

沁园春 雪

颜体是由唐代书法家颜真卿  兰亭序

柳宗元 

萨士比亚


./catalina.sh:1:eval: usr/lib/jvm/java-7-openjdk-amd64/bin/java:not found

TypeError: Cannot read property 'version' of null at src/mongo/shell/utils.js:1008

cfg={_id:"esb",members:[{_id:0,host:'192.168.6.130:27017',priority:10},{_id:1,host:'192.168.6.130:27018',priority:1},{_id:2,host:'192.168.6.130:27019',arbiterOnly:true}]}
rs.reconfig(cfg,{force:true})

mongodb 命令
http://www.cnblogs.com/liyonghui/p/mongodb.html

https://docs.mongodb.com/manual/reference/mongo-shell/


mongo 部署的问题

http://stackoverflow.com/questions/39426821/error-while-creating-replica-set-mongodb

http://www.alexyu.se/content/2012/04/mongodb-quick-start-replica-sets-and-sharding

mongo  sharding 分片
https://docs.mongodb.com/manual/sharding/ 




#./mongod --dbpath=/web/webshare/mongodb-master/data --logpath=/web/webshare/mongodb-master/logs/mongodb.log --port 27017 --fork --replSet esb/192.168.6.130:27017 --maxConns=2000 --logappend --nojournal

#./mongod --dbpath=/web/webshare/mongodb-slave/data --logpath=/web/webshare/mongodb-slave/logs/mongodb.log --port 27018 --fork --replSet esb/192.168.6.130:27018 --maxConns=2000 --logappend --nojournal

#./mongod --dbpath=/web/webshare/mongodb-arbiter/data --logpath=/web/webshare/mongodb-arbiter/logs/mongodb.log --port 27019 --fork --replSet esb/192.168.6.130:27017,esb/192.168.6.130:27018 --logappend

#./mongo

#use admin

#db.runCommand({"replSetInitiate":{ "_id":"esb", "members":[ { "_id":0, "host":"192.168.6.129:27017" }, { "_id":1, "host":"192.168.6.129:27018" } ]}});

#rs.addArb("192.168.6.129:27019");


///////////////////////////////////////////////////////////////////


sudo update-alternatives --config java

sudo update-alternatives --config javac

/////////////////////////

description "Tomcat Server"

  start on runlevel [2345]
  stop on runlevel [!2345]
  respawn
  respawn limit 10 5

  setuid tomcat
  setgid tomcat

  env JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64/jre
  env CATALINA_HOME=/opt/tomcat

  # Modify these options as needed
  env JAVA_OPTS="-Djava.awt.headless=true -Djava.security.egd=file:/dev/./urandom"
  env CATALINA_OPTS="-Xms512M -Xmx1024M -server -XX:+UseParallelGC"

  exec $CATALINA_HOME/bin/catalina.sh run

  # cleanup temp directory after stop
  post-stop script
    rm -rf $CATALINA_HOME/temp/*
  end script



/////////////////////////////////////////////////////////////

https://www.digitalocean.com/community/tutorials/how-to-install-apache-tomcat-8-on-ubuntu-14-04

wget http://mirror.sdunix.com/apache/tomcat/tomcat-8/v8.0.23/bin/apache-tomcat-8.0.23.tar.gz

////////////////////////////////////////////////////////////////////

java.lang.NoClassDefFoundError: javax/websocket/Endpoint


This is your annual notice that all registered domain names must have accurate and up-to-date contact information.

Please review the domain information below and verify its accuracy. If all information is up-to-date then no changes are necessary. Inaccurate or outdated information must be corrected by logging into your account.

While we do respect your privacy, we are required by ICANN, our regulating authority, to send these notices annually to all domain contacts. To learn more about this process and why it is required, please visit ICANN’s website: http://www.icann.org/whois/wdrp-registrant-faq.htm

Please remember that under the terms of your registration agreement, providing false or inaccurate Whois (contact) information can be grounds for the cancellation of your domain name registration.

Domains names for review as of August 1 thru August 31

///////////////////////////////////////////////////////////////////////////////////////

ubuntu14.04  install tomcat7

https://www.digitalocean.com/community/tutorials/how-to-install-apache-tomcat-7-on-ubuntu-14-04-via-apt-get

https://askubuntu.com/questions/154953/specify-jdk-for-tomcat7

ttps://www.digitalocean.com/community/tutorials/how-to-optimize-your-tomcat-installation-on-ubuntu-14-04


ubuntu14.04  install jdk8

https://www.digitalocean.com/community/tutorials/how-to-install-java-on-ubuntu-with-apt-get


////////////////////////////////////////////////////////////////////////////

Credentials are incorrect, please try again   tyk dashboard login

https://github.com/TykTechnologies/tyk/issues/170

1. update the /etc/hosts/ with 127.0.0.1 my-awesome-portal.com both INSIDE and OUTSIDE the vagrant

2. sudo /opt/tyk-dashboard/install/setup.sh domain=my-awesome-portal.com

3. restart both gateway and dashboard service

4. sudo /opt/tyk-dashboard/install/bootstrap.sh my-awesome-portal.com

5. goto http:// my-awesome-portal.com:3000 and login using bootstrap user+pass



/////////////////////////////////////////////////////////////////////
paul

twa

441403879@qq.com

test123

Access Credentials d256625d897c46075f558c88102bb2cc

RPC Credentials 5976f423af9755027c166b04

/////////////////////////////////////////////////////////////////

学习Tyk API

为什么学习tyk API?

通过学习tyk API, 了解tyk API各模块的功能特点，一般学习一般用soapUI一个个测试这些API接口的功能。

更好更快地了解tyk API的功能细节。封装tyk API，我们可以定制dashboard 服务网关管理平台。

//////////////////////////////////////////////////////////////////////////////








这是一种无状态的认证机制，因为用户状态永远不会保存在服务器内存中。 服务器的受保护路由将在授权头中检查有效的JWT，如果存在，则允许用户访问受保护的资源。 由于JWT是独立的，所有必要的信息都在那里，减少了多次查询数据库的需要。

这样可以完全依赖无状态的数据API，甚至可以向下游服务发出请求。 无论哪个域用于您的API都不重要，因此，跨原始资源共享（CORS）不会是一个问题，因为它不使用Cookie。

下图显示了这个过程：

Downloading the SAP Java Connector

Before you can use the SAP Java Connector for Sterling B2B Integrator, you must first download the files for it.
About this task
Follow these steps to download the files that are needed to use the SAP Java Connector.
Procedure

    On the same computer where Sterling B2B Integrator is installed, download the free SAP JCo from the SAP Service Marketplace website at https://websmp101.sap-ag.de/.
    Log in to the SAP Service Marketplace and access the SAP JCo download software from http://service.sap.com/connectors. If necessary, select the Tools & Services page to display the download page. 




https://play.google.com/apps/publish/?dev_acc=13980069071725628395#FinanceOverviewPlace:p=com.autosoft.cloud




https://github.com/TykTechnologies/tyk/issues/170


https://community.tyk.io/t/which-is-user-password-http-my-tyk-instance-com-3000/787

/////////////////////////////////////////////////////////////////////////////////

Install Tyk Gateway on Ubuntu

Tyk has it’s own APT repositories hosted by the kind folks at packagecloud.io, which makes it easy, safe and secure to install a trusted distribution of the Tyk Gateway stack.

This tutorial will run on an Amazon AWS Ubuntu Server 14.04 LTS instance. We will install the Tyk Gateway with all dependencies stored locally.

We’re installing on a t2.micro because this is a tutorial, you’ll need more RAM and more cores for better performance.

Pre-requisites:

Ensure port 8080 is open: this is used in this guide for Gateway traffic (API traffic to be proxied)
Step 1: Set up our APT repositories

First, add our GPG key which signs our binaries:

    curl https://packagecloud.io/gpg.key | sudo apt-key add -
Run update:

    sudo apt-get update
Since our repositories are installed via HTTPS, you will need to make sure APT supports this:

    sudo apt-get install -y apt-transport-https 
Now lets add the required repos and update again (notice the -a flag in the second Tyk commands - this is important!):

    echo "deb https://packagecloud.io/tyk/tyk-gateway/ubuntu/ trusty main" | sudo tee /etc/apt/sources.list.d/tyk_tyk-gateway.list
    
    echo "deb-src https://packagecloud.io/tyk/tyk-gateway/ubuntu/ trusty main" | sudo tee -a /etc/apt/sources.list.d/tyk_tyk-gateway.list
    
    sudo apt-get update
What we’ve done here is:

Added the Tyk Gateway repository
Updated our package list
Step 2: Install the Tyk Gateway

We’re now ready to install Tyk Gateway and Tyk Dashboard, along with all the main dependencies: Redis and MongoDB. To install everything run:

    sudo apt-get install -y redis-server tyk-gateway
What we’ve done here is instructed apt-get to install Redis and the Tyk Gateway without prompting, wait for the downloads to complete.

When Tyk is finished installing, it will have installed some init scripts, but it will not be running yet. The next step will be to setup the Gateway - thankfully this can be done with three very simple commands, however it does depend on whether you are configuring Tyk Gateway for use with the Dashboard or without (Community Edition).

Configure Tyk Gateway Community Edition

You can set up the core settings for Tyk Gateway with a single setup script, however for more involved deployments, you will want to provide your own configuration file. To get things started, run:

    sudo /opt/tyk-gateway/install/setup.sh --listenport=8080 --redishost=localhost --redisport=6379 --domain=""
What we’ve done here is told the setup script that:

--listenport=8080: Listen on port 8080 for API traffic.
--redishost=localhost: Use the hostname localhost for Redis.
--redisport=6379: Use port 6379 for Redis.
--domain="": Do not filter domains for the Gateway, see the note on domains below for more about this.
In this example, we don’t want Tyk to listen on a single domain, and we can always set up custom domains at the API level in the Dashboard. It is recommended to leave the Tyk Gateway domain unbounded for flexibility and ease of deployment.

Starting Tyk

The Tyk Gateway can be started now that it is configured. Use this commannd to start the Tyk Gateway:

    sudo service tyk-gateway start
Configure Tyk Gateway with Dashboard

Prerequisites

This configuration assumes that you have already installed Tyk Dashboard, and have decided on the domain names for your Dashboard and your Portal. They must be different. For testing purposes, it is easiest to add hosts entries to your (and your servers) /etc/hosts file.

Set up Tyk

You can set up the core settings for Tyk Gateway with a single setup script, however for more involved deployments, you will want to provide your own configuration file. To get things running let’s run:

    sudo /opt/tyk-gateway/install/setup.sh --dashboard=1 --listenport=8080 --redishost=localhost --redisport=6379
What we’ve done here is told the setup script that:

--dashboard=1: We want to use the Dashboard, since Tyk Gateway gets all it’s API Definitions from the Dashboard service, as of v2.3 Tyk will auto-detect the location of the dashboard, we only need to specify that we should use this mode.
--listenport=8080: Tyk should listen on port 8080 for API traffic.
--redishost=localhost: Use Redis on the hostname: localhost.
--redisport=6379: Use the default Redis port.
Pro Tip: Domains with Tyk Gateway

Tyk Gateway has full domain support built-in, you can:

Set Tyk to listen only on a specific domain for all API traffic.
Set an API to listen on a specific domain (e.g. api1.com, api2.com).
Split APIs over a domain using a path (e.g. api.com/api1, api.com/api2, moreapis.com/api1, moreapis.com/api2 etc).
If you have set a hostname for the Gateway, then all non-domain-bound APIs will be on this hostname + the listen_path 

/////////////////////////////////////////////////////////////////////////////////////////////////////////////

What is Tyk Pump?

Tyk Pump is responsible for moving analytics between the your API Gateway and the Dashboard database, it can also send data to other sinks such as ElasticSearch, StatsD and InfluxDB.

Tyk has it’s own APT repositories hosted by the kind folks at packagecloud.io, which makes it easy, safe and secure to install a trusted distribution of the Tyk Pump application.

Tutorial

This tutorial will run on an Amazon AWS Ubuntu Server 14.04 LTS instance. We will install Tyk Pump with all dependencies stored locally.

We’re installing on a t2.micro because this is a tutorial, you’ll need more RAM and more cores for better performance.

Pre-requisites:

You have installed MongoDB (usually part of the Dashboard installation)
Note: Skip the MongoDB steps in this guide if you have already installed MongoDB or are using an external host!

Step 1: Set up our APT repositories

First, add our GPGP key which signs our binaries:

    curl https://packagecloud.io/gpg.key | sudo apt-key add -
Do the same for MongoDB (this may change, correct at time of writing):

    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 7F0CEB10
Run update:

    sudo apt-get update
Since our repositories are installed via HTTPS, you will need to make sure APT supports this:

    sudo apt-get install -y apt-transport-https 
Now lets add the required repos and update again (notice the -a flag in the second Tyk commands - this is important!):

    echo "deb http://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.0.list
 
    echo "deb https://packagecloud.io/tyk/tyk-pump/ubuntu/ trusty main" | sudo tee /etc/apt/sources.list.d/tyk_tyk-pump.list
    
    echo "deb-src https://packagecloud.io/tyk/tyk-pump/ubuntu/ trusty main" | sudo tee -a /etc/apt/sources.list.d/tyk_tyk-pump.list
    
    sudo apt-get update
What we’ve done here is:

Added the MongoDB repository for the latest version (3.0 as of time of writing)
Added the Tyk Pump repository
Updated our package list
Step 2: Install the Tyk Pump

We’re now ready to install Tyk Gateway and Tyk Dashboard, along with all the main dependencies: MongoDB. To install everything run:

    sudo apt-get install -y mongodb-org tyk-pump
What we’ve done here is instructed apt-get to install MongoDB and Tyk Pump without prompting, wait for the downloads to complete.

When Tyk Pump is finished installing, it will have installed some init scripts, but it will not be running yet. The next step will be to setup each application - thankfully this can be done with three very simple commands.

Step 3: Configure Tyk Pump

If you don’t complete this step, you won’t see any analytics in your Dashboard, so to enable the analytics service, we need to ensure Tyk Pump is running and configured properly, to configure Tyk Pump is very simple:

    sudo /opt/tyk-pump/install/setup.sh --redishost=localhost --redisport=6379 --mongo=mongodb://127.0.0.1/tyk_analytics
Step 4: Start Tyk Pump

    sudo service tyk-pump start
You can verify if Tyk Pump is running and working by tailing the log file:

    sudo tail -f /var/log/upstart/tyk-pump.log


///////////////////////////////////////////////////////////////////////////////////////////

eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9.eyJhbGxvd2VkX25vZGVzIjoiZGViNzMxYjgtM2IyNC00YWEzLTRlNWUtM2FiOWRlNTY0M2RhIiwiZXhwIjoxNTMxNTY5NzM2LCJvd25lciI6IjU5NjhiMmM4NDVmOTJlNDRiMmQzZmQzZCJ9.fbpKPPSE0DBvCyjSpHlK_EMb-Z0SwM4v5FoJAVOq2qKIH2_sg4u3nTWixvj4WLJm5Hu9cQFuL9_VAKo5kyxcpzJU1t22_Pm058u8GozSfVHUipXAW2oH8En8BMxlvVSUhmWv8bUBIB77uG3RGkuw6D5dhidSVzM7rmEIjOsyLL43oAyls8eDvgRgrPJMhh9L6Yo8wysNutZlAToSVdMjDuD1ZbtHQHRrQFFa8ZXJry8oub-oRNYtb2od_1pmnRwrnkehbuLNk8TQBCSKRPvLQMEsaAW3_5HnOi8N6RqnDN7pIvVTIe4KQJAv_iq6-6gMFXcaBfZpR7rD-1vQs6DNLw
//////////////////////////////////////////////////////////////////////////////////////////////////////////


Install Tyk Dashboard on Ubuntu

Tyk has its own APT repositories hosted by the kind folks at packagecloud.io, which makes it easy, safe and secure to install a trusted distribution of the Tyk Gateway stack.

This tutorial will run on an Amazon AWS Ubuntu Server 14.04 LTS instance. We will install Tyk Dashboard with all dependencies stored locally.

We’re installing on a t2.micro because this is a tutorial, you’ll need more RAM and more cores for better performance.

Pre-requisites:

Ensure port 3000 is open: This is used by the dashboard to provide the GUI and the Developer Portal.
Step 1: Set up our APT repositories

First, add our GPG key which signs our binaries:

    curl https://packagecloud.io/gpg.key | sudo apt-key add -
Do the same for MongoDB (this may change, correct at time of writing):

    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 7F0CEB10
Run update:

    sudo apt-get update
Since our repositories are installed via HTTPS, you will need to make sure APT supports this:

    sudo apt-get install -y apt-transport-https 
Now lets add the required repos and update again (notice the -a flag in the second Tyk commands - this is important!):

    echo "deb http://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.0.list
    
    echo "deb https://packagecloud.io/tyk/tyk-dashboard/ubuntu/ trusty main" | sudo tee /etc/apt/sources.list.d/tyk_tyk-dashboard.list
    
    echo "deb-src https://packagecloud.io/tyk/tyk-dashboard/ubuntu/ trusty main" | sudo tee -a /etc/apt/sources.list.d/tyk_tyk-dashboard.list
    
    sudo apt-get update
What we’ve done here is:

Added the Mongo DB repository for the latest version (3.0 as of time of writing)
Added the Tyk Dashboard repository
Updated our package list
Step 2: Install the Tyk Dashboard

We’re now ready to install Tyk Gateway and Tyk Dashboard, along with all the main dependencies: Redis and MongoDB. To install everything run:

    sudo apt-get install -y mongodb-org tyk-dashboard
What we’ve done here is instructed apt-get to install MongoDB and the Tyk Dashboard without prompting. Wait for the downloads to complete.

When Tyk Dashboard is finished installing, it will have installed some init scripts, but it will not be running yet. The next step will be to setup each application - thankfully this can be done with three very simple commands.

Configure Tyk Dashboard

We can set the dashboard up with a helper setup command script, the below will get the dashboard set up for the local instance:

    sudo /opt/tyk-dashboard/install/setup.sh --listenport=3000 --redishost=localhost --redisport=6379 --mongo=mongodb://127.0.0.1/tyk_analytics --tyk_api_hostname=$HOSTNAME --tyk_node_hostname=http://localhost --tyk_node_port=8080 --portal_root=/portal --domain="XXX.XXX.XXX.XXX"
Note: Make sure to use the actual DNS hostname or the public IP of your instance as the last parameter.

What we have done here is:

--listenport=3000: Told Tyk Dashboard (and Portal) to listen on port 3000.
--redishost=localhost: Tyk Dashboard should use the local Redis instance.
--redisport=6379: Tyk Dashboard should use the default port.
--domain="XXX.XXX.XXX.XXX": Bind the dashboard to the IP or DNS hostname of this instance (required).
--mongo=mongodb://127.0.0.1/tyk_analytics: Use the local MongoDB (should always be the same as the gateway).
--tyk_api_hostname=$HOSTNAME: Tyk Dashboard has no idea what hostname has been given to Tyk, so we need to tell it, in this instance we are just using the local HOSTNAME env variable, but you could set this to the public-hostname/IP of the instance.
--tyk_node_hostname=http://localhost: Tyk Dashboard needs to see a Tyk node in order to create new tokens, so we need to tell it where we can find one, in this case, use the one installed locally.
--tyk_node_port=8080: Tell the dashboard that the Tyk node it should communicate with is on port 8080.
--portal_root=/portal: We want the portal to be shown on /portal of whichever domain we set for the portal.
Step 1: Start Tyk Dashboard

    sudo service tyk-dashboard start
Notice how we haven’t actually started the gateway yet, because this is a Pro install, we need to enter a license first.

Step 2: Enter your dashboard license

Go to http://your-host-name:3000/.

You will see a screen asking for a license, enter it in the section marked “Already have a license?” and click Use this license.

That’s it, your dashboard is now ready to be bootstrapped.

Note: You can bypass this step by adding your license manually to the /var/opt/tyk-dashboard/tyk_analytics.conf file directly in the field marked license.

If all is going well, you will be taken to a log in screen - we’ll get to that soon.

Step 3: Restart the dashboard and start the gateway process

Because we’ve just entered a license via the UI, we need to make sure that these changes get picked up, so to make sure things run smoothly, we restart the dashboard process (you only need to do this once) and then start the gateway:

    sudo service tyk-dashboard restart 
    sudo service tyk-gateway start
Step 4: Bootstrap the dashboard with an initial user and organisation

When Tyk Dashboard is created for the first time, it has no initial user base or organisation to add data to, so we need to add this.

The best way to add this data is with the Admin API, to make it really easy we’ve supplied a bootstrap script that will set you up. If you want to customise it, take a look at the file in /opt/tyk-dashboard/install/bootstrap.sh.

Pre-requisites for this command:

This command assumes you are running on a Linux shell such as Bash
This command assumes you have Python 2.6 or 2.7 installed
To bootstrap your instance:

    sudo /opt/tyk-dashboard/install/bootstrap.sh XXX.XXX.XXX.XXX
This command tells the bootstrap script to use the localhost as the base for the API calls, you can run the bootstrap remotely and change the first command line parameter to the DNS hostname of your instance.

You will now be able to log into and test your Tyk instance with the values given to you by the bootstrap script.







GPG error: https://packagecloud.io trusty InRelease NO_PUBKEY 37BBEE3F7AD95B3F


tyk安装

 tyk2.3  ubuntu16.04
 https://tyk.io/docs/get-started/with-tyk-on-premise/installation/on-ubuntu/
 
 Tyk API网关介绍及安装说明
 http://www.cnblogs.com/lazio10000/p/5281905.html
 
 HTTP API Development Tools
 https://github.com/yosriady/api-development-tools
 
 What is Tyk Gateway ?
 https://www.tyk.io/docs/concepts/tyk-components/gateway/
 
 tyk性能
 https://www.tyk.io/docs/deploy-tyk-premise-production/
 
 tyk api
 http://chariotsolutions.com/blog/post/tyk-on-premise-api-gateway/
 
 API DESIGN 202: ARCHITECTURAL LAYERS
 http://www.apiacademy.co/resources/api-design-202-architectural-layers/
 
 API Gateway Pattern
 https://www.linkedin.com/pulse/api-gateway-pattern-ronen-hamias
