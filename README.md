# NextCloud SaaS Project

## Objective

Set up a cloud storage SaaS using NextCloud. The software must be accessible through the web and should be able to be toggled on and off through the Proxmox VE server set up through Proxmox. Then configure accounts registered by clients that sign up for the service.

### Skills Learned

- Assigning and configure DNS records through Cloudflare.
- Hosting and manage a cloud storage SaaS with active clients.
- Troubleshoot and resolve issues happening in real-time.
- Encrypt and secure a domain to protect client data.
- Enhancing competence in Linux Terminal Operations.

### Tools Used

- Proxmox to set up containers to run and install Nextcloud and Cloudflare.
- Nextcloud to host as a cloud storage service.
- Cloudflare to register a domain and set up tunnels to connect to the container created in Proxmox.

## Steps
Installing and Configuring NextCloud

1. Set up a container using the Debian 11.7 standard edition template.
2. Name the Container "NextCloud" and set a password for the container
3. Allocate 8 gigabytes of disk space, 2 gigabytes of random access memory, and have one core for the processor. 
4. Set the IP address to be assigned by your DHCP server, then configure the DNS domain as "local" and DNS server as 8.8.8.8 (Google's DNS). Click through the menu can create the container afterward.
5. After creating the container click "Console" and log in. The user name should be "root" and password should be whatever you put for the container.
6. Next you should presented with a GUI, set a password for the MySQL on, you then have to confirm right after (make sure to follow the password guidelines). Then repeat for the process for the admin account for NextCloud.
7. The next page will ask for a domain to serve NextCloud, just input the IP that was given to it through the DHCP service
8. Skip through the the rest of the menus until you get to the install screen. Click Install.
9. After the security updates install another screen will prompted with the IP address of the NextCloud server.
10. Access your router via web browser go into DHCP settings, search for NextCloud or anything with the same IP address with your next cloud server and allocate that IP address to the server. (This sets the IP static meaning it will never change)
11. Open the web browser and type the IP address of the NextCloud server in and you should log into the admin address. 

Setting up secure tunnels using Cloudflare
1. Go to https://www.cloudflare.com/ , and create an account with the free subscription.
2. Click on products and register a domain (prices may vary).
3. After creating your domain look at the menu on the left hand side and click "Zero Trust" and you should load into the "Zero Trust Dashboard".
4. Next you click Networks > Tunnels, then click Create tunnel> Select Cloudflared then select a name for the tunnel and click next.
5. On the install screen there will be a number of OS types. Choose Debian (the one installed on the container) and select 64-bit.
6. Then copy the code under where it says "If you don’t have cloudflared installed on your machine" and paste it into the console of the container. ```bash 
   curl -L --output cloudflared.deb https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64.deb && 

	sudo dpkg -i cloudflared.deb && 

	sudo cloudflared service install eyJhIjoiNGM0ZmM0NjQ0YmRhN2NiN2JhYzBkMjdmY2JmMzk1NDYiLCJ0IjoiYTQyZWYzZWYtMTdmOC00NDYxLWI4MmMtNDIwMzk2ODVhNGJjIiwicyI6Ik1EY3laVFl4WWpRdFpXRmpZeTAwWWpGaUxXSmtPR010TW1GaU5HUm1NVEZpWVdObSJ9
7. When you do that there should be an error saying that curl and sudo are not installed. So to install you will run: ```bash
   apt-get install curl && sudo -y
8. After installing these you should copy and paste the Cloudflare code showed on step 6 and download it to the container.
9. When the installation is finished head back to Cloudflare Zero Trust and you should see under the install code that the container is connected.
10. Click next and you'll be prompted to enter the Public Host Subdomain, Domain, Service Type and URL. For the Subdomain type in "nextcloud", for the Public Host Domain select the domain that you bought from Cloudflare, then for service type click HTTPS, and for URL put the static IP address that was set for the NextCloud server(you don't have to put the port number).
11. Click Additional Application Settings, then select TLS scroll down to "No TLS Verify" and toggle the switch on. 
12. Type in your subdomain with your domain public name, and your NextlCloud server should pop prompting the client to log in or sign up to make an account. 
