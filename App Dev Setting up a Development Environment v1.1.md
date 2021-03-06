# App Dev: Setting up a Development Environment v1.1

## Task 1: Creating a Compute Engine Virtual Machine Instance
- Place VM name, region, and zone in environment variables
	- `export vm_name=dev-instance`
	- `export vm_region=us-central1`
	- `export vm_zone=us-central1-a`
- Create a VM with full access scope 
	- `gcloud beta compute instances create $vm_name --region $vm_region --zone $vm_zone --scopes=https://www.googleapis.com/auth/cloud-platform --tags http-server`
- Configure firewall to grant VM access
	- `gcloud compute firewall-rules create allow-http-ingress --zone $vm_zone --region $vm_region --allow tcp:80 --direction INGRESS --source-ranges 0.0.0.0/0 --target-tags=http-server`
- SSH into VM
	- `gcloud compute ssh $vm_name --zone $vm_zone `

## Task 2: Install software on the VM instance

- Update the debian linux OS
	- `sudo apt-get update`
- Install git
	- `sudo apt-get install git`
- Download and execute node js setup script
	- `curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -`
- Install node js
	- `sudo apt install nodejs`

## Task 3: Configure the VM to Run Application Software

- Check version of node js
	- `node -v`
- Clone git Google repo
	- `git clone https://github.com/GoogleCloudPlatform/training-data-analyst`
- Change working directory
	- `cd ~/training-data-analyst/courses/developingapps/nodejs/devenv/`
- Run simple web server
	- `sudo node server/app.js`
- Stop appication from executing
	- `Ctrl+C`
- Install node js library for compute engine
	- `npm install`
- Run node js application that lists GCE instances
	- `node list-gce-instances.js`