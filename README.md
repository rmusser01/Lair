Lair: An Attack Collaboration Framework
===========================================
Lair is a web application framework written in Meteor.js. It's goal is _______. 

[What is Lair?](#whatis)

[Temporary/Quick Install](#quick)

[Longterm Install](#long)

[Lair-Drones](#drones)

[Contact Us/Get involved!](#contact)

###<a name="whatis"></a> What is Lair?
---------------------------------------
Lair is _________

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

[lair-v1.0.4-darwin-x64.7z](https://github.com/fishnetsecurity/Lair/releases/download/v1.0.4/lair-v1.0.4-darwin-x64.7z)

[lair-v1.0.4-linux-x64.7z](https://github.com/fishnetsecurity/Lair/releases/download/v1.0.4/lair-v1.0.4-linux-x64.7z)

[lair-v1.0.4-linux-x86.7z](https://github.com/fishnetsecurity/Lair/releases/download/v1.0.4/lair-v1.0.4-linux-x86.7z)

Next, download the latest drones python package [here](https://github.com/fishnetsecurity/Lair-Drones/releases/latest). 

Click [here](#drones) for instructions on Lair-Drones

Running lair from the application package above is relatively simple.

Download one of the above packages; extract said package, and execute the start.sh script:

        ./start.sh <ip>


with IP being the accessible IP of the Lair server. 

At the end of the script, you'll be given the IP:PORT on which Lair is accessible. To start uploading information to the server, check the next section, [Drones](#drones).

To stop Lair and the dependent services:

        ./stop.sh

Running the start.sh script performs the following:
* Installs stunnel
* Edits stunnel.conf file
* Generates keypair for SSL using OpenSSL
* Installs Node.js+Meteor.js+Lair application
* Creates MongoDB db for Lair as well as users/admins for said db
* Launches MongoDB daemon listener on 127d.0.0.1:11015
* Launches Node.js web server on 127.0.0.1:11016 ; hosts  Lair application
* Launches Node.js proxy, listening on 127.0.0.1:11013 ; Downgrades SSL connections to HTTP for communication between the server and application running on server

-------------------------------------
<a name="drones"></a>Drones
-------------------------------------
Lair takes a different approach to uploading, parsing, and ingestion of automated tool output (xml). We push this work off onto client side scripts called drones. These drones connect directly to the database. To use them all you have to do is export an environment variable "MONGO_URL".

Linux way:

        export MONGO_URL='mongodb://username:password@ip:27017/lair?ssl=true'

This uses the export command to set the var MONGO_URL to everything after the equal sign. 

"export" is used to set global variables in the current shell and child processes.

This is done because Drones are currently written to check for the global as a means of avoiding hardcoding addresses/credentials.

With the environment variable set you will need a project id to import data. You can grab this from the upper right corner of the lair dashboard next to the project name. You can now run any drone.


        drone-nmap <pid> /path/to/nmap.xml

You can install the drones to PATH with pip


        pip install lairdrone-<version>.tar.gz

For the latest release,please check here:
<a href="https://github.com/fishnetsecurity/Lair-Drones/releases">Latest Lair-Drones Releases</a>

For the source code, check that out here:
<a href="https://github.com/fishnetsecurity/Lair-Drones">Lair-Drones Source</a>


        
-------------------------------------
<a name="long"></a>Installing Lair
-------------------------------------

Currently Supported OS's : Linux; OS X(Intel).

Required Applications:

* Node.js : Platform built on Chrome's JavaScript runtime for building Javascript applications. (https://www.nodejs.org)
* Meteor : Javascript Framework for building web applications. ( https://www.meteor.com/ )
* stunnel :  Multiplatform GNU/GPL-licensed proxy encrypting arbitrary TCP connections with SSL/TLS. ( https://www.stunnel.org/index.html ) 
 
	-The Lair application uses HTTPS to authenticate with hosts and then drops the HTTPS > HTTP when  interfacing with the database.

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
	-This command causes OpenSSL to create a new key pair. Change storage location of keys to where you wish. 
* stunnel: Declare key location in stunnel.conf file (where is the config file stored?^^^)	
* stunnel: Declare pid of stunnel in stunnel.conf	
* stunnel: Add the following to the stunnel.conf file: 
		[lair]
		accept=11014 port ; port you've set to have drones connect to.
		connect=127.0.0.1:11015 ; port you've set MongoDB to listen on.
* Launch stunnel
* Drop the Lair app [available here] (https://github.com/fishnetsecurity/Lair/tree/master/app) into Meteor
 
	-Dropping files into meteor: http://docs.meteor.com/#structuringyourapp
        
* Create  MongoDB admin/pass & Lair DB Username/Password

	        -How to do that: copout: [Getting started with Mongo](http://docs.mongodb.org/manual/tutorial/getting-started/)
	
* Set indexes of MongoDB DB.	
	
* Launch MongoDB, Declare IP/Port, Declare DB/Log location

Example:	

	# /path/to/mongod --port 11015 --auth --dbpath=lair_db --bind_ip 127.0.0.1 --nohttpinterface --fork --logpath=deps/var/log/mongodb.log 1>/dev/null 2>error.log			
				
	-launches mongodb daemon, specifies listen port as 11015, enables db auth from remote hosts, defines where db path will be/is, binds listener ip as 127.0.0.1, disables http interface, --fork sets Mongo to run as a daemon, sets log path

* Start Node HTTP Proxy/Server
	- Instructions here
* Server is up and running available on whichever port you set it to.
* Distribute Lair-Drones to clients. Find those and the relevant info here: [Lair-Drones](#drones)
		 

-------------------------------------
<a name="contact"></a>Contact
-------------------------------------
If you need assistance with installation, usage, or are interested in contributing, please contact either Dan Kottmann or Tom Steele at any of the below. IRC is the best way to get a quick response.

Tom Steele
- tom@huptwo34.com
- [@_tomsteele](https://twitter.com/_tomsteele)
- freenode: hydrawat

Dan Kottmann
- [@djkottmann](https://twitter.com/djkottmann)
