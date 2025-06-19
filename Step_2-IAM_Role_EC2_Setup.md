# Setting Up an IAM Role for an EC2 Instance

## Step 1: Open the IAM Console

1. Ensure you are logged in as the **Admin** IAM user.
2. In the AWS Management Console, use the **Services** search bar to search for and open **IAM**.

---

## Step 2: Begin Role Creation

1. In the IAM navigation pane, click on **Roles**.
2. Choose **Create role**.

---

## Step 3: Select Trusted Entity

1. On the **Select trusted entity** page:
    - **Trusted entity type**: `AWS service`
    - **Use case**: `EC2` (Allows EC2 to assume this role)
2. Click **Next**.

---

## Step 4: Attach Permissions Policies

1. In the **permissions filter** box, type `amazons3full`:
    -  Select **AmazonS3FullAccess**
2. Then type `amazondynamodb`:
    -  Select **AmazonDynamoDBFullAccess**
3. Click **Next**.

---

## Step 5: Name the Role and Create

1. On the **Name, review, and create** page:
    - **Role name**: `S3DynamoDBFullAccessRole`
2. Click **Create role**.

---

## Important Note

> ⚠️ **Security Reminder**  
> This example uses full-access policies (AmazonS3FullAccess and AmazonDynamoDBFullAccess) for demonstration and learning purposes.  
> In a **production environment**, avoid granting full access.  
> Once your S3 bucket and DynamoDB table are configured, **refine this IAM role** to follow the principle of **least privilege**.  
> You will learn more about permissions scoping in a later step.
