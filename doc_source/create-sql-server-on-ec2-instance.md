# Deploy Microsoft SQL Server on Amazon EC2<a name="create-sql-server-on-ec2-instance"></a>

To launch Microsoft SQL Server using Amazon Elastic Compute Cloud \(Amazon EC2\) instances with Windows Server, perform the following steps according to your use case\.

New SQL environment deployments are classified under three categories:
+ SQL Server standalone
+ SQL Server Failover Cluster Instances \(FCI\)
+ SQL Server Always On availability groups \(AG\)

## Considerations<a name="create-sql-server-on-ec2-instance-considerations"></a>

Before you launch SQL Server on your instance, consider the following:
+ If you use an AWS provided AMI, you must initially manage SQL Server as the local administrator\. For more information, see [Connect to a Microsoft SQL Server on Amazon EC2](connect-sql-server-on-ec2-instance.md)\.
+ The built\-in availability form of clustering in Windows Server is activated by a feature named Failover Clustering\. This feature allows you to build a Windows Server Failover Cluster \(WSFC\) to use with an availability group or failover cluster instances \(FCI\)\.
+ Always On is an umbrella term for the availability features in SQL Server, and the term covers both availability groups and FCIs\. Always On isn't the name of the Always On availability group \(AG\) feature\. 
+ The major difference between FCI and AG is that all FCIs require some sort of shared storage, even if it's provided through networking\. The FCI's resources can be run and owned by one node at any given time\. AG doesn't require that shared storage is also highly available\. It's a best practice to have replicas that are local in one data center for high availability, and remote ones in other data centers for disaster recovery, each with separate storage\. 
+ An availability group also has another component called the listener\. The listener allows applications and end users to connect without needing to know which SQL Server instance is hosting the primary replica\. Each availability group has its own listener\. 

## Deployment options<a name="create-sql-server-deployment-options"></a>

Use one of the following options to deploy SQL Server on Amazon EC2\.

### Deploy SQL Server on Amazon EC2 with AWS Launch Wizard<a name="create-sql-server-on-ec2-launch-wizard"></a>

AWS Launch Wizard is a service that guides you through the sizing, configuration, and deployment of enterprise applications following AWS Cloud best practices\. AWS Launch Wizard for SQL Server supports both high availability and single instance deployments according to AWS and SQL Server best practices\. For more information, see the [AWS Launch Wizard for SQL Server User Guide](https://docs.aws.amazon.com/launchwizard/latest/userguide/what-is-launch-wizard.html)\.

**Always On availability groups \(AG\)**  
Deploy your SQL Server Always On availability groups with primary and secondary replicas for database level protection\. Each replica is hosted by a SQL Server instance with its own local storage\.

**Always On Failover Cluster Instances \(SQL FCI\)**  
Deploy SQL Server Always On using Failover Cluster Instances \(FCI\) for instance\-level protection\. A single SQL Server instance is installed across Windows Server Failover Clustering \(WSFC\) nodes to ensure high availability and storage sharing\.

Launch Wizard uses Amazon FSx to provide the following shared storage options required for SQL FCI deployments:
+ Amazon FSx for NetApp ONTAP using Microsoft iSCSI endpoint
+ Amazon FSx for Windows File Server using SMB 3\.0 continuously available Windows file share

For more information on how to deploy SQL Server with Launch Wizard, see [Deploy an application with AWS Launch Wizard for SQL Server on Windows](https://docs.aws.amazon.com/launchwizard/latest/userguide/launch-wizard-deploying.html) in the *AWS Launch Wizard User Guide*\.

### Deploy SQL Server standalone<a name="create-sql-server-on-ec2-standalone"></a>

For a SQL Server standalone deployment, you can use one of the license\-include AMIs provided by AWS or by using your own licensed media\. For a list of SQL Server AMIs provided by AWS, see [Windows AMIs](http://aws.amazon.com/windows/resources/amis/)\. For more information on licensing options, see [Licensing Microsoft SQL Server on Amazon EC2](sql-server-on-ec2-licensing.md)\.

### Deploy SQL Server failover cluster instances \(FCIs\)<a name="create-sql-server-on-ec2-fci"></a>

Failover cluster instances \(FCIs\) provide availability for the entire installation of SQL Server known as an instance\. Everything that is included in the instance, such as databases, SQL Server Agent jobs, and linked servers, move to a different server when the underlying server fails\.

You can use AWS Launch Wizard to deploy SQL Server FCIs in the AWS Cloud\. Launch Wizard identifies the AWS resources to automatically provision the SQL Server databases based on your use case\. For more information, see [Get started with AWS Launch Wizard for SQL Server](https://docs.aws.amazon.com/launchwizard/latest/userguide/launch-wizard-getting-started.html)\.

You can reference the following AWS blogs to manually deploy SQL Server FCIs using Amazon FSx:
+ [Deploy a SQL Server FCI using SMB 3\.0 Continuously Available File Shares \(CAFS\) as shared storage](http://aws.amazon.com/blogs/storage/simplify-your-microsoft-sql-server-high-availability-deployments-using-amazon-fsx-for-windows-file-server/)
+ [Deploy a SQL Server FCI using Microsoft iSCSI Initiator as shared storage](http://aws.amazon.com/blogs/modernizing-with-aws/sql-server-high-availability-amazon-fsx-for-netapp-ontap/)

### Deploy SQL Server Always On availability groups \(AG\)<a name="create-sql-server-on-ec2-always-on"></a>

Always on availability groups provide high availability and disaster recovery of user databases through data replication\. Availability groups can also distribute read operations amongst member nodes\.

You can use [AWS Launch Wizard](#create-sql-server-on-ec2-launch-wizard) to deploy a SQL Server Always On availability group in the AWS Cloud\. Launch Wizard identifies the AWS resources to automatically provision the SQL Server databases based on your use case\. For more information, see [Get started with AWS Launch Wizard for SQL Server](https://docs.aws.amazon.com/launchwizard/latest/userguide/launch-wizard-getting-started.html)\.

To manually deploy a SQL Server Always On availability group, perform the following steps:

**Prerequisites**  
Before you manually deploy a SQL Server Always On availability group, you must perform the following prerequisites\.
+ Launch two Amazon EC2 Windows Server instances \(Windows Server 2012 or later\) across two Availability Zones within an Amazon VPC\.
+ Install SQL Server 2014 or later 64\-bit Enterprise edition\. For testing, use SQL Server 2014 or later 64\-bit Evaluation edition\.
+ Configure secondary Amazon EBS volumes to host SQL Server Master Data File \(MDF\), Log Data File \(LDF\), and SQL Backup files \(\.bak\)\.
+ Deploy the cluster nodes in private subnets\. You can then use Remote Desktop Protocol \(RDP\) to connect from a jump server to the cluster node instances\.
+ Configure inbound security group rules and [Windows firewall exceptions](https://docs.microsoft.com/en-us/sql/sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access?view=sql-server-2017) to allow the nodes to communicate in a restrictive environment\.
+ Open all necessary ports for Active Directory domain controllers so that the SQL nodes and witness can join the domain and authenticate against Active Directory\.
+ Join the nodes to the domain before you create the Windows failover cluster\. Ensure that you are logged in with domain credentials before you create and configure the cluster\.
+ Run the SQL Database instances with an Active Directory service account\.
+ Create a SQL login with `sysadmin` permissions using Windows domain authentication\. Consult with your database administrator for details\. For more information, see [Create a login using SSMS for SQL Server](https://docs.microsoft.com/en-us/sql/relational-databases/security/authentication-access/create-a-login?view=sql-server-ver15#SSMSProcedure) in the Microsoft documentation\.
+ Properly configure the SQL browser for SQL Server named instances\.

**Configure the secondary IPs for each cluster node elastic network interface**

Two secondary IP addresses are required for each cluster node elastic network interface\.
**Note**  
If you do not plan to deploy a SQL Group Listener, add only one secondary IP address for each cluster node elastic network interface\.

1. Navigate to the [Amazon EC2 console](https://console.aws.amazon.com/ec2) and choose the AWS Region where you want to host your Always On cluster\.

1. Choose **Instances** from the left navigation pane, and then select your Amazon EC2 cluster instance\.

1. Choose the **Networking** tab\.

1. Under **Network interfaces**, select the network interface and then choose **Actions** > **Manage IP addresses**\.

1. Choose the network interface Id to open the expandable section, and then choose **Assign new IP address**\. You can enter a specific IP address or keep the default entry as `Auto-assign`\. Repeat this step to add a second new IP address\.

1. Choose **Save** > **Confirm**\.

1. Repeat steps 1 through 7 for the other Amazon EC2 instance that will be included in the cluster\.

**Create a two\-node Windows cluster**

Perform the following steps to create a two\-node Windows cluster\.

1. [Connect to your Amazon EC2 instance](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/connecting_to_windows_instance.html) with RDP, using a domain account with local administrator permissions on both nodes\.

1. On the Windows **Start** menu, open **Control Panel**, and then choose **Network and Internet** > **Network and Sharing Center**\.

1. Choose **Change adapter settings** from the left navigation pane\.

1. Choose your network connection, and then choose **Change settings of this connection**\.

1. Choose **Internet Protocol Version 4 TCP/IPV4\), and then choose **Properties**\.**

1. Choose **Advanced**\.

1. Under the **DNS** tab, choose **Append primary and connection specific DNS suffixes\.**

1. Choose **OK** > **OK** > **Close**\.

1. Repeat steps 1 through 8 for the other Amazon EC2 instance to include in the cluster\. 

1. On each instance, install the cluster feature on the nodes from the Server Manager, or run the following Windows PowerShell command:

   ```
   Install-WindowsFeature –Name Failover-Clustering –IncludeManagementTools
   ```

1. Open the command line as an administrator and enter `cluadmin.msc` top open the Cluster Manager\.

1. Open the context menu \(right\-click\) for **Failover Cluster Manager**, and then choose **Create Cluster**\.

1. Choose **Next** > **Browse**\.

1. For **Enter the object names to select**, enter the cluster node hostnames, and then choose **OK**\.

1. Choose **Next**\. You can now choose whether to validate the cluster\. We recommend that you perform a cluster validation\. If the cluster does not pass validation Microsoft may be unable to provide technical support for your SQL cluster\. Choose **Yes** or **No**, and then choose **Next**\.

1. Enter a **Cluster Name**, and then choose **Next**\.

1. Clear **Add all eligible storage to the cluster**, and then choose **Next**\.

1. When the cluster creation is complete, choose **Finish**\.
**Note**  
Cluster logs and reports are located at `%systemroot%\cluster\reports`\.

1. In the **Cluster Core Resources** section of Cluster Manager, expand the entry for your new cluster\.

1. Open the context menu \(right\-click\) for the first IP address entry, and then choose **Properties**\. For **IP Address** , choose **Static IP Address**, and then enter one of the secondary IP addresses associated with the `eth0` elastic network interface\. Choose **OK**\. Repeat this step for the second IP address entry\. 

1. Open the context menu \(right\-click\) for the cluster name, and then choose **Bring Online**\.

**Note**  
We recommend that you configure a [File Share Witness \(FSW\)](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770620(v=ws.10)) in addition to your cluster to act as a tie\-breaker\. You can also use [Amazon FSx for Windows File Server with Microsoft SQL Server](https://docs.aws.amazon.com/fsx/latest/WindowsGuide/sql-server.html)\.

**Create Always On availability groups**

Perform the following steps to create Always On availability groups\.

1. Open SQL Server Configuration Manager\.

1. Open the context menu \(right\-click\) for the SQL instance, and then choose **Properties**\.

1. On the **AlwaysOn High Availability** tab, select **Enable AlwaysOn Availability Groups**, and then choose **Apply**\.

1. Open the context menu \(right\-click\) for the SQL instance, and then choose **Restart**\.

1. Repeat steps 1 through 4 on the other cluster node to include in the cluster\.

1. Open Microsoft SQL Server Management Studio \(SSMS\)\.

1. Log in to one of the SQL instances with your Windows authenticated login that has access to the SQL instance\.
**Note**  
We recommend that you use the same MDF and LDF directory file paths across the SQL instances\.

1. Create a test database\. Open the context menu \(right\-click\) for **Databases**, and then choose **New Database**\. 
**Note**  
Make sure that you use the **Full** [recovery model](https://docs.microsoft.com/en-us/sql/relational-databases/backup-restore/recovery-models-sql-server?view=sql-server-2017) on the **Options** page\.

1. Enter a **Database name**, and then choose **OK**\.

1. Open the context menu \(right\-click\) for the new database name, choose **Tasks**, and then choose **Back Up** For **Backup type**, choose **Full**\. 

1. Choose **OK** > **OK**\.

1. Open the context menu \(right\-click\) for **Always On High Availability** and then choose **New Availability Group Wizard**\.

1. Choose **Next**\.

1. Enter an **Availability group name**, and then choose **Next**\.

1. Select your database, and then choose **Next**\.

1. A primary replica is already present in the Availability Replicas window\. Choose **Add Replica** to create a secondary replica\.

1. Enter a **Server name** for the secondary replica and then choose **Connect**\.

1. [Decide which **Availability Mode** you want to use](https://docs.microsoft.com/en-us/sql/database-engine/availability-groups/windows/availability-modes-always-on-availability-groups?view=sql-server-2017) for each replica, and then choose either **Synchronous commit** or **Asynchronous commit**\.

1. Choose **Next**\.

1. Choose your [data synchronization preference](https://docs.microsoft.com/en-us/sql/database-engine/availability-groups/windows/select-initial-data-synchronization-page-always-on-availability-group-wizards?view=sql-server-2017), and then choose **Next**\.

1. When the validation is successful, choose **Next**\.
**Note**  
You can safely ignore **Checking the listener configuration** because you will add it later\.

1. Choose **Finish** > **Close**\.

**Add a SQL Group Listener**

Perform the following steps to add a SQL Group Listener\.

1. Open SQL Server Management Studio \(SSMS\) and expand **Always On High Availability, Availability Groups, <primary replica name>**\.

1. Open the context menu \(right\-click\) for **Availability Group Listeners** and then choose **Add Listener**\. Enter a **DNS Name**\.

1. Enter **Port** `1443`\.

1. Choose **Static IP** for **Network Mode**\.

1. Choose **Add**\.

   For the **IPv4 Address**, enter the second secondary IP address from one of the cluster node instances, and then choose **OK**\. Repeat this step using the second secondary IP address from the other cluster node instance\.

1. Choose **OK**\.

**Note**  
If you receive errors when you add a SQL Group Listener, you may be missing permissions\. For troubleshooting see:  
[Troubleshooting AlwaysOn availability group listener creation in SQL Server 2012](https://support.microsoft.com/en-us/topic/kb2829783-troubleshooting-alwayson-availability-group-listener-creation-in-sql-server-2012-42b42543-3c4b-49e3-14fc-5bc76e7eec89)
[Create Availability Group Listener Fails with Message 19471, ‘The WSFC cluster could not bring the Network Name resource online’](https://docs.microsoft.com/en-us/archive/blogs/alwaysonpro/create-availability-group-listener-fails-with-message-19471-the-wsfc-cluster-could-not-bring-the-network-name-resource-online)

**Test failover**

1. From SSMS, open the context menu \(right\-click\) for the primary replica on the navigation menu, and then choose **Failover**\.

1. Choose **Next** > **Next**\.

1. Choose **Connect** > **Connect**\.

1. Choose **Next**, and then choose **Finish**\. The primary replica will become the secondary replica after failover\.