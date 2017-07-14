# blockbox


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
