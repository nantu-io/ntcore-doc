## <b>Production</b> <!-- {docsify-ignore} -->

## Prerequisites
### On-Premises
On-Premises is suitable for you if you're experienced in Kubernetes.
1. **Kubernetes**: A Kubernetes cluster is required to run NTCore in production. Kubernetes is an open source container orchestration engine for automating deployment, scaling, and management of containerized applications. You can setup a Kubernetes cluster manually following this [guidance](https://kubernetes.io/docs/setup/production-environment/).

2. **kubectl**: kubectl is the Kubenetes command line tool that allows you to run commands against Kubernetes clusters. Follow this [guidance](https://kubernetes.io/docs/tasks/tools/) to install kubectl, and this [guidance](https://kubernetes.io/docs/tasks/access-application-cluster/configure-access-multiple-clusters/) to configure kubectl to communicate with your Kubernetes cluster. Run the below command to confirm if kubectl is configured properly.
```
kubectl version
```

3. **PostgreSQL**: NTCore uses Postgres SQL to store data in production. Download and install Postgres SQL database following this [guidance](https://www.postgresql.org/download/).

### Amazon EKS
AWS provides the Elastic Kubernetes Servie (Amazon EKS) to run and scale Kubernetes applications. There're different ways to setup EKS cluster. This tutorial goes through the steps using eksctl which is a simple tool.
1. **kubectl**: A command line tool to work with Kubernetes clusters. Follow this [guidance](https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html) to install kubectl.
2. **eksctl**: A command line tool to create and manage EKS clusters. Follow this [guidance](https://docs.aws.amazon.com/eks/latest/userguide/eksctl.html) to install eksctl.
3. **awscli**: A unified tool to manage AWS services. Follow this [guidance](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html) to install awscli.
4. **IAM permissions**: IAM permissions are required for eksctl to manage AWS resources for EKS clusters. 
    1. Login your AWS account and go to the IAM service console.
    2. In the Policies section under Access Management, create new policies as below if they don't already exist.

    <details>
    <summary>AmazonEC2FullAccess (AWS Managed Policy)</summary>
    <p>

    ```
    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Action": "ec2:*",
                "Effect": "Allow",
                "Resource": "*"
            },
            {
                "Effect": "Allow",
                "Action": "elasticloadbalancing:*",
                "Resource": "*"
            },
            {
                "Effect": "Allow",
                "Action": "cloudwatch:*",
                "Resource": "*"
            },
            {
                "Effect": "Allow",
                "Action": "autoscaling:*",
                "Resource": "*"
            },
            {
                "Effect": "Allow",
                "Action": "iam:CreateServiceLinkedRole",
                "Resource": "*",
                "Condition": {
                    "StringEquals": {
                        "iam:AWSServiceName": [
                            "autoscaling.amazonaws.com",
                            "ec2scheduled.amazonaws.com",
                            "elasticloadbalancing.amazonaws.com",
                            "spot.amazonaws.com",
                            "spotfleet.amazonaws.com",
                            "transitgateway.amazonaws.com"
                        ]
                    }
                }
            }
        ]
    }
    ```

    </p>
    </details>

    <details>
    <summary>AWSCloudFormationFullAccess (AWS Managed Policy)</summary>
    <p>

    ```
    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Effect": "Allow",
                "Action": [
                    "cloudformation:*"
                ],
                "Resource": "*"
            }
        ]
    }
    ```

    </p>
    </details>

    <details>
    <summary>EKSAllAccess</summary>
    <p>

    ```
    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Effect": "Allow",
                "Action": "eks:*",
                "Resource": "*"
            },
            {
                "Action": [
                    "ssm:GetParameter",
                    "ssm:GetParameters"
                ],
                "Resource": "*",
                "Effect": "Allow"
            },
            {
                "Action": [
                    "kms:CreateGrant",
                    "kms:DescribeKey"
                ],
                "Resource": "*",
                "Effect": "Allow"
            }
        ]
    }
    ```

    </p>
    </details>

    <details>
    <summary>IAMLimitedAccess</summary>
    <p>

    ```
    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Effect": "Allow",
                "Action": [
                    "iam:CreateInstanceProfile",
                    "iam:DeleteInstanceProfile",
                    "iam:GetInstanceProfile",
                    "iam:RemoveRoleFromInstanceProfile",
                    "iam:GetRole",
                    "iam:CreateRole",
                    "iam:DeleteRole",
                    "iam:AttachRolePolicy",
                    "iam:PutRolePolicy",
                    "iam:ListInstanceProfiles",
                    "iam:AddRoleToInstanceProfile",
                    "iam:ListInstanceProfilesForRole",
                    "iam:PassRole",
                    "iam:DetachRolePolicy",
                    "iam:DeleteRolePolicy",
                    "iam:GetRolePolicy",
                    "iam:GetOpenIDConnectProvider",
                    "iam:CreateOpenIDConnectProvider",
                    "iam:DeleteOpenIDConnectProvider",
                    "iam:TagOpenIDConnectProvider",
                    "iam:ListAttachedRolePolicies",
                    "iam:TagRole"
                ],
                "Resource": "*"
            },
            {
                "Effect": "Allow",
                "Action": [
                    "iam:GetRole"
                ],
                "Resource": "*"
            },
            {
                "Effect": "Allow",
                "Action": [
                    "iam:CreateServiceLinkedRole"
                ],
                "Resource": "*",
                "Condition": {
                    "StringEquals": {
                        "iam:AWSServiceName": [
                            "eks.amazonaws.com",
                            "eks-nodegroup.amazonaws.com",
                            "eks-fargate.amazonaws.com"
                        ]
                    }
                }
            }
        ]
    }
    ```

    </p>
    </details>

    3. In the Users section under Access Management, create a new user with Access key credential type and attach the 4 policies above to this user. Keep a note of the access_key and access_key_id.
    4. Specify the user profile in `~/.aws/credentials` (Linux & Mac) or `%USERPROFILE%\.aws\credentials` (Windows) for awscli as below.
    ```
    [default]
    aws_access_key_id={your_access_key_id}
    aws_secret_access_key={your_access_key}
    ```

5. Create an EKS cluster. You can create a simple EKS cluster with 1 node with the provided config as below.
```
eksctl create cluster -f cloud/aws/eksctl.yml
```
You can find more examples about how to create EKS clusters in the eksctl [introduction](https://eksctl.io/introduction/) and [github](https://eksctl.io/introduction/).

6. Verify kubectl is configured properly to interact with EKS cluster with the below command.
```
kubectl version
```
7. **PostgreSQL**: Follow this [guidance](https://aws.amazon.com/getting-started/hands-on/create-connect-postgresql-db/) to create a PostgreSQL database with Amazon RDS.

## Installation
1. Download NTCore repository from github.
```
git clone https://github.com/nantutech/ntcore.git
```
2. Provide the PostgreSQL config in <ins>kube/app-config.yml</ins> as below.
```
host: <postgres_host>
port: <postgres_port>
user: <postgres_user>
database: <postgres_database>
password: <postgres_password>
```

3. <ins>Inside</ins> the NTCore repository, run the below command.
```
kubectl apply -f kube/custom-resources.yml
kubectl apply -f kube/
```
4. Run the below command to find out the hostname of ntcore (See the EXTERNAL-IP column).
```
kubectl get service/traefik | awk {'print $1" " $2 " " $4 " " $5'} | column -t
``` 

5. Launch NTCore from your browser at the below url.
```
http://{hostname}:8000/dsp/console/workspaces
```