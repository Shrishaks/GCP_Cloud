Google Compute Engine VM Startup Script

1. What is a VM Startup Script?

A startup script is a file that contains commands that run when a virtual machine (VM) instance boots.

GCE provides support for running startup scripts on Linux VMs and Windows VMs.

Startup Scripts can be configured at VM-level and Project-level.

Startup Scripts configured at VM-level will override the Project-level startup scripts.

Why do we need to configure Project-level startup scripts?

If we want all the VMs in that respective project to have the same scripts applied (install antivirus package), then we can define project-level startup scripts which apply to all VMs.

2. Linux Startup Script

Linux Startup Script Types

Bash and Non-Bash

To use a non-bash file, designate the interpreter by adding a #! to the top of the file.

Example: #!/usr/bin/python3

What happens when we provide a Startup Script to a VM?

Compute Engine copies the startup script to the VM.

Sets run permissions on the startup script.

Runs the startup script as the root user when the VM boots.

3. Google Compute Engine - VM Startup Script

Method 1: Pass a startup script directly

We can do this in two ways:

Option 1: Pass the script content in Startup Script Text Box

Option 2: Pass in Metadata

Key: startup-script

Value: Script content

Method 2: Pass a startup script from a local file

We can do this via gcloud only.

gcloud compute instances create VM_NAME \
--image-project=debian-cloud \
--image-family=debian-10 \
--metadata-from-file=startup-script=FILE_PATH

Method 3: Pass a startup script from Cloud Storage

We can do this by passing in metadata.

Key: startup-script-url

Value: https://storage.googleapis.com/BUCKET/FILE

Value: gs://BUCKET/FILE

4. VM Startup Scripts

How many startup scripts can we use per VM?

We can use multiple startup scripts.

What is the Order of Execution for Startup Scripts?

Metadata-Key: startup-script

Startup scripts provided directly or locally will take the first preference in order of execution.

Metadata-Key: startup-script-url

Startup scripts provided in Cloud Storage will take the second preference in order of execution.

What is the startup script maximum allowed size?

Scripts up to 256 KB are allowed.

5. Linux Startup Script - NGINX Web Server

#!/bin/bash
sudo apt install -y telnet
sudo apt install -y nginx
sudo systemctl enable nginx
sudo chmod -R 755 /var/www/html
HOSTNAME=$(hostname)
sudo echo "<!DOCTYPE html> <html> <body style='background-color:rgb(250, 210, 210);'> <h1>Welcome to StackSimplify - WebVM App1 </h1> <p><strong>VM Hostname:</strong> $HOSTNAME</p> <p><strong>VM IP Address:</strong> $(hostname -I)</p> <p><strong>Application Version:</strong> V1</p> <p>Google Cloud Platform - Demos</p> </body></html>" | sudo tee /var/www/html/index.html

Script Explanation

#!/bin/bash — Specifies Bash as the script interpreter.

sudo apt install -y telnet — Installs Telnet.

sudo apt install -y nginx — Installs the NGINX web server.

sudo systemctl enable nginx — Enables NGINX to start automatically when the VM boots.

sudo chmod -R 755 /var/www/html — Sets permissions on the web directory.

HOSTNAME=$(hostname) — Retrieves the VM hostname.

sudo tee /var/www/html/index.html — Writes the HTML content to the web server's index page.

Sample Web Page Details

The script creates a sample web page displaying:

Welcome to StackSimplify - WebVM App1

VM Hostname

VM IP Address

Application Version: V1

Google Cloud Platform - Demos

After the startup script executes and NGINX is running, the sample web page can be accessed through a browser using the VM's external IP address.

6. Key Learnings

Understanding VM-level and project-level startup scripts.

Configuring startup scripts using the Google Cloud Console, metadata, local files, and Cloud Storage.

Understanding startup script execution and permissions.

Automating NGINX installation and sample web page deployment using Bash.

Understanding startup script execution order and the maximum allowed script size.
