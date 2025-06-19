# Launching an EC2 Instance for the Employee Directory Application

## Step 1: Log in to the AWS Management Console

1. Sign in as the **Admin** IAM user, if you're not already logged in.

---

## Step 2: Open the EC2 Service

1. In the **Services** search bar, type **EC2** and select **EC2**.
2. In the left-hand navigation pane, choose **Instances**.

---

## Step 3: Launch a New EC2 Instance

1. Click **Launch instances**.
2. Configure the following settings.

---

## Step 4: Configure Instance Settings

| **Section** | **Setting** |
| --- | --- |
| **Name** | `employee-directory-app` |
| **Application and OS Image** | Amazon Linux |
| **Instance type** | `t2.micro` |
| **Key pair (login)** | Create new key pair:<br>• Name: `app-key-pair`<br>• Click **Create key pair**<br>• A `.pem` file will be downloaded |
| **Network settings** | • **VPC**: Default<br>• **Subnet**: Select the **first** one<br>• **Auto-assign Public IP**: Enabled |
| **Firewall (security groups)** | • Choose **Create security group**<br>• Name/Description: `app-sg`<br>• Remove default SSH rule<br>• Add Rule: HTTP, Source: Anywhere |

---

## Step 5: Add IAM Role and User Data

1. Expand **Advanced details**.
2. Under **IAM instance profile**, select: `S3DynamoDBFullAccessRole`.
3. Paste the following in **User data**:

    ```bash
    #!/bin/bash -ex
    wget https://aws-tc-largeobjects.s3-us-west-2.amazonaws.com/DEV-AWS-MO-GCNv2/FlaskApp.zip
    unzip FlaskApp.zip
    cd FlaskApp/
    yum -y install python3 mysql
    pip3 install -r requirements.txt
    amazon-linux-extras install epel
    yum -y install stress
    export PHOTOS_BUCKET=${SUB_PHOTOS_BUCKET}
    export AWS_DEFAULT_REGION=<INSERT REGION HERE>
    export DYNAMO_MODE=on
    FLASK_APP=application.py /usr/local/bin/flask run --host=0.0.0.0 --port=80
    ```

4. Replace the line below with your **actual AWS region**:

    ```bash
    export AWS_DEFAULT_REGION=<INSERT REGION HERE>
    ```

    **Example for US West (Oregon):**

    ```bash
    export AWS_DEFAULT_REGION=us-west-2
    ```

> ⚠️ **Note:** Keep `${SUB_PHOTOS_BUCKET}` as-is. You will update this in a later step.

---

## Step 6: Launch the Instance

1. Click **Launch instance**.
2. Then click **View all instances**.

---

## Step 7: Verify the Instance

1. Wait until **Instance state** is **Running**.
2. Confirm that **Status check** shows **2/2 checks passed**.

> 🔄 If the page doesn't update, refresh after a few minutes.

---

## Viewing the Application in a Web Browser

1. Select your instance.
2. In the **Details** tab, copy the **Public IPv4 address**.

> ⚠️ Only copy the address — **do not click** the link.

3. Open a new browser tab and go to:

    ```
    http://<your-public-ipv4>
    ```

    **Example:**

    ```
    http://18.223.45.67
    ```

---

## ✅ Success

You should now see the **Employee Directory** placeholder page!

While the database connection is not yet configured, the application front end is successfully running on your EC2 instance.

> 🎉 Congratulations! You've launched your EC2 instance and deployed the app front end.

