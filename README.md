Lair: An Attack Collaboration Framework
===========================================
 Lair is a reactive attack collaboration framework and web application built with meteor.js.


[What is Lair?](#whatis)

[Temporary/Quick Install](#quick)

[Creating a Build Environment](#long)

[Lair-Drones](#drones)

[Contribute/Get involved!](#contribute)

[Contact Us](#contact)

###<a name="whatis"></a> What is Lair?
---------------------------------------
Lair is a reactive attack collaboration framework and web application built with meteor.js.

Lair operates off the client server model, with a series of packaged python scripts("Drones") that resides on the user's machine, which are used to authenticate and upload scan results into the server application.

 
Data sets are sorted into projects, for coherence, while data within projects are sorted by:
* Hosts
        - List of hosts sorted
* Services
        - Display services of specified hosts.
* Vulnerabilities
        - List vulnerabilities, show various information about. 
* Notes
        - Notes. Simple collaborative notebook.
* Credentials
        - Store User;Pass; combos/hashes etc.
* Contributors
        - Define who may contribute to said project.
* Files
        - Store yer loot here.
* Log
        - A log of all commands executed along with a list of all data uploaded by drones. 



Like the saying, a picture is worth a thousand words, here's a picture walkthrough of Lair in action so that you  can 
see what it might help you do.


In this picture, we see Lair after a fresh install/clear of all projects.
![Feeling Fresh](/path/to/image.jpg "FrontFresh")


Here, we have an image of a project being created, with the name "Test", and the description "chicken".
![Test1](/path/to/image.jpg "Testpage")
This picture shows the Services section with the nmap scan results of an "-A" scan on scanme.nmap.org imported by a Lair
drone into the database showing. 

This was performed with the lairdrone client and the command "drone-nmap <pid> 
/path/to/nmap.xml" "pid" being the project Id# available from the lair server homepage on the upper right. 
![Test2](/path/to/image.jpg "Testresults")
Currently, there are drones for Nmap, Burp, Nessus, and Nexpose.
Uses drones for data collection/import.

Imports exist for Nmap, Burp, Nessus, and Nexpose. Because Lair is opensource and written in Python, the barrier to writing drones that can upload results of other scans/results that are exportable to .xml is relatively low. If one happens to do so, please share your drones with us so that others may use them.


-------------------------------------
<a name="quick"></a>Temporary/Quick Installation:
--------------------------------------------------
Precompiled application packages are available for Linux and OS X. Download one of the current application packages below:

[lair-v1.0.5-darwin-x64.7z](https://github.com/fishnetsecurity/Lair/releases/download/v1.0.5/lair-v1.0.5-darwin-x64.7z)

[lair-v1.0.5-linux-x64.7z](https://github.com/fishnetsecurity/Lair/releases/download/v1.0.5/lair-v1.0.5-linux-x64.7z)

[lair-v1.0.5-linux-x86.7z](https://github.com/fishnetsecurity/Lair/releases/download/v1.0.5/lair-v1.0.5-linux-x86.7z)

Next, download the latest drones python package [here](https://github.com/fishnetsecurity/Lair-Drones/releases/latest).

Running lair from the application package above is self-explanatory.
To start Lair and all the required services:

        ./start.sh <ip>

with IP being the accessible IP of the Lair server. 

Running the start.sh script performs the following:
* Installs stunnel
* Edits stunnel.conf file
* Generates keypair for SSL using OpenSSL
* Installs Node.js+Meteor.js+Lair application
* Creates MongoDB db for Lair as well as users/admins for said db
* Launches MongoDB daemon listener on 127d.0.0.1:11015
* Launches Node.js web server on 127.0.0.1:11016 ; hosts  Lair application
* Launches Node.js proxy, listening on 127.0.0.1:11013 ; Downgrades SSL connections to HTTP for communication between the server and application running on server

At the end of the script, you'll be given the IP:PORT on which Lair is accessible. To start uploading information to the server, check the next section, [Drones](#drones).

To stop Lair and the dependent services:

        ./stop.sh



-------------------------------------
<a name="drones"></a>Drones
-------------------------------------
Lair takes a different approach to uploading, parsing, and ingestion of automated tool output (xml). We push this work off onto client side scripts called drones. These drones connect directly to the database. To use them all you have to do is export an environment variable "MONGO_URL". This variable is probably going to be the same you used for installation

        export MONGO_URL='mongodb://username:password@ip:27017/lair?ssl=true'

With the environment variable set you will need a project id to import data. You can grab this from the upper right corner of the lair dashboard next to the project name. You can now run any drones.

        drone-nmap <pid> /path/to/nmap.xml

You can install the drones to PATH with pip

        pip install lairdrone-<version>.tar.gz

For the latest release,please check here:
<a href="https://github.com/fishnetsecurity/Lair-Drones/releases">Latest Lair-Drones Releases</a>

For the source code, check that out here:
<a href="https://github.com/fishnetsecurity/Lair-Drones">Lair-Drones Source</a>

-------------------------------------
<a name="long">Setting up a Development Environment in Linux</a>
-------------------------------------
* <a name="linux">Setting up a Development Environment in Linux</a>
* <a name="osx">Setting up a Development Environment in OS X</a>
-------------------------------------
<a name="linux">Setting up a Development Environment in Linux</a>
-------------------------------------
Required Applications:
* Node.js : Platform built on Chrome's JavaScript runtime for building Javascript applications. (https://www.nodejs.org)
* Meteor : Javascript Framework for building web applications. ( https://www.meteor.com/ )
* stunnel :  Multiplatform GNU/GPL-licensed proxy encrypting arbitrary TCP connections with SSL/TLS. ( https://www.stunnel.org/index.html ) 
  * The Lair application uses HTTPS to authenticate with hosts and then drops the HTTPS > HTTP when  interfacing with the database.
* openssl-devel(Redhat-based) or libssl-devel(debian based)
* simple proxy : Currently, a simple proxy written in .js is used, though it should be possible to use another.(https://gist.github.com/tomsteele/5118594)
* MongoDB : Document database that provides high performance, high availability, and easy scalability. ( http://www.mongodb.org )
	        	
Steps performed in Short:
* Install stunnel
* Generate SSL keypair
* Edit stunnel.conf file
* Install Node.js
* Meteor.js (comes with Lair app?)
* Create MongoDB db for Lair as well as users/admins for said db
* Launch MongoDB daemon listener on 127.0.0.1:11015
* Launch Node.js web server on 127.0.0.1:11016
* Launch Node.js proxy listening on 127.0.0.1:11013

Walkthrough Installation:
* Install Node.js: https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager
* Install Meteor.js (comes with Lair app?)
* Generate key pair for stunnel/SSL
  * Using OpenSSL: # openssl req -new -x509 -days 3095 -nodes -out /place/to/save/key/lair.pem -keyout /place/to/save/key/lair-key.pem
  * This command causes OpenSSL to create a new key pair. Change storage location of keys to where you wish. 
  * stunnel: Declare key location in stunnel.conf file (where is the config file stored?^^^)	
  * stunnel: Declare pid of stunnel in stunnel.conf	
  * stunnel: Add the following to the stunnel.conf file: 
		[lair]
		accept=11014 port ; port you've set to have drones connect to.
		connect=127.0.0.1:11015 ; port you've set MongoDB to listen on.
* Launch stunnel
* Drop the Lair app [available here] (https://github.com/fishnetsecurity/Lair/tree/master/app) into Meteor
 *  Dropping files into meteor: http://docs.meteor.com/#structuringyourapp
        
* Create  MongoDB admin/pass & Lair DB Username/Password
  *How to do that: copout: [Getting started with Mongo](http://docs.mongodb.org/manual/tutorial/getting-started/)
* Set indexes of MongoDB DB.	
* Launch MongoDB, Declare IP/Port, Declare DB/Log location

Example:	

	# /path/to/mongod --port 11015 --auth --dbpath=lair_db --bind_ip 127.0.0.1 --nohttpinterface --fork --logpath=deps/var/log/mongodb.log 1>/dev/null 2>error.log			
  *launches mongodb daemon, specifies listen port as 11015, enables db auth from remote hosts, defines where db path will be/is, binds listener ip as 127.0.0.1, disables http interface, --fork sets Mongo to run as a daemon, sets log path
* Start Node HTTP Proxy/Server
  *Instructions here
* Server is up and running available on whichever port you set it to.
* Distribute Lair-Drones to clients. Find those and the relevant info here: [Lair-Drones](#drones)
	

	         
-------------------------------------
<a name="osx">Setting up a development environment (OSX)</a>
-------------------------------------
1. Install mongodb 2.6.0 or later preferably with ssl support (`brew install mongodb --with-openssl`)
2. If using SSL then perform the following to setup certs:
  * `openssl req –new –x509 –days 365 –nodes –out mongodb-cert.crt –key out mongodb-cert.key`
  * `cat mongodb-cert.crt mongodb-cert.key > mongodb.pem`
  * Start Mongo with SSL support via mongod.conf or command line (`mongod —sslMode requireSSL —sslPEMKeyFile mongodb.pem`)
3. Add a Lair database user:
  * `mongo lair --ssl`
  * `db.createUser({user: "lair", "pwd": "yourpassword", roles:["readWrite"]});`
  * Confirm user authentication: `db.auth("lair", "yourpassword");`
4. Set the appropriate Lair environment variable...
  * With SSL:  `export MONGO_URL=mongodb://lair:yourpassword@localhost:27017/lair?ssl=true`
  * No SSL: `export MONGO_URL=mongodb://lair:password@localhost:27017/lair`
5. [Download](http://nodejs.org/download/) and install node.js
6. Install Meteor: `curl https://install.meteor.com | /bin/sh`
7. Install Meteorite package manager: `sudo npm install -g meteorite`
8. Fork the Lair project on GitHub and clone the repo locally
9. Install dependencies: `cd /path/to/lair/app && mrt` (you can kill the mrt process after dependencies are downloaded)
10. Start Lair:  `cd /path/to/lair/app && meteor`
11. Browse to http://localhost:3000
12. Code your changes and submit pull requests!

There are occasional issues and confilicts with Meteor and the Fibers module. If you run into a situation where you cannot start Meteor due to Fibers conflicts, refer to the following for potential fixes:
* [Error: Cannot find module 'fibers'](http://stackoverflow.com/questions/15851923/cant-install-update-or-run-meteor-after-0-6-release)
* [Error: fibers.node is missing](http://stackoverflow.com/questions/13327088/meteor-bundle-fails-because-fibers-node-is-missing)


-------------------------------------
<a name="contribute">Contributing</a>
-------------------------------------
1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request 

-------------------------------------
<a name="contact"></a>Contact
-------------------------------------
If you need assistance with installation, usage, or are interested in contributing, please contact either Dan Kottmann or Tom Steele at any of the below. IRC is the best way to get a quick response.

Tom Steele
- tom@stacktitan.com
- [@_tomsteele](https://twitter.com/_tomsteele)

Dan Kottmann
- djkottmann@gmail.com
- [@djkottmann](https://twitter.com/djkottmann)

