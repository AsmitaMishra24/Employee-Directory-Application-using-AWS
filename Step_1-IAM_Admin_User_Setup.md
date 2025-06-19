# Creating an IAM User with Administrator Access

## Step 1: Log in as the Root User

1. Open the [AWS Management Console](https://aws.amazon.com/console/).
2. Sign in using your **root user credentials** (email and password associated with the AWS account).

---

## Step 2: Open the IAM Console

1. In the **Services** search bar, type **IAM** and select **IAM** from the results.
2. In the IAM dashboard, navigate to the **Users** section in the left-hand pane.

---

## Step 3: Add a New IAM User

1. Click on **Add users**.
2. On the **Set user details** page, configure the following:
    - **User name**: `Admin`
    - **Select AWS credential type**:
        -  **Access key – Programmatic access**
        -  **Password – AWS Management Console access**
    - Under **Console password**:
        - Choose **Custom password**
        - Enter a secure password of your choice
    - Uncheck **Require password reset**
3. Click **Next: Permissions**

---

## Step 4: Set Permissions

1. On the **Set permissions** page, choose **Attach existing policies directly**.
2. In the **Filter policies** box, type `AdministratorAccess`.
3. Check the box for **AdministratorAccess**.

---

## Step 5: Add Tags (Optional)

1. Optionally add tags for user identification or skip this step.
2. Click **Next: Review**.

---

## Step 6: Review and Create User

1. Review the user configuration.
2. Click **Create user**.

---

## Step 7: Sign In with the New IAM User

1. After creation, you'll see a **Success** message with a **sign-in URL**.
2. The URL will look like this:  
   `https://123456789012.signin.aws.amazon.com/console`  
   *(Replace `123456789012` with your actual AWS account ID)*

---

## Step 8: Log in as Admin

1. Go to the sign-in URL.
2. Enter the **Admin** username and the password you created.
3. You are now logged in as an IAM user with administrator privileges.
