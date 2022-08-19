# Connect to a Microsoft SQL Server on Amazon EC2<a name="connect-sql-server-on-ec2-instance"></a>

**Topics**
+ [SSMS](#connect-sql-server-on-ec2-instance-ssms)
+ [Configuration Manager](#connect-sql-server-on-ec2-instance-configuration-manager)

## SQL Server Management Studio \(SSMS\)<a name="connect-sql-server-on-ec2-instance-ssms"></a>

By default, only the built\-in local administrator account can access a SQL Server instance launched from an AWS Windows AMI\. You can use SQL Server Management Studio \(SSMS\) to add domain users so that they can access and manage SQL Server\.

Perform the following steps to access a SQL Server instance on Amazon EC2 as a domain user\.

1. [Connect to your instance](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/connecting_to_windows_instance.html) as a local administrator using Remote Desktop Protocol \(RDP\)\.

1. Open SQL Server Management Studio \(SSMS\)\. For **Authentication**, choose **Windows Authentication** to log in with the built\-in local administrator\.

1. Choose **Connect**\.

1. In Object Explorer, expand **Security**\.

1. Open the context menu \(right\-click\) for **Logins** then select **New Login**\.

1. For **Login name**, select **Windows authentication**\. Enter **Domain\\username**, replacing **DomainName** with your domain NetBIOS name and **username** with your Active Directory user name\.

1. On the **Server roles** page, select the [server roles](https://docs.microsoft.com/en-us/sql/relational-databases/security/authentication-access/server-level-roles?view=sql-server-2017) that you want to grant to the Active Directory user\.

1. Select the **General** page, and then choose **OK**\.

1. Log out from the instance and then log in again as a domain user\.

1. Open SSMS\. For **Authentication**, choose **Windows authentication** to log in with your domain user account\.

1. Choose **Connect**\.

## SQL Server Configuration Manager<a name="connect-sql-server-on-ec2-instance-configuration-manager"></a>

To connect to SQL Server using SQL Server Configuration Manager, see [SQL Server Configuration Manager ](https://docs.microsoft.com/en-us/sql/relational-databases/sql-server-configuration-manager?view=sql-server-ver15) in the Microsoft documentation\.