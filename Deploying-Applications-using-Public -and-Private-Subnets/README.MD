# AWS Application Deployment using Public and Private Subnets

## 📋 Project Overview

This project demonstrates deploying a scalable web application on AWS using a multi-tier architecture with public and private subnets, load balancing, auto-scaling, and secure access through a bastion host.

---

## 🏗️ Architecture Components

- **VPC**: Custom VPC with public and private subnets
- **Multi-AZ**: Deployment across two availability zones
- **Application Load Balancer**: Distributes traffic across instances
- **Auto Scaling Group**: Manages 1-4 EC2 instances automatically
- **NAT Gateways**: Two NAT Gateways (one per AZ)
- **Bastion Host**: Secure jump server for private subnet access
- **Python Web Application**: Simple HTTP server on port 8000

---

## 📚 Table of Contents

- [Step 0: Architecture Diagram](#step-0-architecture-diagram)
- [Step 1: Create VPC](#step-1-create-vpc)
- [Step 2: Configure Subnets](#step-2-configure-subnets)
- [Step 3: NAT Gateway Setup](#step-3-nat-gateway-setup)
- [Step 4: VPC Creation Complete](#step-4-vpc-creation-complete)
- [Step 5: Create Auto Scaling Group](#step-5-create-auto-scaling-group)
- [Step 6: No Launch Template Available](#step-6-no-launch-template-available)
- [Step 7: Create Launch Template](#step-7-create-launch-template)
- [Step 8: Configure Launch Template](#step-8-configure-launch-template)
- [Step 9: Create Security Group](#step-9-create-security-group)
- [Step 10: Launch Template Created](#step-10-launch-template-created)
- [Step 11: Select Launch Template](#step-11-select-launch-template)
- [Step 12: Configure Auto Scaling Capacity](#step-12-configure-auto-scaling-capacity)
- [Step 13: Auto Scaling Group Created](#step-13-auto-scaling-group-created)
- [Step 14: Two Instances Running](#step-14-two-instances-running)
- [Step 15: Create Bastion Host](#step-15-create-bastion-host)
- [Step 16: Configure Bastion Host](#step-16-configure-bastion-host)
- [Step 17: Three Instances Running](#step-17-three-instances-running)
- [Step 18: Open MobaXterm](#step-18-open-mobaxterm)
- [Step 19: Transfer PEM File](#step-19-transfer-pem-file)
- [Step 20: SSH to Bastion Host](#step-20-ssh-to-bastion-host)
- [Step 21: SSH to Private Instance](#step-21-ssh-to-private-instance)
- [Step 22: Install Python and Create HTML](#step-22-install-python-and-create-html)
- [Step 23: Start Python Server](#step-23-start-python-server)
- [Step 24: Deploy on Second Instance](#step-24-deploy-on-second-instance)
- [Step 25: Create Target Group](#step-25-create-target-group)
- [Step 26: Create Load Balancer](#step-26-create-load-balancer)
- [Step 27: Configure Load Balancer Name](#step-27-configure-load-balancer-name)
- [Step 28: Select Public Subnets](#step-28-select-public-subnets)
- [Step 29: Configure Security Groups](#step-29-configure-security-groups)
- [Step 30: Select Target Group](#step-30-select-target-group)
- [Step 31: Load Balancer Created](#step-31-load-balancer-created)
- [Step 32: Copy DNS Name](#step-32-copy-dns-name)
- [Step 33: Access Application - Instance 1](#step-33-access-application---instance-1)
- [Step 34: Access Application - Instance 2](#step-34-access-application---instance-2)
- [Step 35: Verify Load Balancing](#step-35-verify-load-balancing)

---

## STEP 0: Architecture Diagram

**Overview**: Complete project architecture showing all components.

![Architecture Diagram](./images/step0.png)

This diagram shows:
- VPC with two availability zones
- Two public subnets (for ALB and Bastion Host)
- Two private subnets (for application instances)
- Application Load Balancer
- NAT Gateways (one per AZ)
- Auto Scaling Group
- Bastion Host

---

## STEP 1: Create VPC

**Action**: Create a custom VPC named "aws-project".

![Step 1](./images/step1.png)

1. Go to **AWS Console → VPC Dashboard**
2. Click **Create VPC**
3. Select **VPC and more**
4. **VPC Name**: `aws-project`
5. Leave default CIDR block: `10.0.0.0/16`

---

## STEP 2: Configure Subnets

**Action**: Set up 2 public and 2 private subnets across 2 availability zones.

![Step 2](./images/step2.png)

**Configuration**:
- **Availability Zones**: 2
- **Public Subnets**: 2 (one per AZ)
- **Private Subnets**: 2 (one per AZ)

---

## STEP 3: NAT Gateway Setup

**Action**: Configure NAT Gateways for private subnet internet access.

![Step 3](./images/step3.png)

**Configuration**:
- **NAT Gateways**: 1 per AZ (Total: 2)
- **VPC Endpoints**: None (not needed)

---

## STEP 4: VPC Creation Complete

**Action**: Verify VPC and all resources are created.

![Step 4](./images/step4.png)

**Created Resources**:
- ✅ VPC
- ✅ 4 Subnets (2 public, 2 private)
- ✅ Internet Gateway
- ✅ 2 NAT Gateways
- ✅ Route Tables

**Resource Map**: Shows complete VPC configuration with all components.

---

## STEP 5: Create Auto Scaling Group

**Action**: Begin creating Auto Scaling Group for EC2 instances.

![Step 5](./images/step5.png)

1. Go to **EC2 Dashboard → Auto Scaling Groups**
2. Click **Create Auto Scaling group**
3. **Name**: `aws-project`

---

## STEP 6: No Launch Template Available

**Action**: Identify that launch template needs to be created.

![Step 6](./images/step6.png)

No launch template exists yet. Click **Create a launch template** to proceed.

---

## STEP 7: Create Launch Template

**Action**: Create launch template for Auto Scaling instances.

![Step 7](./images/step7.png)

1. Go to **EC2 → Launch Templates → Create launch template**
2. **Name**: `aws-project-launch-template`
3. **Description**: "Launch template for auto-scaling instances in private subnets"

---

## STEP 8: Configure Launch Template

**Action**: Select AMI, instance type, and key pair.

![Step 8](./images/step8.png)

**Configuration**:
- **AMI**: Ubuntu Server (latest LTS)
- **Instance Type**: t2.micro
- **Key Pair**: `manoj-841` (select existing key pair)

---

## STEP 9: Create Security Group

**Action**: Configure firewall rules for instances.

![Step 9](./images/step9.png)

**Security Group Name**: `aws-project-sg`
**VPC**: Select `aws-project`

**Inbound Rules**:
1. **SSH**: Port 22, Source: Anywhere (0.0.0.0/0)
2. **Custom TCP**: Port 8000, Source: Anywhere (0.0.0.0/0)

---

## STEP 10: Launch Template Created

**Action**: Finalize and create launch template.

![Step 10](./images/step10.png)

Click **Create launch template** button. Template is now ready to use.

---

## STEP 11: Select Launch Template

**Action**: Return to Auto Scaling Group and select the launch template.

![Step 11](./images/step11.png)

1. Go back to **Auto Scaling Groups** creation
2. Select **aws-project-launch-template**
3. Choose **Latest** version
4. Click **Next**

---

## STEP 12: Configure Auto Scaling Capacity

**Action**: Set scaling parameters and select private subnets.

![Step 12](./images/step12.png)

**Capacity Settings**:
- **Desired capacity**: 2
- **Minimum capacity**: 1
- **Maximum capacity**: 4

**Network**:
- **VPC**: `aws-project`
- **Subnets**: Select **both private subnets** ✅

Click **Next** and then **Create Auto Scaling group**.

---

## STEP 13: Auto Scaling Group Created

**Action**: Verify Auto Scaling Group is created and active.

![Step 13](./images/step13.png)

Auto Scaling Group `aws-project` is now active and managing instances.

---

## STEP 14: Two Instances Running

**Action**: Verify two EC2 instances are launched in private subnets.

![Step 14](./images/step14.png)

Go to **EC2 → Instances**:
- ✅ 2 instances running
- ✅ Both in private subnets
- ✅ No public IPs (private instances)

---

## STEP 15: Create Bastion Host

**Action**: Launch bastion host for secure access to private instances.

![Step 15](./images/step15.png)

1. Go to **EC2 → Launch Instance**
2. **Instance Name**: `bastion-host`

---

## STEP 16: Configure Bastion Host

**Action**: Configure bastion host with public IP enabled.

![Step 16](./images/step16.png)

**Configuration**:
- **AMI**: Ubuntu Server
- **Instance Type**: t2.micro
- **Key Pair**: `manoj-841`
- **VPC**: `aws-project`
- **Subnet**: Select a **public subnet** ✅
- **Auto-assign Public IP**: **Enable** ✅

**Security Group**: Allow SSH (port 22) from anywhere

Click **Launch Instance**.

---

## STEP 17: Three Instances Running

**Action**: Verify three instances (1 bastion + 2 private instances).

![Step 17](./images/step17.png)

**Instance Summary**:
- ✅ 1 bastion-host (public subnet, has public IP)
- ✅ 2 private instances (private subnets, no public IPs)

---

## STEP 18: Open MobaXterm

**Action**: Open MobaXterm for SSH connections.

![Step 18](./images/step18.png)

Launch MobaXterm application on your local computer.

---

## STEP 19: Transfer PEM File

**Action**: Copy PEM file to bastion host using SCP.

![Step 19](./images/step19.png)

**Command**:
```bash
scp -i /path/to/manoj-841.pem /path/to/manoj-841.pem ubuntu@<bastion-public-ip>:~/
```

**Example**:
```bash
scp -i manoj-841.pem manoj-841.pem ubuntu@54.123.45.67:~/
```

---

## STEP 20: SSH to Bastion Host

**Action**: Connect to bastion host via SSH.

![Step 20](./images/step20.png)

**Command**:
```bash
ssh -i /path/to/manoj-841.pem ubuntu@<bastion-public-ip>
```

**Example**:
```bash
ssh -i manoj-841.pem ubuntu@54.123.45.67
```

---

## STEP 21: SSH to Private Instance

**Action**: From bastion host, SSH to first private instance.

![Step 21](./images/step21.png)

**Commands (on Bastion Host)**:
```bash
# Set PEM file permissions
chmod 400 manoj-841.pem

# SSH to private instance (use private IP)
ssh -i manoj-841.pem ubuntu@<private-instance-1-ip>
```

**Example**:
```bash
ssh -i manoj-841.pem ubuntu@10.0.128.45
```

---

## STEP 22: Install Python and Create HTML

**Action**: Create HTML page on first private instance.

![Step 22](./images/step22.png)

**Commands (on Private Instance 1)**:
```bash
# Create HTML file
nano index.html
```

**HTML Content**:
```html
<!DOCTYPE html>
<html>
<head>
    <title>AWS Project</title>
</head>
<body>
    <h1>Welcome to AWS Project!</h1>
    <p>This is Private Instance 1</p>
</body>
</html>
```

**Save**: Press `Ctrl+X`, then `Y`, then `Enter`

---

## STEP 23: Start Python Server

**Action**: Launch Python HTTP server on port 8000.

![Step 23](./images/step23.png)

**Command**:
```bash
python3 -m http.server 8000
```

**Output**:
```
Serving HTTP on 0.0.0.0 port 8000 ...
```

⚠️ **Keep this terminal open** - server runs in foreground.

---

## STEP 24: Deploy on Second Instance

**Action**: Deploy application on second private instance with different content.

![Step 24](./images/step24.png)

**Steps**:
1. Open **new MobaXterm session**
2. SSH to bastion: `ssh -i manoj-841.pem ubuntu@<bastion-public-ip>`
3. SSH to second private instance: `ssh -i manoj-841.pem ubuntu@<private-instance-2-ip>`
4. Create `index.html` with different content:

```html
<!DOCTYPE html>
<html>
<head>
    <title>AWS Project</title>
</head>
<body>
    <h1>Welcome to AWS Project!</h1>
    <p>This is Private Instance 2</p>
</body>
</html>
```

5. Start server: `python3 -m http.server 8000`

---

## STEP 25: Create Target Group

**Action**: Create target group for load balancer.

![Step 25](./images/step25.png)

1. Go to **EC2 → Target Groups → Create target group**
2. **Target type**: Instances
3. **Name**: `aws-project`
4. **Protocol**: HTTP
5. **Port**: 8000
6. **VPC**: `aws-project`
7. Click **Next**
8. **Register targets**: Select both private instances
9. Click **Include as pending below**
10. Click **Create target group**

---

## STEP 26: Create Load Balancer

**Action**: Begin creating Application Load Balancer.

![Step 26](./images/step26.png)

1. Go to **EC2 → Load Balancers → Create Load Balancer**
2. Select **Application Load Balancer**
3. Click **Create**

---

## STEP 27: Configure Load Balancer Name

**Action**: Set load balancer name and basic settings.

![Step 27](./images/step27.png)

**Configuration**:
- **Name**: `aws-project`
- **Scheme**: Internet-facing
- **IP address type**: IPv4

---

## STEP 28: Select Public Subnets

**Action**: Select public subnets for load balancer placement.

![Step 28](./images/step28.png)

**Network Mapping**:
- **VPC**: `aws-project`
- **Mappings**: Select **both availability zones**
  - ✅ AZ 1 → **Public Subnet 1**
  - ✅ AZ 2 → **Public Subnet 2**

---

## STEP 29: Configure Security Groups

**Action**: Create or select security group for load balancer.

![Step 29](./images/step29.png)

**Security Group**: Create new or select existing

**Inbound Rule**:
- **Type**: HTTP
- **Port**: 80
- **Source**: 0.0.0.0/0 (Anywhere)

---

## STEP 30: Select Target Group

**Action**: Configure listener and select target group.

![Step 30](./images/step30.png)

**Listener Configuration**:
- **Protocol**: HTTP
- **Port**: 80
- **Default action**: Forward to target group
- **Target group**: Select `aws-project`

Click **Create load balancer**.

---

## STEP 31: Load Balancer Created

**Action**: Wait for load balancer to become active.

![Step 31](./images/step31.png)

**Status**: Provisioning → Active (wait 2-3 minutes)

Load balancer `aws-project` is now active.

---

## STEP 32: Copy DNS Name

**Action**: Copy load balancer DNS name.

![Step 32](./images/step32.png)

1. Go to **Load Balancers**
2. Select `aws-project`
3. Copy **DNS name** from details section

**Example DNS**:
```
aws-project-1234567890.us-east-1.elb.amazonaws.com
```

---

## STEP 33: Access Application - Instance 1

**Action**: Access application via load balancer DNS.

![Step 33](./images/step33.png)

1. Open web browser
2. Paste load balancer DNS name
3. Press Enter

**Expected Output**:
```
Welcome to AWS Project!
This is Private Instance 1
```

---

## STEP 34: Access Application - Instance 2

**Action**: Refresh browser to see load balancing in action.

![Step 34](./images/step34.png)

1. **Refresh browser** (F5) or open **new tab**

**Expected Output**:
```
Welcome to AWS Project!
This is Private Instance 2
```

This proves load balancer is distributing traffic between both instances!

---

## STEP 35: Verify Load Balancing

**Action**: Final verification of complete architecture.

![Step 35](./images/step35.png)

**Test with multiple browsers/tabs**:
- Open DNS in Chrome → May show Instance 1
- Open DNS in Firefox → May show Instance 2
- Open multiple tabs → Traffic distributes across instances

**Success Indicators**:
- ✅ Load balancer distributing traffic
- ✅ Both instances responding
- ✅ Different content shows on different requests
- ✅ Multi-AZ architecture working
- ✅ Complete project deployed successfully!

---

## 🎉 Project Complete!

You have successfully:
- ✅ Created VPC with public and private subnets
- ✅ Deployed resources across 2 availability zones
- ✅ Configured Auto Scaling Group
- ✅ Set up Application Load Balancer
- ✅ Implemented bastion host for secure access
- ✅ Deployed Python web application
- ✅ Verified load balancing functionality

---

## 📝 Key Concepts

- **VPC**: Isolated network environment
- **Public Subnets**: For internet-facing resources (ALB, Bastion)
- **Private Subnets**: For application instances (no direct internet access)
- **NAT Gateway**: Allows private instances to access internet
- **Bastion Host**: Secure gateway for SSH access
- **Application Load Balancer**: Distributes traffic across instances
- **Auto Scaling**: Automatically manages instance count
- **Target Group**: Routes traffic to healthy instances

---

## 🛠️ Troubleshooting

### Cannot connect to bastion host
- Check security group allows SSH from your IP
- Verify bastion has public IP
- Ensure bastion is in public subnet

### Cannot SSH to private instances
- Verify PEM file is on bastion host
- Check security group allows SSH from bastion
- Use private IP addresses, not public IPs

### Python server not starting
- Check if port 8000 is already in use
- Verify Python3 is installed: `python3 --version`

### Cannot access via load balancer
- Check target group health: Both should be "healthy"
- Verify security groups allow traffic
- Ensure Python servers are running on both instances

---

## 👤 Author

**Gaddam Manoj Kumar Reddy**  
Date: October 21, 2025

---

## 📄 License

This project is for educational purposes.

---

## 🔗 Additional Resources

- [AWS VPC Documentation](https://docs.aws.amazon.com/vpc/)
- [AWS EC2 Documentation](https://docs.aws.amazon.com/ec2/)
- [AWS Load Balancer Documentation](https://docs.aws.amazon.com/elasticloadbalancing/)
- [AWS Auto Scaling Documentation](https://docs.aws.amazon.com/autoscaling/)
