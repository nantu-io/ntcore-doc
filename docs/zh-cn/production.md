## <b>生产部署</b> <!-- {docsify-ignore} -->

## 先决条件
### On-Premises
如果您在 Kubernetes 方面有经验，则 On-Premises 适合您。
1. **Kubernetes**: 在生产中运行 NTCore 需要 Kubernetes 集群。Kubernetes 是一个开源容器编排引擎，用于自动部署、扩展和管理容器化应用程序。您可以按照本[指南](https://kubernetes.io/docs/setup/production-environment/)手动设置 Kubernetes 集群。

2. **kubectl**: kubectl 是 Kubernetes 命令行工具，允许您针对 Kubernetes 集群运行命令。按照这个[指导](https://kubernetes.io/docs/tasks/tools/) 安装kubectl，而这个[指导](https://kubernetes.io/docs/tasks/access-application-cluster/configure-access-multiple-clusters/)来配置kubectl与Kubernetes集群通信。运行以下命令以确认 kubectl 是否配置正确。    

```
kubectl version
```

3. **PostgreSQL**: NTCore 使用 Postgres SQL 在生产中存储数据。按照本[指南](https://www.postgresql.org/download/)下载并安装 Postgres SQL 数据库。 

### 亚马逊 EKS
AWS 提供 Elastic Kubernetes Service (Amazon EKS) 来运行和扩展Kubernetes应用程序。有多种设置 EKS 集群的方法。本教程介绍使用 eksctl 的步骤，这是一个简单的工具。

1. **kubectl**: 一个用于 Kubernetes 集群的命令行工具。按照此[指南](https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html)安装 kubectl。  
2. **eksctl**: 用于创建和管理 EKS 集群的命令行工具。按照此[指南](https://docs.aws.amazon.com/eks/latest/userguide/eksctl.html) to安装 eksctl。  
3. **awscli**: 管理 AWS 服务的统一工具。按照此[指南](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html)安装 awscli
4. **IAM permissions**: eksctl 需要 IAM 权限才能管理 EKS 集群的 AWS 资源。
    1. 登录您的 AWS 账户并转到 IAM 服务控制台
    2. 在访问管理下的策略部分，如果新策略尚不存在，请按如下方式创建新策略。

    <details>
    <summary> AmazonEC2FullAccess（AWS 托管策略）</summary>
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
    <summary> AWSCloudFormationFullAccess（AWS 托管策略）</summary>
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

    3. 在访问管理下的用户部分，创建一个具有访问密钥凭据类型的新用户，并将上述 4 条策略附加到该用户。记下 access_key 和 access_key_id。
    4. 在~/.aws/credentials （Linux 和 Mac）或%USERPROFILE%\.aws\credentials (Windows) 中为 awscli指定用户配置文件，如下所示。    
    ```
    [default]
    aws_access_key_id={your_access_key_id}
    aws_secret_access_key={your_access_key}
    ```

5. 创建 EKS 集群。您可以使用提供的配置创建一个具有 1 个节点的简单 EKS 集群，如下所示。
```
eksctl create cluster -f cloud/aws/eksctl.yml
```
您可以在[eksctl](https://eksctl.io/introduction/)介绍和[github](https://eksctl.io/introduction/) 中找到有关如何创建 EKS 集群的更多示例。 

6. 使用以下命令验证 kubectl 是否已正确配置以与 EKS 集群交互。
```
kubectl version
```
7. **PostgreSQL**: 按照此[指南](https://aws.amazon.com/getting-started/hands-on/create-connect-postgresql-db/)使用 Amazon RDS 创建 PostgreSQL 数据库。  

## 安装
1. 从 github 下载 NTCore 存储库。
```
git clone https://github.com/nantutech/ntcore.git
```
2. 提供 PostgreSQL 配置  <ins>kube/app-config.yml</ins> 如下。
```
host: <postgres_host>
port: <postgres_port>
user: <postgres_user>
database: <postgres_database>
password: <postgres_password>
```

3. 在NTCore 存储库，运行以下命令
```
kubectl apply -f kube/traefik/
kubectl apply -f kube/ntcore/
```
4. 运行以下命令以找出 ntcore 的主机名（请参阅 EXTERNAL-IP 列）。
```
kubectl get service/traefik | awk {'print $1" " $2 " " $4 " " $5'} | column -t
``` 

5. 从浏览器的以下网址启动 NTCore。
```
http://{hostname}:8000/dsp/console/workspaces
```
 




