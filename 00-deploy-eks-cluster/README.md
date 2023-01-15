# deploy-eks-cluster

This example is based on eksctl which is a simple CLI tool for creating and managing clusters on EKS.EKS Clusters can be deployed and managed with a number of solutions including Terraform, Cloudformation,AWS Console and AWS CLI.

## Prerequisites

- An active AWS account
- VPC - eksctl creates a new vpc named eksctl-my-demo-cluster-cluster/VPC in the target region
- IAM permissions – The IAM security principal that you're using must have permissions to work with Amazon EKS IAM roles and service-linked roles, AWS CloudFormation, and a VPC and related resources.
- Install kubectl,eksctl and AWS CLI in your local machine or in the CICD setup

### IAM Setup

- For this setup, create an IAM policy name AmazonEKSAdminPolicy with the policy details in `01-AmazonEKSAdminPolicy.json` and attach the policy to the principal

### Creating an EKS cluster

The eksctl tool uses CloudFormation under the hood, creating one stack for the EKS master control plane and another stack for the worker nodes.

Run the command below to create a new cluster in the `us-west-2` region; expect this to take around 20 minutes and refer to eksctl documentation for all available options to customize the cluster configurations.

	eksctl create cluster \
    --version 1.23 \
    --region us-west-2 \
    --node-type t3.medium \
    --nodes 3 \
    --nodes-min 1 \
    --nodes-max 4 \
 	--name my-demo-cluster 

As an alternative, you can also use YAML, as sort of DSL (domain specific language) script for creating Kubernetes clusters with EKS.

Run the below command to create the cluster (expect this to take around 20 minutes):
		
    eksctl create cluster -f 02-demo-cluster.yaml

Sample log:

```
Chimbus-MacBook-Pro:eks chimbu$     eksctl create cluster -f 02-demo-cluster.yaml
2023-01-15 10:52:38 [ℹ]  eksctl version 0.124.0-dev+ac917eb50.2022-12-23T08:05:44Z
2023-01-15 10:52:38 [ℹ]  using region us-west-2
2023-01-15 10:52:39 [ℹ]  setting availability zones to [us-west-2c us-west-2a us-west-2b]
2023-01-15 10:52:39 [ℹ]  subnets for us-west-2c - public:192.168.0.0/19 private:192.168.96.0/19
2023-01-15 10:52:39 [ℹ]  subnets for us-west-2a - public:192.168.32.0/19 private:192.168.128.0/19
2023-01-15 10:52:39 [ℹ]  subnets for us-west-2b - public:192.168.64.0/19 private:192.168.160.0/19
2023-01-15 10:52:40 [ℹ]  nodegroup "my-demo-workers" will use "ami-0d453cab46e7202b2" [AmazonLinux2/1.23]
2023-01-15 10:52:40 [ℹ]  using Kubernetes version 1.23
2023-01-15 10:52:40 [ℹ]  creating EKS cluster "my-demo-cluster" in "us-west-2" region with un-managed nodes
2023-01-15 10:52:40 [ℹ]  1 nodegroup (my-demo-workers) was included (based on the include/exclude rules)
2023-01-15 10:52:40 [ℹ]  will create a CloudFormation stack for cluster itself and 1 nodegroup stack(s)
2023-01-15 10:52:40 [ℹ]  will create a CloudFormation stack for cluster itself and 0 managed nodegroup stack(s)
2023-01-15 10:52:40 [ℹ]  if you encounter any issues, check CloudFormation console or try 'eksctl utils describe-stacks --region=us-west-2 --cluster=my-demo-cluster'
2023-01-15 10:52:40 [ℹ]  Kubernetes API endpoint access will use default of {publicAccess=true, privateAccess=false} for cluster "my-demo-cluster" in "us-west-2"
2023-01-15 10:52:40 [ℹ]  CloudWatch logging will not be enabled for cluster "my-demo-cluster" in "us-west-2"
2023-01-15 10:52:40 [ℹ]  you can enable it with 'eksctl utils update-cluster-logging --enable-types={SPECIFY-YOUR-LOG-TYPES-HERE (e.g. all)} --region=us-west-2 --cluster=my-demo-cluster'
2023-01-15 10:52:40 [ℹ]
2 sequential tasks: { create cluster control plane "my-demo-cluster",
    2 sequential sub-tasks: {
        wait for control plane to become ready,
        create nodegroup "my-demo-workers",
    }
}
2023-01-15 10:52:40 [ℹ]  building cluster stack "eksctl-my-demo-cluster-cluster"
2023-01-15 10:52:42 [ℹ]  deploying stack "eksctl-my-demo-cluster-cluster"
2023-01-15 10:53:12 [ℹ]  waiting for CloudFormation stack "eksctl-my-demo-cluster-cluster"
2023-01-15 10:53:44 [ℹ]  waiting for CloudFormation stack "eksctl-my-demo-cluster-cluster"
2023-01-15 10:54:45 [ℹ]  waiting for CloudFormation stack "eksctl-my-demo-cluster-cluster"
2023-01-15 10:55:46 [ℹ]  waiting for CloudFormation stack "eksctl-my-demo-cluster-cluster"
2023-01-15 10:56:47 [ℹ]  waiting for CloudFormation stack "eksctl-my-demo-cluster-cluster"
2023-01-15 10:57:48 [ℹ]  waiting for CloudFormation stack "eksctl-my-demo-cluster-cluster"
2023-01-15 10:58:49 [ℹ]  waiting for CloudFormation stack "eksctl-my-demo-cluster-cluster"
2023-01-15 10:59:50 [ℹ]  waiting for CloudFormation stack "eksctl-my-demo-cluster-cluster"
2023-01-15 11:00:51 [ℹ]  waiting for CloudFormation stack "eksctl-my-demo-cluster-cluster"
2023-01-15 11:01:53 [ℹ]  waiting for CloudFormation stack "eksctl-my-demo-cluster-cluster"
2023-01-15 11:02:54 [ℹ]  waiting for CloudFormation stack "eksctl-my-demo-cluster-cluster"
2023-01-15 11:03:55 [ℹ]  waiting for CloudFormation stack "eksctl-my-demo-cluster-cluster"
2023-01-15 11:06:02 [ℹ]  building nodegroup stack "eksctl-my-demo-cluster-nodegroup-my-demo-workers"
2023-01-15 11:06:04 [ℹ]  deploying stack "eksctl-my-demo-cluster-nodegroup-my-demo-workers"
2023-01-15 11:06:04 [ℹ]  waiting for CloudFormation stack "eksctl-my-demo-cluster-nodegroup-my-demo-workers"
2023-01-15 11:06:35 [ℹ]  waiting for CloudFormation stack "eksctl-my-demo-cluster-nodegroup-my-demo-workers"
2023-01-15 11:07:11 [ℹ]  waiting for CloudFormation stack "eksctl-my-demo-cluster-nodegroup-my-demo-workers"
2023-01-15 11:07:50 [ℹ]  waiting for CloudFormation stack "eksctl-my-demo-cluster-nodegroup-my-demo-workers"
2023-01-15 11:08:36 [ℹ]  waiting for CloudFormation stack "eksctl-my-demo-cluster-nodegroup-my-demo-workers"
2023-01-15 11:09:15 [ℹ]  waiting for CloudFormation stack "eksctl-my-demo-cluster-nodegroup-my-demo-workers"
2023-01-15 11:09:15 [ℹ]  waiting for the control plane to become ready
2023-01-15 11:09:16 [✔]  saved kubeconfig as "/Users/chimbu/.kube/config"
2023-01-15 11:09:16 [ℹ]  no tasks
2023-01-15 11:09:16 [✔]  all EKS cluster resources for "my-demo-cluster" have been created
2023-01-15 11:09:17 [ℹ]  adding identity "arn:aws:iam::317630533282:role/eksctl-my-demo-cluster-nodegroup-NodeInstanceRole-14J48FWWCMCO7" to auth ConfigMap
2023-01-15 11:09:17 [ℹ]  nodegroup "my-demo-workers" has 0 node(s)
2023-01-15 11:09:17 [ℹ]  waiting for at least 1 node(s) to become ready in "my-demo-workers"
2023-01-15 11:10:05 [ℹ]  nodegroup "my-demo-workers" has 4 node(s)
2023-01-15 11:10:05 [ℹ]  node "ip-192-168-51-22.us-west-2.compute.internal" is ready
2023-01-15 11:10:05 [ℹ]  node "ip-192-168-62-41.us-west-2.compute.internal" is not ready
2023-01-15 11:10:05 [ℹ]  node "ip-192-168-8-29.us-west-2.compute.internal" is not ready
2023-01-15 11:10:05 [ℹ]  node "ip-192-168-84-235.us-west-2.compute.internal" is not ready
2023-01-15 11:10:07 [ℹ]  kubectl command should work with "/Users/chimbu/.kube/config", try 'kubectl get nodes'
2023-01-15 11:10:07 [✔]  EKS cluster "my-demo-cluster" in "us-west-2" region is ready
Chimbus-MacBook-Pro:eks chimbu$
```

<img width="1512" alt="Screenshot 2023-01-15 at 11 12 17" src="https://user-images.githubusercontent.com/112865563/212537419-fbbc301a-6e00-4926-bbb5-f715a8ee0d54.png">

Run the below command to destroy the cluster (expect this to take around 20 minutes):
		
    eksctl delete cluster -f 02-demo-cluster.yaml