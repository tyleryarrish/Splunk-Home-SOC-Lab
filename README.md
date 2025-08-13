<h1>Splunk: Home SOC Lab</h1>

<h2>Description</h2>
Set up a SOC lab by installing and configuring Splunk on both Linux and Windows hosts, deploying Splunk Forwarders, and ingesting OS-based and web logs for centralized analysis. Gained hands-on experience with SIEM deployment, log collection, and cross-platform monitoring—key skills for SOC operations. TryHackMe site was used and I had to provide answers to questions.

<h2>Program walk-through:</h2>

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


