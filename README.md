# 🧑‍💼 Employee Directory Application using AWS

This is a full-stack **Employee Directory Management** application hosted on an EC2 instance and powered by AWS cloud services. The application allows users to add, view, update, and delete employee details such as name, location, email, hobbies, contact, and profile photo.

## 📌 Features

- Add new employees with details: **Name**, **Location**, **Email**, **Hobbies**, **Contact**, and **Photo**
- View a dynamic list of all employees
- Upload and update employee profile images
- Delete employee records
- AWS-integrated backend for data and image storage

## ☁️ AWS Services Used

- **Amazon EC2 Instance** – Compute resource to host and run the application  
- **AWS IAM** – Security and access management between services  
- **Amazon S3** – Storage for employee profile images  
- **Amazon DynamoDB** – NoSQL database to store employee records  
- **Amazon RDS DB Instance** – Relational database service (if used)  
- **Amazon CloudWatch** – Monitoring and logging (optional)


## ✅ How It Works

1. Application is launched on an EC2 public IP  
2. User fills out the employee form via the web UI  
3. Data is saved in **Amazon DynamoDB**, and images are uploaded to **Amazon S3**  
4. The UI dynamically displays all entries using data from DynamoDB and S3  
5. Users can update or delete employee records

## 🚀 Deployment Instructions

1. Launch an EC2 instance and deploy the application  
2. Create a DynamoDB table named `Employees` with fields: `id`, `name`, `location`, `email`, `hobbies`, `contact`, `photo`  
3. Create an S3 bucket and allow public access for employee images  
4. Configure IAM roles to grant EC2 access to S3 and DynamoDB  
5. (Optional) Set up Amazon CloudWatch for monitoring  
6. Run the app and access it using the EC2 public IP

## 🖼️ Screenshots

<details>
<summary>🔻 Initial Setup (Before Integrating DynamoDB and S3)</summary>

![Initial Setup](https://github.com/user-attachments/assets/d3d355c7-b25e-45b1-91eb-a6d33f483c49)

</details>

<details>
<summary>✅ Employee Added Successfully</summary>

![Employee Added](https://github.com/user-attachments/assets/bcf3c786-7119-4d40-9ed7-42accd0439e1)

</details>

<details>
<summary>👥 Multiple Employees with Photos</summary>

![Multiple Employees](https://github.com/user-attachments/assets/fb5a4ac2-7d9f-42f9-aad2-c0fd1f13b6b1)

</details>

<details>
<summary>🧾 Deleted Employee – Updated View</summary>

![Deleted Employees](https://github.com/user-attachments/assets/dc188cff-360a-4f76-85fa-8604981dbd8c)

</details>

<details>
<summary>🔄 Employee Update Success</summary>

![Updated Employees](https://github.com/user-attachments/assets/06db0dc0-d7ec-46e5-9b31-52d57650749c)

</details>

<details>
<summary>🔍 DynamoDB Table Entries</summary>

![DynamoDB View](https://github.com/user-attachments/assets/f7cf2f7a-c6ed-4e8f-8cb3-76097876e93e)

</details>

## 📝 Notion Documentation

[Create an Employee Directory Application using AWS](https://shining-buffet-929.notion.site/Create-a-Employee-Directory-Application-using-AWS-216e8249092f8021bd09ccd9f7724f75?source=copy_link)

## 📬 Contact

Made by **Asmita Mishra**
- 🔗 [LinkedIn](https://www.linkedin.com/in/asmitamishra1/)  
- 💻 [GitHub](https://github.com/AsmitaMishra24)  

🙋‍♀️ **Have a suggestion or found a bug?**  
- Feel free to [**raise an issue**](https://github.com/AsmitaMishra24/Employee-Directory-Application-using-AWS/issues) in this repository.

📩 **Want to connect or collaborate?**
- Feel free to reach out via [LinkedIn](https://www.linkedin.com/in/asmitamishra1/)
