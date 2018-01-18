# AWS Practice Notes

## 2018-01-17: First Use


### First Use of IAM

Before creating any new instances or doing anything else, I decided to create a user account for use while I was learning. This seemed like a good practice and would allow me to start using IAM right away.

The account was allowed  access via access key ID and secret access key so I could use awscli to control things.

I created a new permissions group named 'hyper-learning-group-1'. This group is given full Elastic Beanstalk access, including underlying services like EC2 and S3. I chose these settings since I plan on starting out learning about EC2 and S3 anyway.


### Deploying an Instance in EC2 using AWS CLI

I began by creating a new key pair for EC2 using the CLI with the following command:

```
aws ec2 create-key-pair --key-name FirstEC2KeyPair --query 'KeyMaterial' --output text > FirstKeyPair.pem
```

After this was done, I verified the fingerprint using the following command:

```
aws ec2 describe-key-pairs --key-name FirstEC2KeyPair --output json
```

Everything was correct, so I moved onto creating a new EC2 security group with the group name 'learning-sg' and Group ID `sg-d55a88ac`.

```
aws ec2 create-security-group --group-name learning-sg --description "My EC2 learning security group"
```

With the new group created, I moved on to allowing port 22 inbound to the instance so that I could log in via SSH if necessary.

```
aws ec2 authorize-security-group-ingress --group-name learning-sg --protocol tcp --port 22 --cidr 172.90.19.0/24
```

Finally, I moved on to creating my first EC2 instance. 

```
aws ec2 run-instances --image-id ami-a51f27c5 --count 1 --instance-type t2.micro --key-name FirstEC2KeyPair --security-groups learning-sg
```

After waiting a few minutes to ensure the instance was up and running, I connected via SSH in a separate terminal:

```
ssh -i FirstKeyPair.pem ec2-user@13.57.28.215
```

When I was done looking around, I ran the following command to terminate the existing instance

```
aws ec2 terminate-instances --instance-ids i-0ac9870d141572ff6
```


