# Problem statement
We need to write code that will query the **meta data** of an instance within **AWS/Azure/GCP** and provide a JSON formatted output. The choice of language and implementation is up to you.

# Answer of this Statement

Instance metadata is available for running VMs created/managed using Azure Resource Manager. Access all data categories for an instance use the following URI.
On a linux terminal

curl -H Metadata:true --noproxy "*" "http://169.254.169.254/metadata/instance?api-version=2021-09-01"  

  

