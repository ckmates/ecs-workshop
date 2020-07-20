# Lab2

## 建立 Amazon ECR Repositories

- [ ] GUI 方式建立 (最省事)
- [ ] 也可使用 aws cli 建立 `aws ecr create-repository --repository-name demo`
- [ ] registryId 每個帳號 ID 不同

```bash

aws ecr create-repository --repository-name demo
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

```

## Docker Pull / Push 到剛剛建立的 Amazon ECR Repositories

```bash

# docker 基本的指令
# 提取 docker images
docker pull amazon/amazon-ecs-sample:latest
docker pull ckmates/httpd:v1

# 以下的設定每個帳號 ID 不同，不可照抄！
#
# Amazon ECR 操作指令
# 重新命名 docker images tag
docker tag demo:latest 674636563715.dkr.ecr.us-east-1.amazonaws.com/demo:latest

# 取得 Amazon ECR 登入憑證 
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 674636563715.dkr.ecr.us-east-1.amazonaws.com

# 推送至 Amazon ECR
docker push 674636563715.dkr.ecr.us-east-1.amazonaws.com/demo:latest

```
