## GADS2020-PROJECT_PRACTICE_SUBMISSION



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


## LAB_TRANSLATION(CONSOLE INSTRUCTIONS TO COMMAND LINE)

# VPC Networks Lab Translation

// creating a vpc network named mynetwork //

gcloud compute networks create mynetwork --project=qwiklabs-gcp-04-00b7439920ab --subnet-mode=auto --bgp-routing-mode=regional

// creating firewall rules //

gcloud compute firewall-rules create mynetwork-allow-icmp --project=qwiklabs-gcp-04-00b7439920ab --network=projects/qwiklabs-gcp-04-00b7439920ab/global/networks/mynetwork --description=Allows\ ICMP\ connections\ from\ any\ source\ to\ any\ instance\ on\ the\ network. --direction=INGRESS --priority=65534 --source-ranges=0.0.0.0/0 --action=ALLOW --rules=icmp

gcloud compute firewall-rules create mynetwork-allow-internal --project=qwiklabs-gcp-04-00b7439920ab --network=projects/qwiklabs-gcp-04-00b7439920ab/global/networks/mynetwork --description=Allows\ connections\ from\ any\ source\ in\ the\ network\ IP\ range\ to\ any\ instance\ on\ the\ network\ using\ all\ protocols. --direction=INGRESS --priority=65534 --source-ranges=10.128.0.0/9 --action=ALLOW --rules=all

gcloud compute firewall-rules create mynetwork-allow-rdp --project=qwiklabs-gcp-04-00b7439920ab --network=projects/qwiklabs-gcp-04-00b7439920ab/global/networks/mynetwork --description=Allows\ RDP\ connections\ from\ any\ source\ to\ any\ instance\ on\ the\ network\ using\ port\ 3389. --direction=INGRESS --priority=65534 --source-ranges=0.0.0.0/0 --action=ALLOW --rules=tcp:3389

gcloud compute firewall-rules create mynetwork-allow-ssh --project=qwiklabs-gcp-04-00b7439920ab --network=projects/qwiklabs-gcp-04-00b7439920ab/global/networks/mynetwork --description=Allows\ TCP\ connections\ from\ any\ source\ to\ any\ instance\ on\ the\ network\ using\ port\ 22. --direction=INGRESS --priority=65534 --source-ranges=0.0.0.0/0 --action=ALLOW --rules=tcp:22


// creating a vm instance in us-central1 named mynet-us-vm //

gcloud beta compute --project=qwiklabs-gcp-04-00b7439920ab instances create mynet-us-vm --zone=us-central1-c --machine-type=n1-standard-1 --subnet=mynetwork --network-tier=PREMIUM --maintenance-policy=MIGRATE --service-account=737065806886-compute@developer.gserviceaccount.com --scopes=https://www.googleapis.com/auth/devstorage.read_only,https://www.googleapis.com/auth/logging.write,https://www.googleapis.com/auth/monitoring.write,https://www.googleapis.com/auth/servicecontrol,https://www.googleapis.com/auth/service.management.readonly,https://www.googleapis.com/auth/trace.append --image=debian-9-stretch-v20200805 --image-project=debian-cloud --boot-disk-size=10GB --boot-disk-type=pd-standard --boot-disk-device-name=mynet-us-vm --reservation-affinity=any


// creating a vm instance in europe-west1 named mynet-eu-vm //

gcloud beta compute --project=qwiklabs-gcp-04-00b7439920ab instances create mynet-eu-vm --zone=europe-west1-c --machine-type=n1-standard-1 --subnet=mynetwork --network-tier=PREMIUM --maintenance-policy=MIGRATE --service-account=737065806886-compute@developer.gserviceaccount.com --scopes=https://www.googleapis.com/auth/devstorage.read_only,https://www.googleapis.com/auth/logging.write,https://www.googleapis.com/auth/monitoring.write,https://www.googleapis.com/auth/servicecontrol,https://www.googleapis.com/auth/service.management.readonly,https://www.googleapis.com/auth/trace.append --image=debian-9-stretch-v20200805 --image-project=debian-cloud --boot-disk-size=10GB --boot-disk-type=pd-standard --boot-disk-device-name=mynet-eu-vm --reservation-affinity=any





// creating a customized network named managementnet //

gcloud compute networks create managementnet --project=qwiklabs-gcp-04-00b7439920ab --subnet-mode=custom --bgp-routing-mode=regional

gcloud compute networks subnets create managementsubnet-us --project=qwiklabs-gcp-04-00b7439920ab --range=10.130.0.0/20 --network=managementnet --region=us-central1




// creating firewall rules for managementnet //


gcloud compute --project=qwiklabs-gcp-04-00b7439920ab firewall-rules create managementnet-allow-icmp-ssh-rdp --direction=INGRESS --priority=1000 --network=managementnet --action=ALLOW --rules=tcp:22,tcp:3389,icmp --source-ranges=0.0.0.0/0


// creating management vm instance //


gcloud beta compute --project=qwiklabs-gcp-04-00b7439920ab instances create managementnet-us-vm --zone=us-central1-c --machine-type=f1-micro --subnet=managementsubnet-us --network-tier=PREMIUM --maintenance-policy=MIGRATE --service-account=737065806886-compute@developer.gserviceaccount.com --scopes=https://www.googleapis.com/auth/devstorage.read_only,https://www.googleapis.com/auth/logging.write,https://www.googleapis.com/auth/monitoring.write,https://www.googleapis.com/auth/servicecontrol,https://www.googleapis.com/auth/service.management.readonly,https://www.googleapis.com/auth/trace.append --image=debian-9-stretch-v20200805 --image-project=debian-cloud --boot-disk-size=10GB --boot-disk-type=pd-standard --boot-disk-device-name=managementnet-us-vm --reservation-affinity=any

