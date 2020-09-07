# GADS2020-PROJECT_PRACTICE_SUBMISSION



<details>
 <summary>Lab 1: VPC NETWORKS</summary>
  
  <img src="qwiklab_shots/VPC_Networking.PNG">
</details>


<details>
 <summary>Lab 2: GETTING STARTED WITH GKE</summary>
  
  <img src="qwiklab_shots/GCP_Getting_started_with_gke.PNG">
</details>

<details>
 <summary>Lab 3: GETTING STARTED WITH COMPUTE ENGINE</summary>
  
  <img src="qwiklab_shots/GCP_Compute_Engine.PNG">
</details>

<details>
 <summary>Lab 4: SETTING UP A DEV ENVIROMEMENT 1.1</summary>
  
  <img src="qwiklab_shots/GCP_Setting up a dev env.PNG">
</details>


<details>
 <summary>Lab 5: INFRASTRUCTURE PREVIEW</summary>
  
  <img src="qwiklab_shots/Infrastructure Preview.PNG">
</details>

<details>
 <summary>Lab 6: GETTING STARTED WITH APP ENGINE</summary>
  
  <img src="qwiklab_shots/GCP_ App Engine.PNG">
</details>

<details>
 <summary>Lab 7: GOOGLE CLOUD MARKET PLACE</summary>
  
 <img src="qwiklab_shots/GCP_Cloud mkt_place.PNG">
</details>

<details>
 <summary>Lab 8: CLOUD STORAGE & SQL</summary>
  
 <img src="qwiklab_shots/GCP_Cloud Storage & SQL.PNG">
</details>

<details>
 <summary>Lab 9: CONSOLE AND CLOUD SHELL</summary>
  
 <img src="qwiklab_shots/Console & Cloud shell.PNG">
</details>


<details>
 <summary>Lab 10: CLOUD SQL & DATAPROC</summary>
  
 <img src="qwiklab_shots/CLOUD SQL & DATAPROC.PNG">
</details>

<details>
 <summary>Lab 11: Classify Images with Pre-built ML Models using Cloud Vision API and AutoML.</summary>
  
 <img src="qwiklab_shots/Classify Images with Pre-built ML Models using Cloud Vision API and AutoML..PNG">
</details>


# LAB_TRANSLATION(CONSOLE INSTRUCTIONS TO COMMAND LINE)

### VPC Networks Lab Translation

```



// creating a vpc network named <mynetwork> //

gcloud compute networks create mynetwork --project=gads-proj-1 --subnet-mode=auto --bgp-routing-mode=regional

// creating firewall rules //

gcloud compute firewall-rules create mynetwork-allow-icmp --project=gads-proj-1 --network=mynetwork --description=Allows\ ICMP\  --direction=INGRESS --priority=65534 --source-ranges=0.0.0.0/0 --action=ALLOW --rules=icmp

gcloud compute firewall-rules create mynetwork-allow-internal --project=gads-proj-1 --network=mynetwork --description=Allows\ all\ protocols. --direction=INGRESS --priority=65534 --source-ranges=10.128.0.0/9 --action=ALLOW --rules=all

gcloud compute firewall-rules create mynetwork-allow-rdp --project=gads-proj-1 --network=mynetwork --description=Allows\ RDP\--direction=INGRESS --priority=65534 --source-ranges=0.0.0.0/0 --action=ALLOW --rules=tcp:3389

gcloud compute firewall-rules create mynetwork-allow-ssh --project=gads-proj-1 --network=mynetwork --description=Allows\ TCP\ --direction=INGRESS --priority=65534 --source-ranges=0.0.0.0/0 --action=ALLOW --rules=tcp:22


// creating a vm instance in us-central1 named mynet-us-vm //

gcloud beta compute --project=gads-proj-1 instances create mynet-us-vm --zone=us-central1-c --machine-type=n1-standard-1 --subnet=mynetwork --network-tier=PREMIUM --maintenance-policy=MIGRATE 
--image=debian-9-stretch-v20200805 --image-project=debian-cloud --boot-disk-size=10GB --boot-disk-type=pd-standard --boot-disk-device-name=mynet-us-vm --reservation-affinity=any


// creating a vm instance in europe-west1 named mynet-eu-vm //

gcloud beta compute --project=gads-proj-1 instances create mynet-eu-vm --zone=europe-west1-c --machine-type=n1-standard-1 --subnet=mynetwork --network-tier=PREMIUM --maintenance-policy=MIGRATE 
--image=debian-9-stretch-v20200805 --image-project=debian-cloud --boot-disk-size=10GB --boot-disk-type=pd-standard --boot-disk-device-name=mynet-eu-vm --reservation-affinity=any





// creating a customized network named managementnet //

gcloud compute networks create managementnet --project=gads-proj-1 --subnet-mode=custom --bgp-routing-mode=regional

gcloud compute networks subnets create managementsubnet-us --project=gads-proj-1 --range=10.130.0.0/20 --network=managementnet --region=us-central1




// creating firewall rules for managementnet //


gcloud compute --project=gads-proj-1 firewall-rules create managementnet-allow-icmp-ssh-rdp --direction=INGRESS --priority=1000 --network=managementnet --action=ALLOW --rules=tcp:22,tcp:3389,icmp --source-ranges=0.0.0.0/0


// creating management vm instance //


gcloud beta compute --project=gads-proj-1 instances create managementnet-us-vm --zone=us-central1-c --machine-type=f1-micro --subnet=managementsubnet-us --network-tier=PREMIUM --maintenance-policy=MIGRATE 
--image=debian-9-stretch-v20200805 --image-project=debian-cloud --boot-disk-size=10GB --boot-disk-type=pd-standard --boot-disk-device-name=managementnet-us-vm --reservation-affinity=any

```

### Creating a VM using cloud shell

```

//  IN THE CLOUD SHELL //

// To display a list of all the zones in the region enter the following cmd //

	gcloud compute zones list 

// To chose a zone in this case we are chosing zone us-central1-b //

	gcloud config set compute/zone us-central1-b

// To create a VM instance called my-vm-2 in that zone, execute this command //


	gcloud compute instances create "my-vm-2" \
	--machine-type "n1-standard-1" \
	--image-project "debian-cloud" \
	--image "debian-9-stretch-v20190213" \
	--subnet "default"


// To check connectivity between the 2 vms use the ping cmd in an SSH session //

	ping my-vm-1
 
 ```
 
 ## Setting up developer enviroment Lab translation
```
// creating a vm instance //

gcloud beta compute --project=setting_dev_env instances create dev-instance 
--zone=us-central1-a 
--machine-type=e2-medium 
--subnet=default 
--network-tier=PREMIUM 
--maintenance-policy=MIGRATE 
--tags=http-server 
--image=debian-9-stretch-v20200805 
--image-project=debian-cloud 
--boot-disk-size=10GB 
--boot-disk-type=pd-standard 
--boot-disk-device-name=dev-instance 
--reservation-affinity=any

// firewall rules //

gcloud compute --project=setting_dev_env firewall-rules create default-allow-http --direction=INGRESS --priority=1000 
--network=default --action=ALLOW --rules=tcp:80 --source-ranges=0.0.0.0/0 --target-tags=http-server



*// in the ssh session to update the deb package //*

	sudo apt-get update


// to install git //

	sudo apt-get install git


// to download node //

	curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -




// to install npm and node //

	sudo apt install nodejs


// To check the version of Node.js, execute the following command //

	node -v


// To clone the class repository, execute the following command //

	git clone https://github.com/GoogleCloudPlatform/training-data-analyst


// to change the working directory //

	cd ~/training-data-analyst/courses/developingapps/nodejs/devenv/

// to run the webserver //

	sudo node server/app.js


// To install the Node.js library for Compute Engine //

	npm install


// To run a simple Node.js application that lists Compute Engine instances //

	node list-gce-instances.js
```
