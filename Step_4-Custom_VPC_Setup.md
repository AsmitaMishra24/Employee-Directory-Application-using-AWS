# Creating a Custom VPC with Subnets, Internet Gateway, and Route Tables

---

## Step 1: Create a New VPC

1. Log in to the **AWS Management Console** as the **Admin** user.
2. Search for **VPC** in the Services bar and open the **VPC console**.
3. In the navigation pane, under **Virtual private cloud**, select **Your VPCs**.
4. Click **Create VPC**.
5. Configure the following:
   - **Name tag**: `app-vpc`
   - **IPv4 CIDR block**: `10.1.0.0/16`
6. Click **Create VPC**.

---

## Step 2: Create an Internet Gateway and Attach It

1. Navigate to **Internet gateways**.
2. Click **Create internet gateway**.
3. Set the **Name tag** to `app-igw`, then click **Create internet gateway**.
4. After creation, choose **Actions > Attach to VPC**.
5. Select `app-vpc` and click **Attach internet gateway**.

---

## Step 3: Create Subnets

Create **four subnets** — two public and two private:

1. Go to **Subnets** and click **Create subnet**.
2. Choose `app-vpc` as the **VPC ID**.

### Public Subnet 1

- **Name**: `Public Subnet 1`
- **AZ**: `us-west-2a` (example)
- **CIDR**: `10.1.1.0/24`

### Public Subnet 2

- **Name**: `Public Subnet 2`
- **AZ**: `us-west-2b`
- **CIDR**: `10.1.2.0/24`

### Private Subnet 1

- **Name**: `Private Subnet 1`
- **AZ**: `us-west-2a`
- **CIDR**: `10.1.3.0/24`

### Private Subnet 2

- **Name**: `Private Subnet 2`
- **AZ**: `us-west-2b`
- **CIDR**: `10.1.4.0/24`

3. Click **Create subnet** once all four are added.

---

## Step 4: Enable Auto-Assign Public IP for Public Subnets

### For Public Subnet 1

1. Select `Public Subnet 1`.
2. Go to **Actions > Edit subnet settings**.
3. Enable **Auto-assign public IPv4 address**.
4. Click **Save**.

### For Public Subnet 2

1. Repeat the same steps for `Public Subnet 2`.

---

## Step 5: Create Route Tables

### Public Route Table

1. Navigate to **Route tables** > **Create route table**.
2. Set:
   - **Name**: `app-routetable-public`
   - **VPC**: `app-vpc`
3. Click **Create route table**.

#### Add Internet Gateway Route

1. Select `app-routetable-public`.
2. Under the **Routes** tab, click **Edit routes > Add route**.
   - **Destination**: `0.0.0.0/0`
   - **Target**: Internet Gateway → `app-igw`
3. Click **Save changes**.

#### Associate Public Subnets

1. Go to the **Subnet associations** tab.
2. Click **Edit subnet associations**.
3. Select:
   - `Public Subnet 1`
   - `Public Subnet 2`
4. Click **Save associations**.

---

### Private Route Table

1. Go back to **Route tables** and click **Create route table**.
2. Set:
   - **Name**: `app-routetable-private`
   - **VPC**: `app-vpc`
3. Click **Create route table**.

#### Associate Private Subnets

1. Select `app-routetable-private`.
2. Go to **Subnet associations > Edit subnet associations**.
3. Select:
   - `Private Subnet 1`
   - `Private Subnet 2`
4. Click **Save associations**.

---

## VPC Configuration Complete

You have successfully:

- Created a custom VPC: `app-vpc`
- Added and attached an Internet Gateway: `app-igw`
- Created:
  - 2 Public Subnets
  - 2 Private Subnets
- Enabled auto-assign public IP for public subnets
- Created and associated:
  - Public Route Table with Internet Access
  - Private Route Table with no external route (yet)
