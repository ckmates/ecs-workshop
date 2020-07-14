# ecs-workshop

利用 Cloud9 建立一個用於線上開發的環境，部署 ecs 服務  
LAB 練習區域限定於 us-east-1 (維吉尼亞)


## 建立 IAM 使用者金鑰

直接建立 Admin 權限用於測試環境，減少權限問題，用完務必刪除

## Amazon Cloud9 開發環境

- [ ] 關閉預設 `credentials`
- [ ] 設定字型大小便於使用
- [ ] 設定 `aws configure`
- [ ] 區域填入`us-east-1`
- [ ] 測試 `aws cli`

## 安裝 Zsh , ECS CLI

```bash
# https://github.com/ohmyzsh/ohmyzsh

sudo apt install zsh -y ; sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

# 設定命令完成套件，編輯.zshrc
plugins=(git docker docker-compose aws)

# 設定完成後，重新開一個終端機視窗

# 安裝 ECS-CLI
sudo curl -o /usr/local/bin/ecs-cli https://amazon-ecs-cli.s3.amazonaws.com/ecs-cli-linux-amd64-latest
sudo chmod +x /usr/local/bin/ecs-cli

```


## Amazon ECR Repositories

使用 aws cli 建立 `aws ecr create-repository --repository-name demo`

```bash

➜  environment aws ecr create-repository --repository-name demo
{
    "repository": {
        "repositoryArn": "arn:aws:ecr:us-east-1:674636563715:repository/demo",
        "registryId": "674636563715",
        "repositoryName": "demo",
        "repositoryUri": "674636563715.dkr.ecr.us-east-1.amazonaws.com/demo",
        "createdAt": 1594711027.0,
        "imageTagMutability": "MUTABLE",
        "imageScanningConfiguration": {
            "scanOnPush": false
        }
    }
}
➜  environment 
```


## Docker Pull / Push

```bash
# docker 基本的指令
# 拉取 docker images
docker pull amazon/amazon-ecs-sample:latest
docker pull ckmates/httpd:v1

# Amazon ECR 操作指令
# 重新命名tag
docker tag demo:latest 674636563715.dkr.ecr.us-east-1.amazonaws.com/demo:latest

# 取得登入憑證
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 674636563715.dkr.ecr.us-east-1.amazonaws.com

# 推送至 Amazon ECR
docker push 674636563715.dkr.ecr.us-east-1.amazonaws.com/demo:latest
```

## Cluster

建立 Cluster (Fargate)

```bash
# 使用指令建立
➜  environment aws ecs create-cluster --cluster-name fg-demo
{
    "cluster": {
        "clusterArn": "arn:aws:ecs:us-east-1:674636563715:cluster/fg-demo",
        "clusterName": "fg-demo",
        "status": "ACTIVE",
        "registeredContainerInstancesCount": 0,
        "runningTasksCount": 0,
        "pendingTasksCount": 0,
        "activeServicesCount": 0,
        "statistics": [],
        "tags": [],
        "settings": [
            {
                "name": "containerInsights",
                "value": "disabled"
            }
        ],
        "capacityProviders": [],
        "defaultCapacityProviderStrategy": []
    }
}
➜  environment
```


## Task Definitions

建立工作藍圖，利用剛剛的 amazon-ecs-sample 建立最基本的工作，注意工作 Role 需要事先定義


```json

{
    "requiresCompatibilities": [
        "FARGATE"
    ],
    "inferenceAccelerators": [],
    "containerDefinitions": [
        {
            "name": "amazon-ecs-sample",
            "image": "amazon-ecs-sample",
            "resourceRequirements": null,
            "essential": true,
            "portMappings": [
                {
                    "containerPort": "80",
                    "protocol": "tcp"
                }
            ],
            "environment": null,
            "secrets": null,
            "mountPoints": null,
            "volumesFrom": null,
            "hostname": null,
            "user": null,
            "workingDirectory": null,
            "extraHosts": null,
            "logConfiguration": {
                "logDriver": "awslogs",
                "options": {
                    "awslogs-group": "/ecs/amazon-ecs-sample",
                    "awslogs-region": "us-east-1",
                    "awslogs-stream-prefix": "ecs"
                }
            },
            "ulimits": null,
            "dockerLabels": null,
            "dependsOn": null,
            "repositoryCredentials": {
                "credentialsParameter": ""
            }
        }
    ],
    "volumes": [],
    "networkMode": "awsvpc",
    "memory": "512",
    "cpu": "256",
    "executionRoleArn": null,
    "family": "amazon-ecs-sample",
    "taskRoleArn": "",
    "tags": []
}

```



## Run Task

使用 GUI 建立 workshop 環境