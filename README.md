

Source: 
https://github.com/sagarising/hello-world-terraform


```
cd infra/accounts/dev/remote_state; terraform init; terraform apply
```
![image](https://github.com/n0psI3d/terraform-ecs-demo/blob/main/Screenshots/Pasted%20image%2020230810131046.png)

This will deploy the `DynoDB` & an `S3 Bucket` which will be displayed in the AWS Dashboard
![image](https://github.com/n0psI3d/terraform-ecs-demo/blob/main/Screenshots/Pasted%20image%2020230810131225.png)

![image](https://github.com/n0psI3d/terraform-ecs-demo/blob/main/Screenshots/Pasted%20image%2020230810131216.png)

Next, we need to apply the `ecs` project.

```
cd infra/account/dev/ecs; terraform init; terraform apply
```
![image](https://github.com/n0psI3d/terraform-ecs-demo/blob/main/Screenshots/Pasted%20image%2020230810131334.png)
This will result in creating a bunch of resources such as the `ecs cluster`, `security groups`,`vpc`,`load balancer` , and more. 
Once done, the `web_endpoint` will be displayed along with the `ecr_repo_url`
![image](https://github.com/n0psI3d/terraform-ecs-demo/blob/main/Screenshots/Pasted%20image%2020230810131841.png)

## Docker configuration

Once we have the `ecr_repo_url` we can build the docker configuration.

```bash
docker build -t <ecr_repo_url>:<version> .
```

![image](https://github.com/n0psI3d/terraform-ecs-demo/blob/main/Screenshots/Pasted%20image%2020230810131216.png)

Perform authentication:
```
aws ecr get-login-password --region <region> | docker login --username AWS --password-stdin <ecr_repo_url>
```
![image](https://github.com/n0psI3d/terraform-ecs-demo/blob/main/Screenshots/Pasted%20image%20230810140114.png)

Push the docker image:
```bash
docker push <ecr_repo_url>:<version>
```
![image](https://github.com/n0psI3d/terraform-ecs-demo/blob/main/Screenshots/Pasted%20image%20230810140431.png)

![image](https://github.com/n0psI3d/terraform-ecs-demo/blob/main/Screenshots/Pasted%20image%20230810140703.png)

The following command will deploy a task definition
```bash
cd infra/accounts/dev/ecs; terraform apply -var="release_version=v1.0.0"
```
![image](https://github.com/n0psI3d/terraform-ecs-demo/blob/main/Screenshots/Pasted%20image%20230810133256.png)
The results can be viewed in the AWS Dashboard:
![image](https://github.com/n0psI3d/terraform-ecs-demo/blob/main/Screenshots/Pasted%20image%20230810133459.png)

![image](https://github.com/n0psI3d/terraform-ecs-demo/blob/main/Screenshots/Pasted%20image%20230810133641.png)
