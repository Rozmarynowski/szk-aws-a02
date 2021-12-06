<img src="https://welastic.pl/wp-content/uploads/2020/05/cropped-welastic_logo-300x259.png" alt="Welastic logo" width="100" align="left">
<br><br>
<br><br>
<br><br>

# EC2 Systems Manager - Run Command

## LAB Overview

#### In this lab you will create use EC2 System Manager to restore ssh connection to your virtual machine.

## Task 1: Create IAM Role for SSM.

By default, AWS Systems Manager doesn't have permission to perform actions on your instances. You must grant access by using an AWS Identity and Access Management (IAM) instance profile. 

1.  In the AWS Management Console, on the **Services** menu, click **IAM**.
2. In the navigation pane on the left, click **Roles**.
3. Click **Create role** button.
4. For **Select type of trusted entity** choose the **AWS serivce** and for **Choose a use case**  select EC2 and click **Next: Permissions** button.
5. In **Search** box paste **AmazonEC2RoleforSSM**, and select a policy.
6. Click **Next:Tags** and **Next: Review**.
7. Insert the **Role name**: **StudentX-SSMRole**.
8. Click **Create role**.

## Task 2: Create new EC2 instance

In this task you will spin up a new EC2 machine without SSH key.

1.  On the **Services** menu, click **EC2**.
2.  Click **Launch** **Instance**.
3.  Select **Amazon Linux 2 AMI.**
4.  On the **Choose an Instance Type** page, select the **t2.micro** instance type.
5.  Click **Next: Configure Instance Details**.
6.  On the **Configure Instance Details** page select LAB-VPC and subnet and enable public IP.
7.  Additionally set up IAM Role that you created in task 1.
8.  Click **Next: Add Storage**.
9.  Click **Next: Add Tags** to accept the default storage device configuration.
10. On the **Add Tags** page, click **Add Tag,** type a **Name** for a Key box and  studentX_NoKey in the Value  box.
11. Click **Next: Configure Security Group**.
12. For **Assign a security group**, select a WebServers Security Group.
13. Click **Review and Launch**.
14. Review your choices, and then click **Launch**.
15. Select **Proceed without a key pair** and check Acknowledge box.
16. On the status page, which notifies you that your instances are launching, click **View Instances**.
17. Wait unitl install will be ready.
18. Copy the **IPv4 Public IP** value to notepad. 

## Task 3: Test EC2 SSM.

1. On the **Services** menu, click **AWS Systems Manager**.
2. On the left, click **Run Command**.
3. Click **Run Command** button.
4. For **Command document**, choose **AWS-RunShellScript**.
5. For **Commands**, type **ps aux**.
6. For **Target**, Choose instances manually. If you don't see the instance, verify that you are currently in the same region as the instance you created. Also verify that you configured the IAM role and trust policies as described earlier.

7.  For **Comment**, type StudentX-ps.
8.  Choose **Run** to execute the command. Run Command displays a status screen. Choose **View result**.
9.  To view the output, choose the instance Id and unwind output 

## Task 4: Retrive a public key from you key pair.

You need you public key that needs to be imported into orphaned VM.

1. On your local Linux or Mac computer or Cloud9 console, you can use the **ssh-keygen** command to retrieve the public key for your key pair. 

```she
ssh-keygen -y -f /path_to_key_pair/my-key-pair.pem
```

The command returns the public key. For example:

```she
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQClKsfkNkuSevGj3eYhCe53pcjqP3maAhDFcvBS7O6V
hz2ItxCih+PnDSUaw+WNQn/mZphTk/a/gU8jEzoOWbkM4yxyb/wB96xbiFveSFJuOp/d6RJhJOI0iBXrlsLnBItntckiJ7FbtxJMXLvvwJryDUilBMTjYtwB+QhYXUMOzce5Pjz5/i8SeJtjnV3iAoG/cQk+0FzZqaeJAAHco+CY/5WrUBkrHmFJr6HcXkvJdWPkYQS3xqC0+FmUZofz221CBt5IMucxXPkX4rWi+z7wB3RbBQoQzd8v7yeb7OzlPnWOyN0qFU0XA246RA8QFYiCNYwI3f05p6KLxEXAMPLE
```

2. (Optionally) You can retrive a key from your other instance (with the same key) through its instance metadata:

```shell
[ec2-user ~]$ curl http://169.254.169.254/latest/meta-data/public-keys/0/openssh-key
```

## Task 5: Use SSM to import a public key.

During this task you will use SSM to import a public key to the virtual machine. After that you will try to connect.

1. Repeat a steps from **Task 3** 1-4.
2. For **Command**, paste the following command

```shell
echo 'key name' >> /home/ec2-user/.ssh/authorized_keys
```

For example:

```she
echo 'ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCwfPwfUU9KjUYpuia6wcAsXrbetJOS/T7yOvJOukK/78CQT4xOUteHi36w4KOxzanDsVYg/jdU2RoXLZ5cof05TJKU7QgD3Ahhnm+D2g97wkjeBz34fjyqUJlivMlmpxt1zzSMXQh+PYHXC1d4bhJFE0QY4jMGq4fNbg+VBjIf7gYVoj1DB+DMxpGlz8JtEBmpYl6bhp6gqe9916laMEOh6Ja9RJJCvmWCm0PSGg2aeKRCdDjzGLiRZEGGva4hixRJ5/Ob69h4bsJuVCYf6FS7tJEfaZiYgcuVWuxkYcNO7VPQqumXUvuotFTL/kehBRPkGsnW21fsi9lAVwb4s7Qt klucz' >> /home/ec2-user/.ssh/authorized_keys
```

3. For **Comment**, type StudentX-addKey.
4. Choose **Run** to execute the command.

Check if the key has been correctly imported.

5. Repeat a steps from **Task 3** 1-4.
6. For **Command**, paste the following command:

```she
cat /home/ec2-user/.ssh/authorized_keys
```

7.  For **Comment**, type StudentX-check.
8.  Choose **Run** to execute the command. Run Command displays a status screen. Choose **View result**.
9.  To view the output, choose the command invocation for the command, choose the **Output** tab, and then choose **View Output**.
10. If the key is correctly imported try to connect over SSH.

## Task 6: Check access to EC2 instance.
1. Open your favourite Terminal application.
2. Connect to your EC2 instance by typing: **ssh -i** <path_to_your_pem_file> **ec2-user@**<public IP from Task 2 step 18>. 

## END LAB

This is the end of the lab. Terminate instance your created in this LAB (studentX_NoKey).

47. In AWS console select your **studentX** EC2 instance.
48. Click **Action** button and select **Instance state** -> **Terminate.**
49. Click **Yes, Terminate.**

<br><br>

<p align="right">&copy; 2021 Welastic Sp. z o.o.<p>