# blockbox




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
