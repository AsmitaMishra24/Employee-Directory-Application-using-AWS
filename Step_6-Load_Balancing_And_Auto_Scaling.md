#  Load Balancing and Auto Scaling

---

### **Step 1: Launch an EC2 Instance**

1. Go to **EC2 > Instances**.
2. Select the stopped instance: `employee-directory-app-exercise6`.
3. Choose **Actions > Image and templates > Launch more like this**.
4. Modify the **Name tag**: `employee-directory-app-exercise7`.
5. Set **Key pair name** to: `app-key-pair`.
6. Under **Network settings**, set **Auto-assign Public IP** to `Enable`.
7. Click **Launch instance**, then **View all instances**.
8. Wait until **Status Checks** show 2/2 passed.
9. Copy its **Public IPv4 address** and test via `http://<public-ip>`.

---

### **Step 2: Create an Application Load Balancer (ALB)**

1. Navigate to **EC2 > Load Balancers > Create Load Balancer**.
2. Choose **Application Load Balancer**.
3. Set:
    - **Name**: `app-alb`
    - **VPC**: `app-vpc`
    - **AZs/Subnets**: Select both public subnets.
4. Create a **new security group**:
    - **Name**: `load-balancer-sg`
    - **Inbound rule**: Type `HTTP`, Source: `Anywhere (0.0.0.0/0)`
5. In **Listeners and Routing**, choose **Create target group**:
    - **Name**: `app-target-group`
    - **Target type**: `Instances`
    - Set advanced health check settings:
        - Healthy threshold: `2`
        - Unhealthy threshold: `5`
        - Timeout: `30`
        - Interval: `40`
6. Register the instance: `employee-directory-app-exercise7` → **Include as pending**.
7. Back in ALB setup, assign the new **target group**.
8. Finish creating the ALB and wait for its **state** to become `Active`.
9. Copy the ALB **DNS name**, prefix with `http://`, and test in a browser.

---

### **Step 3: Create a Launch Template**

1. Go to **EC2 > Launch Templates > Create Launch Template**.
2. Set:
    - **Name**: `app-launch-template`
    - **Version description**: “Web server for employee directory”
    - **AMI**: Use current one
    - **Instance type**: `t2.micro`
    - **Key pair**: `app-key-pair`
    - **Security group**: `web-security-group`
3. Under **Advanced details**:
    - **IAM instance profile**: `S3DynamoDBFullAccessRole`
    - **User data** (update values accordingly):

```bash
bash
CopyEdit
#!/bin/bash -ex
wget https://aws-tc-largeobjects.s3-us-west-2.amazonaws.com/DEV-AWS-MO-GCNv2/FlaskApp.zip
unzip FlaskApp.zip
cd FlaskApp/
yum -y install python3 mysql
pip3 install -r requirements.txt
amazon-linux-extras install epel
yum -y install stress
export PHOTOS_BUCKET=employee-photo-bucket-al-907
export AWS_DEFAULT_REGION=us-west-2
export DYNAMO_MODE=on
FLASK_APP=application.py /usr/local/bin/flask run --host=0.0.0.0 --port=80

```

1. Click **Create launch template**.

---

### **Step 4: Create an Auto Scaling Group**

1. Go to **EC2 > Auto Scaling Groups > Create Auto Scaling group**.
2. Set:
    - **Name**: `app-asg`
    - **Launch Template**: `app-launch-template`
3. Under **Network**, choose:
    - **VPC**: `app-vpc`
    - **AZs/Subnets**: Select both **public subnets**.
4. Attach to existing **target group**: `app-target-group`.
    - Health checks: **ELB**
5. Configure group size and scaling:
    - Desired: `2`
    - Min: `2`
    - Max: `4`
    - Scaling policy: **Target tracking**
        - Target value: `60`
        - Instances need: `300`
6. Add notifications:
    - SNS Topic: `app-sns-topic`
    - Email: Your own email → Confirm via AWS email.

---

### **Step 5: Test & Stress Auto Scaling**

1. Go to **EC2 > Target Groups > app-target-group > Targets**.
2. Wait for 2 instances to be `healthy`.
3. Test app via ALB URL: `http://<alb-dns-name>/info`.
4. Refresh the page — instance_id or AZ should change each time.
5. Start stress test via browser (or CLI):
    - Click **Stress CPU** for 10 minutes.
6. After ~10 min, check **EC2 > Target Groups** again.
    - You should see new instances added due to scaling.
    - Also, you’ll get a **notification email** from SNS.

---

### **Step 6: Cleanup Resources**

To avoid charges, delete all resources:

| Resource | How to Delete |
| --- | --- |
| **Auto Scaling Group** | EC2 > Auto Scaling Groups > Select `app-asg` > Delete |
| **Application Load Balancer** | EC2 > Load Balancers > `app-alb` > Delete |
| **Target Group** | EC2 > Target Groups > `app-target-group` > Delete |
| **EC2 Instances** | EC2 > Instances > Select all `employee-directory-app-*` > Terminate |
| **DynamoDB Table** | DynamoDB > Tables > `Employees` > Delete |
| **S3 Bucket** | S3 > `employee-photo-bucket-*` > Empty & Delete |
| **Route Tables** | VPC > Route Tables > `app-routetable-*` > Unassociate subnets > Delete |
| **Internet Gateway** | VPC > Internet Gateways > `app-igw` > Detach > Delete |
| **Subnets** | VPC > Subnets > All 4 > Delete |
| **VPC** | VPC > Your VPCs > `app-vpc` > Delete |
| **Security Groups** | EC2 > Security Groups > `app-sg`, `load-balancer-sg` > Delete |
| **IAM Role** | IAM > Roles > `S3DynamoDBFullAccessRole` > Delete |
| **SNS Topic** | SNS > Topics > `app-sns-topic` > Delete |
