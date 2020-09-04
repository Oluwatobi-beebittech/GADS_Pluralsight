# Implement Private Google Access and Cloud NAT

## Task 1. Create the VM instance

### Create a VPC network and firewall rules

- Store VPC name in enviroment variable
	- `export vpc_name=privatenet-us`
	- `export vpc_region=us-central1`
- Create VPC for VM
	- `gcloud compute networks create $vpc_name --subnet-mode custom --region $vpc_region --range 10.130.0.0/20`
- Create firewall for VM
	- `gcloud compute firewall-rules create privatenet-allow-ssh --allow ssh:22 --direction INGRESS --target-tags --network privatenet --source-ranges 35.235.240.0/20`
- Create the VM instance with no public IP address
	- `gcloud compute instances create vm-internal --region $vpc_region --zone us-central1-c --machine-type n1-standard-1 --network privatenet --subnet privatenet-us --no-address`
- SSH to vm-internal to test the IAP tunnel
	- `gcloud compute ssh vm-internal --zone us-central1-c --tunnel-through-iap`
- Test the external connectivity of vm-internal
	- `ping -c 2 www.google.com`
	- `exit`

## Task 2. Enable Private Google Access

### Create a Cloud Storage bucket

- Command to create bucket
	- `gsutil mb  -l US gs://$DEVSHELL_PROJECT_ID`

### Copy an image file into your bucket

- `gsutil cp gs://cloud-training/gcpnet/private/access.svg gs://$DEVSHELL_PROJECT_ID`
- `gsutil ls -l gs://$DEVSHELL_PROJECT_ID`

### Access the image from VM instance

- Copy image from bucket into cloud shell
	- `gsutil cp gs://$DEVSHELL_PROJECT_ID/*.svg .`
- Connect to `vm-internal`
	- `gcloud compute ssh vm-internal --zone us-central1-c --tunnel-through-iap`
- Attempt copying image from bucket to `vm-internal`
	- `gsutil cp gs://[my_bucket]/*.svg .`
	- `Ctrl+C`

### Enable Private Google Access

- `gcloud compute networks subnets update privatenet-us --enable-private-ip-google-access`
- Connect to `vm-internal` through SSH
	- `gcloud compute ssh vm-internal --zone us-central1-c --tunnel-through-iap`
- Attempt copying image from bucket to `vm-internal` again (it should work)
	- `gsutil cp gs://[my_bucket]/*.svg .`

## Task 3. Configure a Cloud NAT gateway
