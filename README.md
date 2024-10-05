# git_assignment_HeroVired_NetworkingandServer
This repository contains the assignment file with all three tasks performed locally, and the corresponding outputs are attached within it.

Coonect to AWS: ssh  -i /home/rajiv-sharma/Downloads/DevOpsTest.pem ec2-user@ec2-43-205-115-66.ap-south-1.compute.amazonaws.com
Task 1: Deploy a Website on Localhost Using Apache2
Step-by-Step Documentation
Objective: Deploy a simple HTML website on localhost using either Apache2 or Nginx and configure a custom DNS name rajivtest.
Prerequisites:
    • A working Ubuntu installation via WSL, VirtualBox, or a native installation.
    • Basic understanding of HTML and web servers.
Option 1: Deploying with Apache2
Step 1: Install Apache2
Open your terminal and install Apache2 using the following command:
> sudo apt update
> sudo apt install apache2 -y
Step 2: Configure Apache2
After installation, ensure Apache is running:
> sudo systemctl start apache2
To ensure Apache2 runs on startup, enable it:
> sudo systemctl enable apache2
Step 3: Create a Simple HTML Website
Create a directory for your website and an HTML file:
> sudo mkdir -p /var/www/rajivtest
> sudo nano /var/www/rajivtest/index.html
Inside index.html, add your HTML code:
Eg code: 

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Apache2 test deployment</title>
</head>
<body>
    <h1>Hello Rajiv!</h1>
    <p>This is a simple HTML page hosted on Apache2.</p>
</body>
</html>


Step 4: Configure Virtual Host
Create a new virtual host configuration file for your website:

> sudo nano /etc/apache2/sites-available/rajivtest.conf
Add the following configuration to the file:

<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    ServerName rajivtest
    DocumentRoot /var/www/rajivtest

    <Directory /var/www/rajivtest>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
Step 5: Enable the Site
Enable the website and restart Apache:
> sudo a2ensite rajivtest
> sudo systemctl reload apache2
Step 6: Configure DNS
Add rajivtest to your local DNS by editing the /etc/hosts file:
> sudo nano /etc/hosts
Add the following entry:
127.0.0.1   rajivtest
Step 7: Access the Website
Open your browser and go to http://rajivtest. You should see your website.

Task 2: Python Script to Monitor Subdomain Status
Script Overview
This Python script will check the status of subdomains (UP or DOWN) by pinging them every minute and display the results in a table.
Step 1: Install Dependencies
You will need the requests library to check the status of each subdomain:
pip install requests
Step 2: Write the Python Script
Create a Python script subdomain_checker.py:
python
Copy code
import requests
import time
from prettytable import PrettyTable

# Subdomains to check
subdomains = ['http://sub1.example.com', 'http://sub2.example.com']

def check_status():
    table = PrettyTable(['Subdomain', 'Status'])
    for subdomain in subdomains:
        try:
            response = requests.get(subdomain)
            if response.status_code == 200:
                table.add_row([subdomain, 'UP'])
            else:
                table.add_row([subdomain, 'DOWN'])
        except requests.exceptions.RequestException:
            table.add_row([subdomain, 'DOWN'])
    print(table)

if __name__ == '__main__':
    while True:
        print("\nChecking status of subdomains...\n")
        check_status()
        time.sleep(60)
Step 3: Run the Script
Run the script to check subdomains every minute:
python3 subdomain_checker.py
Output:

Task 3: Virtual Machine Overview and Nmap Scan
Task Overview:
    • Install a virtual machine using Oracle VirtualBox.
    • Use Ubuntu 22.04 as the guest OS.
    • Install Nginx inside the Ubuntu VM and host a website.
    • Scan the VM using Nmap from the host machine and observe open ports.

Step 1: Install VirtualBox
1.1 Download VirtualBox
    1. Go to the VirtualBox website.
    2. Click on Downloads.
    3. Select the package for your operating system (Windows, macOS, or Linux).
1.2 Install VirtualBox
    • For Windows:
        1. Run the downloaded .exe file.
        2. Follow the installation prompts to complete the setup.
    • For macOS:
        1. Open the .dmg file and run the VirtualBox installer.
    • For Ubuntu/Linux:
        1. Open the terminal and install the downloaded .deb package:
           sudo dpkg -i <VirtualBox_package_name>.deb
        2. Resolve any dependencies using:
           sudo apt --fix-broken install
1.3 Add User to vboxusers Group (For Linux Users)
After installing VirtualBox, run the following command to add your user to the vboxusers group:
sudo usermod -aG vboxusers $USER

Then reboot your system to apply changes.

Step 2: Download and Set Up Ubuntu 22.04 VM
2.1 Download Ubuntu Image
    1. Go to OSBoxes.
    2. Download the Ubuntu 22.04 image for VirtualBox.
2.2 Import the VM into VirtualBox
    1. Open VirtualBox and click on File > Import Appliance.
    2. Select the downloaded .ova file and proceed with the import process.
2.3 Configure Network Adapter
    • Go to the Settings of the VM.
    • Under the Network tab, ensure that Adapter 1 is set to Bridged Adapter (this will allow the VM to obtain its own IP address).
    • If you are using wifi select Name to wlp13s0

Step 3: Install Nginx and Host a Website on the VM
3.1 Start the VM and Update Ubuntu
    1. Start the Ubuntu VM.
    2. Open a terminal inside the VM and update the system:
       > sudo apt update
       > sudo apt upgrade -y
3.2 Install Nginx
    1. Install Nginx on the Ubuntu VM:
       > sudo apt install nginx -y
    2. Verify that Nginx is installed and running:
       > sudo systemctl status nginx
    3. If Nginx is not running, start it:
       > sudo systemctl start nginx
3.3 Host a Website
    1. Create a simple HTML page to host on Nginx:
       > sudo nano /var/www/html/index.html
       Add the following HTML content:
       <!DOCTYPE>
       <html>
       <head>
           <title>Welcome to My Website</title>
       </head>
       <body>
           <h1>Hello, this is rajiv’s website hosted on an Ubuntu VM!</h1>
       </body>
       </html>
    2. Save the file and exit.
    3. Restart Nginx to apply changes:
       sudo systemctl restart nginx



    4. Find the IP address of the VM using:
       ip a
       
    5. Look for the bridged network interface (typically eth0 or ens33), and note the IP address.
3.4 Test Website from Host
    1. Open a browser on your host machine and navigate to the VM’s IP address:
       http://<vm-ip-address>
       
    2. You should see the HTML page you created.

Step 4: Scan the VM Using Nmap from the Host Machine
4.1 Install Nmap (If Not Already Installed)
On the host machine, install Nmap:
    • Ubuntu/Linux:
      > sudo apt install nmap -y
    • Windows: Download Nmap for Windows.
4.2 Scan the VM for Open Ports
Run an Nmap scan against the VM’s IP address from the host machine:
> nmap <vm-ip-address>
4.3 Interpret the Output
The Nmap scan will show you the open ports on the VM. Look for the following typical open ports:
    • Port 80: HTTP (Nginx web server).
    • Other ports related to services that might be running.
Example Output:
Starting Nmap 7.80 ( https://nmap.org ) at 2024-10-03 15:32 UTC
Nmap scan report for 192.168.1.10
Host is up (0.00057s latency).
Not shown: 998 closed ports
PORT    STATE SERVICE
22/tcp  open  ssh
80/tcp  open  http
MAC Address: 08:00:27:5A:3B:E4 (Oracle VirtualBox virtual NIC)

    • Port 22: SSH is open, meaning the VM allows SSH connections.
    • Port 80: HTTP is open, meaning Nginx is serving the website.

Step 5: Document the Process and Scan Results
5.1 Documentation Structure
    1. Introduction:
        ◦ Overview of the task, including the purpose of using a VM, Nginx, and Nmap.
    2. VirtualBox Installation:
        ◦ Mention the steps to install VirtualBox on the host machine.
    3. Setting up the Ubuntu VM:
        ◦ Steps to download, import, and configure the Ubuntu 22.04 VM in VirtualBox.
        ◦ Include screenshots of network settings.
    4. Installing and Configuring Nginx:
        ◦ Steps to install and configure Nginx on the VM.
        ◦ Steps to create and host a simple HTML page.
    5. Scanning the VM Using Nmap:
        ◦ Instructions for installing Nmap on the host machine.
        ◦ The command used for scanning the VM.
        ◦ Example output from Nmap showing the open ports.
    6. Observations:
        ◦ Mention the ports that were open (e.g., port 80 for HTTP).
        ◦ Include any other insights, such as security implications of open ports.

Example Nmap Scan Report Documentation
Nmap Scan Results:
    • Target: Ubuntu VM at IP 192.168.1.10
    • Host Status: Host is up
    • Open Ports:
        ◦ 22/tcp: SSH
        ◦ 80/tcp: HTTP (Nginx web server)
These open ports indicate that the VM is accessible over SSH, and the website hosted on Nginx is reachable via HTTP.

Step 6: Final Notes
    • Ensure that your host and VM are on the same network and that no firewalls are blocking the connection.
    • Double-check the network settings if the host cannot communicate with the VM.
Output image:
