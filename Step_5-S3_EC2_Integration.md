# Integrating S3 with an EC2 Based Application

---

### **Task 1: Create an S3 Bucket**

1. Log in to the **AWS Management Console** as the **Admin user**.
2. Search for **S3** in the search bar and select **S3** to open the console.
3. Choose **Create bucket**.
4. Enter the following in **Bucket name**:
    
    ```
    employee-photo-bucket-<your-initials>-<unique-number>
    
    ```
    
    **Example**: `employee-photo-bucket-al-907`
    
5. Choose **Create bucket**.

---

### **Task 2: Upload a Photo to S3**

1. Click the name of your **newly created bucket** to open it.
2. Choose **Upload** → **Add files**.
3. Select a photo from your computer and click **Open**.
4. Choose **Upload**.
5. Confirm that **Upload succeeded** appears in green.
6. Click **Close**.

---

### **Task 3: Modify the S3 Bucket Policy**

1. In the bucket's dashboard, go to the **Permissions** tab.
2. Scroll to **Bucket policy** and click **Edit**.
3. Paste the following JSON into the editor, updating placeholders:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowS3ReadAccess",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::<INSERT-ACCOUNT-NUMBER>:role/S3DynamoDBFullAccessRole"
      },
      "Action": "s3:*",
      "Resource": [
        "arn:aws:s3:::<INSERT-BUCKET-NAME>",
        "arn:aws:s3:::<INSERT-BUCKET-NAME>/*"
      ]
    }
  ]
}

```

1. Replace:
    - `<INSERT-ACCOUNT-NUMBER>` → your **AWS account number**
    - `<INSERT-BUCKET-NAME>` → your **bucket name**
    
    **Example**:
    
    ```json

    "AWS": "arn:aws:iam::123456789012:role/S3DynamoDBFullAccessRole"
    "Resource": [
      "arn:aws:s3:::employee-photo-bucket-al-907",
      "arn:aws:s3:::employee-photo-bucket-al-907/*"
    ]
    
    ```
    
2. Click **Save changes**.

---

### **Task 4: Launch EC2 Instance With S3 Integration**

1. Go to **EC2** from the AWS Services menu.
2. In the **Instances** section, find the **employee-directory-app** instance (should be **Stopped**).
3. Choose **Actions > Image and templates > Launch more like this**.
4. Update the **Name** tag value to:
    
    ```
    employee-directory-app-s3
    
    ```
    
5. Under **Key pair name**, select: `app-key-pair`.
6. Expand **Network settings** and set:
    - **Auto-assign Public IP** → **Enable**
7. Scroll to **Advanced details** > **User data**, and update the script:

```bash
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

> ⚠️ Replace employee-photo-bucket-al-907 and us-west-2 with your actual bucket name and region if different.
> 
1. Click **Launch instance**.

---

### **Task 5: Test the Application**

1. Go to **EC2 > Instances**.
2. Wait for the new instance to show:
    - **Instance state**: `Running`
    - **Status checks**: `2/2 checks passed`
3. Copy the **Public IPv4 address** (do **not** click the link).
4. Open a new browser tab and paste the IP, using `http://` (not `https`):
    
    ```
    http://<your-ec2-public-ip>
    
    ```
    
5. You should see an **Employee Directory** placeholder page.

---

### **Final Step: Stop the Instance (To Avoid Charges)**

1. Select the **employee-directory-app-s3** instance in the EC2 console.
2. Choose **Instance state > Stop instance**.

---

### **Congratulations!**

You've successfully:

1. Created and configured an **S3 bucket**
2. Uploaded a photo
3. Applied a custom **bucket policy**
4. Launched an EC2 app that pulls images from S3
