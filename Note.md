# Note

- [ ] task 與 sevice的差別?
- [ ] 如何配合 Application LoadBalancer 使用?
- [ ] 其他建構方式?

## Amazon ECS 硬體限制與部署小建議

- [x] 使用 Linux 容器只能在 Linux 上的叢集運作
- [x] 使用 Windows 容器也只能在 EC2 上運作，Fargate 不支援 Windows 容器
- [x] 資料庫系統建立搭配 Amazon RDS 或其他全託管的服務
- [x] 盡可能精簡化 docker images ，靜態檔案可以直接透過 S3 存取
- [x] https://docs.aws.amazon.com/AmazonECS/latest/developerguide/Welcome.html