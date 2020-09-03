# AK8S-05 Upgrading Kubernetes Engine Clusters

## Task 1. Deploy a GKE cluster
	
- Configure the cluster name and zone
	`export my_cluster=cluster-1`
	export my_zone=us-central1-c

	-Deploy cluster
	gcloud container clusters create $my_cluster --zone $my_zone --enable-ip-alias --num-nodes 3 --cluster-version 1.14.10-gke.27 

## Task 2: Upgrade your GKE cluster

	- List clusters
	gcloud conainer clusters list

	- Upgrade master in cluster-1 to 1.15.11-gke.5
	gcloud container clusters upgrade $my_cluster --cluster-version 1.15.11-gke.5 --zone $my_zone --master  

	-Upgrade node pool in cluster-1 to 1.15.11-gke.5
		1. Get node pool list of cluster-1
		export node_pool_list = $(gcloud container node-pools list --cluster $my_cluster --zone $my_zone)

		2. Upgrade node pool using node pool list obtained
		gcloud container clusters upgrade $my_cluster --cluster-version 1.15.11-gke.5 --zone $my_zone --node-pool $node_pool_list
