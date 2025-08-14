<h1>Splunk: Home SOC Lab</h1>

<h2>Description</h2>
Set up a SOC lab by installing and configuring Splunk on a Linux host, deploying Splunk Forwarders, and ingesting OS-based and web logs for centralized analysis. Gained hands-on experience with SIEM deployment, log collection, and cross-platform monitoring—key skills for SOC operations. TryHackMe site was used and I had to provide answers to questions.

<h2>Project Walk-Through:</h2>

<h3>Step 1: Install Splunk (Linux host)</h3>

- Download Splunk
   - Create an account on splunk.com and download the latest Splunk Enterprise (Linux .tgz).
- Switch to root
   - sudo -i (or su -)
- Extract the installer
   - tar -xvzf splunk_installer.tgz
- Move Splunk to /opt
   - mv splunk /opt/
- Start Splunk and accept the license
   - cd /opt/splunk/bin
   - ./splunk start --accept-license
- Create admin credentials
   -When prompted on first start, set the admin username and password.
- Access the web UI
   - In your VM browser, go to http://<hostname>:8000 (e.g., http://coffely:8000) and log in with the credentials you created.
- Q: What is the default port for Splunk?
   - A: 8000
<br/>
<img src="https://i.imgur.com/F0tCNS3.png" height="80%" width="80%">
<br />

<h3>Step 2: Splunk: Interacting with CLI</h3>

- Command: Splunk start (This command starts all the necessary Splunk processes and enables the server to accept incoming data.)
<img src="https://i.imgur.com/CUlPEV0.png" height="80%" width="80%">

- Command: Splunk stop (This command stops all the running Splunk processes and disables the server from accepting incoming data.)
<img src="https://i.imgur.com/zFPJ54A.png" height="80%" width="80%">

- Command: Splunk restart (This command stops all the running Splunk processes and then starts them again. This is useful when changes have been made to the Splunk configuration files or when the server needs to be restarted for any other reason.)
<img src="https://i.imgur.com/syoh7Bn.png" height="80%" width="80%">

- Command: Splunk status (This command will display information about the current state of the server, including whether it is running or not, and any errors that may be occurring.)
<img src="https://i.imgur.com/Je8b7hl.png" height="80%" width="80%">

- Command: Splunk search (This command can be used to search for specific events, as well as to perform more complex searches using Splunk's search language.)
<img src="https://i.imgur.com/8aIfuah.png" height="80%" width="80%">

- Q: In Splunk, what is the command to search for the term coffely in the logs?
   - A: ./bin splunk search coffely

<h3>Step 3: Splunk: Data Ingestion</h3>

- Configuring data ingestion allows for the data to be indexed and searchable for the analysts. In this task, we will use Splunk Forwarder to ingest the Linux logs into our Splunk instance.
- Splunk has two primary types of forwarders that can be used in different use cases. They are explained below:
   - Heavy Forwarders: Used when we need to apply a filter, analyze or make changes to the logs at the source before forwarding it to the destination.
   - Universal Forwarders: Lightweight agent that gets installed on the target host, and its main purpose is to get the logs and send them to the Splunk instance or another forwarder without applying any filters or indexing. It has to be downloaded separately and has to be enabled before use.
- Now we can install the forwarder:
   - Change the user to sudo, unpack, and install the forwarder with the following command: tar xvzf splunkforwarder.tgz
- The above command will install all required files in the folder splunkforwarder
- Next, we will move this folder to /opt/ path with the command: mv splunkforwarder /opt/
<img src="https://i.imgur.com/edzqqHU.png" height="80%" width="80%">

- By default, Splunk forwarder runs on port 8089. If the system finds the port unavailable, it will ask the user for the custom port. In this example, we are using 8090 for the forwarder.
- Q: What is the default port, on which Splunk Forwarder runs on?
   - A: 8089
 
<h3>Step 4: Configuring Forwarder on Linux</h3>

- Splunk Configuration: Log into Splunk and Go to Settings > Forward and receiving tab, as shown below:
<img src="https://i.imgur.com/YJaQ3E1.png" height="80%" width="80%">

- It will show multiple options to configure both forwarding and receiving. As we want to receive data from the Linux endpoint, click on "Configure receiving" and then proceed by configuring a new receiving port.
<img src="https://i.imgur.com/ls4AeNu.png" height="80%" width="80%">

- By default, the Splunk instance receives data from the forwarder on the port 9997. It's up to us to use this port or change it. For now, we will configure our Splunk to start listening on port 9997 and Save, as shown below:
<img src="https://i.imgur.com/npcHmaX.png" height="80%" width="80%">
<img src="https://i.imgur.com/qBAHNFr.png" height="80%" width="80%">

- Creating Index: Now that we have enabled a listening port, next is to create an index that will store all the receiving data. If we do not specify an index, it will start storing received data in the default index, which is called the main index.
<img src="https://i.imgur.com/tucEbqQ.png" height="80%" width="80%">
<img src="https://i.imgur.com/qcJeRHa.png" height="80%" width="80%">
<img src="https://i.imgur.com/1uS14nx.png" height="80%" width="80%">

- Configuring Forwarder: Next, configure the forwarder to ensure it sends the data to the right destination. Back in the Linux host terminal, go to the /opt/splunkforwarder/bin directory:
<img src="https://i.imgur.com/GbwAREp.png" height="80%" width="80%">

- This command will add the forwarder server, which listens to port 9997
- Next, we tell Splunk Forwarder to monitor the /var/log/syslog file.
<img src="https://i.imgur.com/kZPo1nG.png" height="80%" width="80%">

- Exploring Inputs.conf: We can open the inputs.conf file located in /opt/splunkforwarder/etc/apps/search/local, and look at the configuration added after the commands we used above:
<img src="https://i.imgur.com/hfO5PWf.png" height="80%" width="80%">
<img src="https://i.imgur.com/02HeP5W.png" height="80%" width="80%">

   - We can view the content of the input.conf using the cat command.
- Utilizing Logger Utility: Logger is a built-in command line tool to create test logs added to the syslog file. As we are already monitoring the syslog file and sending all logs to the Splunk, the log we generate in the next step can be found with Splunk logs. To run the command, use the following command:
<img src="https://i.imgur.com/DeIbr9M.png" height="80%" width="80%">
<img src="https://i.imgur.com/M8gMQqv.png" height="80%" width="80%">

- You can search the index we created ie linux_host on the Splunk console.
- Q: Follow the same steps and ingest /var/log/auth.log file into Splunk index Linux_logs. What is the value in the source typefield?
   - A: syslog
- Q: Create a new user named analyst using the command adduser analyst. Once created, look at the events generated in Splunk related to the user creation activity. How many events are returned as a result of user creation?
   - A: 6
- Q: What is the path of the group the user is added after creation?
   - A: /etc/group
