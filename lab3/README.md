# Lab3

## 建立 Amazon ECS Fargate Cluster 

- 建立 Cluster (Fargate) 使用 AWS Sample App 建立，會建構一些必要的 IAM Role 便於使用  
![info](sample1.png)  
![info](iam-role.png)

### 建立 Task Definitions

- first-run-task-definition.json 引入
- JSON 檔內的帳號 ID 需要自行調整成自己的帳號 ID
- `"arn:aws:iam::674636563715:role/ecsTaskExecutionRole"`

### Run Task

使用 AWS Sample App 跑起來的環境做教學說明

## 建立 Amazon ECS with EC2 Cluster

- 部署模型使用 EC2 Linux + Networking  
![info](./Mac-2020-07-17%20at%2015.38.48.png)

- 設定細節參考  
![info](./screenshot-ap-northeast-1.console.aws.amazon.com-2020.07.17-15_11_01.png)

- 執行並建立 EC2 Linux Cluster  
![info](./Mac-2020-07-17%20at%2015.44.36.png)

- task-def-wordpress.json 引入
- JSON 檔內的帳號 ID 需要自行調整成自己的帳號 ID
- `"arn:aws:iam::674636563715:role/ecsTaskExecutionRole"`
