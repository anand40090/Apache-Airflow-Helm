# Apache-Airflow installation on Ubuntu OS with Helm Chart

1. Install The Prerequsite Tools 

```
1. Install AWS CLI2 >> This is used to spin the EKS cluster on AWS cloud

curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip" 
sudo apt install unzip
unzip awscliv2.zip 
sudo ./aws/install
aws --version

2. Install and Setup Kubectl >> This is used to interact with Kubernetes cluster

curl -LO "https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin
kubectl version

3. Install and Setup eksctl >> This is used to create and manage EKS cluster 

curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin
eksctl version

4. Install Helm chart and Airflow helm repository

curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh

```
_________________

2. Configure AWS cli with the AWS-USER Secrete & Private key

```
1. Sign in to the AWS Management Console: Sign in to the AWS Management Console using your AWS account credentials.
2. Open the IAM Console: Navigate to the IAM (Identity and Access Management) console.
3. Create IAM User:
Click on "Users" in the left navigation pane.
Click on "Add user".
Enter a username for the IAM user.
Select "Programmatic access" as the access type.
4. Set Permissions:
Attach policies that grant the necessary permissions for working with Amazon EKS. At a minimum, the user should have permissions to interact with EKS resources. You can either choose existing policies or create custom policies based on your requirements.

5. Create the User: Click on "Create user".
6. Access Key Creation:
After the user is created, you'll be presented with a success screen. Here, you can create an access key for the user.
Click on "Create access key".
Make sure to copy the Access Key ID and Secret Access Key as you will need them to configure the AWS CLI.

7. Configure AWS CLI:

Install the AWS CLI on your local machine if you haven't already.
Configure the AWS CLI with the access key and secret access key of the IAM user you just 
```

Output - 

![image](https://github.com/anand40090/Apache-Airflow-Helm/assets/32446706/ab782798-771f-4bd7-9fc6-dcdd875bfe25)

_____

3. Create EKS cluster for this Lab 

```
As we have already installed and configured AWS-CLI and eksctl, now we can create EKS cluster from the aws EC2 system

Run below mentioned command to create EKS cluster, it will take 15 minutes to create and spin the cluster

eksctl create cluster --name eks2 --version 1.24 --region ap-south-1 --nodegroup-name worker-nodes --node-type t2.large --nodes 2 --nodes-min 2 --nodes-max 3
aws eks update-kubeconfig --name eks4



```

Output - 

![image](https://github.com/anand40090/Apache-Airflow-Helm/assets/32446706/4f809a9c-544e-46ab-86fc-40dac3906128)

--------

4. Install Apache Airflow using Helm Chart

````
1. Create values.yaml file with "KubernetesExecutor" executor to support EKS cluster

executor: "KubernetesExecutor"
scheduler:
  extraVolumes:
    - name: dags
      hostPath:
        path: /home/admin1/Airflow/Dag           ##This is the local path of your system where DAG files are stored
  extraVolumeMounts:
    - name: dags
      mountPath: /home/admin1/Airflow/Dag        ##This is the local path of your system where DAG files are stored 


2. Installing the Chart

helm repo add apache-airflow https://airflow.apache.org
## This will add the apache-airflow helm chart repository

helm upgrade --install airflow apache-airflow/airflow --namespace airflow --create-namespace -f values.yaml
## This will install the apache-airflow using helmchart with description mentioned in the values.yaml file and it will create kubernetes namespace airflow 

-----

Output -

![image](https://github.com/anand40090/Apache-Airflow-Helm/assets/32446706/9f8e46d3-2b8b-4aad-ad94-59a91449fcf5)

![image](https://github.com/anand40090/Apache-Airflow-Helm/assets/32446706/8c87e492-f5c5-4b60-9c01-afbd08261971)


____________________________________

Now, you can access Airflow UI by using kubectl port-forward:

```
kubectl port-forward svc/airflow-webserver 8080:8080 -n airflow
```

Output -

![image](https://github.com/anand40090/Apache-Airflow-Helm/assets/32446706/ab0d4520-8f49-4ee5-8034-f2ccd2f768b1)


