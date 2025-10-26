# Security
Of course! Based on your lab summary, here's a well-structured write-up about your security-focused database administration lab. I've organized it to highlight the security aspects, which is crucial for your AWS Cloud Practitioner exam.

---

### Lab Write-Up: Secure Database Administration in AWS

**Lab Objective:** To practice secure database administration tasks in an AWS cloud environment, emphasizing proper access control and resource management.

#### 1. Secure Access to the Command Host

**What I Did & The Security Principle:**
The first critical security step was establishing a connection to the EC2 Command Host. Instead of using traditional SSH keys or passwords that require open ports, I used **AWS Systems Manager Session Manager**.

*   **Action:** I navigated to the EC2 console, located the `Command Host` instance, and selected "Connect" -> "Session Manager".
*   **Security Significance:** Session Manager provides **secure and auditable** instance connectivity without needing to open inbound ports (like port 22 for SSH) in the security group. This follows the security best practice of **"reduce attack surface area."** All session activity is logged through AWS CloudTrail, providing an audit trail.

*(Representative Image: Connecting via Session Manager)*
![AWS EC2 Connect using Session Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/images/session-manager-connect-button.png)

#### 2. Principle of Least Privilege & Authentication

**What I Did & The Security Principle:**
Once connected to the host, I accessed the MySQL database using specific, provided credentials.

*   **Action:** I executed the command:
    ```bash
    mysql -u root --password='re:St@rt!9'
    ```
*   **Security Significance:**
    *   **Authentication:** I used the database's built-in authentication system to prove my identity as the `root` user. In a real-world scenario, you would create individual users with specific privileges instead of always using the root account.
    *   **Principle of Least Privilege:** While I used the powerful root account for this lab, the concept was demonstrated when I created and managed specific databases and tables. In practice, you would grant users only the permissions they absolutely need (e.g., `SELECT` but not `DROP`).

#### 3. Secure Resource Management with SQL

**What I Did & The Security Principle:**
The core of the lab involved using SQL commands to create, modify, and destroy database resources. This demonstrates the user's responsibility for data security "in" the cloud.

*   **Actions & Their Security Implications:**
    1.  **`CREATE DATABASE world;`** - I created a new, isolated environment for my data, preventing conflicts with other system databases.
    2.  **`CREATE TABLE country (...);`** - I defined a schema, enforcing data structure and type. I encountered and **corrected a schema error** (`ALTER TABLE country RENAME COLUMN Conitinent TO Continent;`), which is crucial for data integrity.
    3.  **Verification Queries (`SHOW DATABASES;`, `SHOW TABLES;`)** - I continuously verified my actions. This is a key operational security practice to ensure configurations are applied correctly and to detect any unintended changes.
    4.  **`DROP TABLE city;` / `DROP DATABASE world;`** - I properly decommissioned the resources. This is a critical security practice for **data lifecycle management**. Ensuring unused data containing potentially sensitive information is securely deleted prevents data leakage.

*(Representative Image: MySQL session showing database and table creation)*
![MySQL command line showing CREATE and SHOW commands](https://www.mysqltutorial.org/wp-content/uploads/2009/12/MySQL-Create-Database.png)

### Summary: Key Security Takeaways for AWS Cloud Practitioner

This lab, while focused on database administration, reinforced several vital AWS security concepts:

1.  **Secure Access:** Using **AWS Session Manager** is a more secure method for managing EC2 instances than traditional SSH.
2.  **Identity and Access Management (IAM):** The need for strong **authentication** (usernames/passwords) is fundamental, both for AWS services and the applications running on them.
3.  **Operational Excellence & Auditing:** The practice of verifying changes with `SHOW` commands aligns with the need for logging and monitoring (using services like **CloudTrail**) to maintain a secure and auditable environment.
4.  **Data Security:** You are responsible for the security of your data *in* the cloud. This includes defining database schemas, managing access within the database, and securely destroying data when it's no longer needed.

By completing this lab, I demonstrated hands-on experience with the **customer's side of the Shared Responsibility Model**, specifically in managing access, application-level security, and data lifecycle for a database hosted on an EC2 instance.
