# Microsoft SQL Server on Amazon EC2 concepts<a name="sql-server-on-ec2-concepts"></a>

The following concepts introduce you to the fundamental terminology used when working with Microsoft SQL Server on Amazon Elastic Compute Cloud \(Amazon EC2\) instances:
+  [Amazon Machine Images (AMIs)](#ami)
+  [Backup](#backup)
+  [Billing](#billing)
+  [High availability and disaster recovery (HADR)](#ha)
+  [Instance](#instance)
+  [Instance types](#instance-types)
+  [Launching](#launching)
+  [Security](#sec)
+  [Storage](#storage)

**Amazon Machine Images \(AMIs\)**  
SQL Server on Amazon EC2 instances are created from Amazon Machine Images \(AMIs\)\. AMIs are similar to templates\. SQL Server on Amazon EC2 AMIs are pre\-installed with an operating system, typically Microsoft Windows Server, and other software\. Together, these determine the operating environment\. You can select an AMI provided by AWS, create your own AMI, or select an AMI from the AWS Marketplace\. To find a SQL Server on Amazon EC2 AMI, see the options under [Find a Windows AMI](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/finding-an-ami.html) in the *Amazon EC2 User Guide*\.

**Backup**  
Your backup and recovery design for SQL Server on Amazon EC2 is flexible, depending on your RTO and RPO requirements\. AWS provides the ability to perform server\-level backups using Windows Volume Shadow Copy Service \(VSS\)\-enabled Amazon Elastic Block Store \(Amazon EBS\) snapshots and with AWS Backup\. You can also perform database\-level backups using [native backup and restore procedures](https://docs.microsoft.com/en-us/sql/relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases?view=sql-server-ver16) for SQL Server databases\. Database\-level backups can be stored on Amazon EBS, FSx for Windows File Server, or Amazon Simple Storage Service using AWS Storage Gateway\. For more information about backing up SQL Server on Amazon EC2, see [Backup and restore options for SQL Server on Amazon EC2](https://docs.aws.amazon.com/prescriptive-guidance/latest/sql-server-managing-on-aws/welcome.html) in the *AWS Prescriptive Guidance*\.

**Billing**  
A SQL Server on Amazon EC2 instance is charged by the second, with a minimum of 1 minute\. Applied rates are based on the type and size of the selected instance, the edition of SQL Server when using a license\-included instance, along with the cost of any additional services, such as storage or networking\. AWS provides a variety of instance families that are favorable to the performance requirements of SQL Server workloads\.

You can rent an instance based on your unique CPU, memory, and storage throughput requirements\. You can also stop or terminate an instance at any time to pause or stop billing for the instance\. The main advantage of the On\-Demand model is the ability to save on CAPEX when an instance is no longer required\.

**Warning**  
Any data on [Amazon EC2 instance store](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/InstanceStorage.html) volumes are lost if your instance is stopped or terminated\. You'll still incur costs for EBS volumes when your instance is stopped\. For more information, see [Stop and start your instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Stop_Start.html) in the *Amazon EC2 User Guide for Windows Instances*\.

**High availability and disaster recovery \(HADR\)**  
You can take advantage of Windows Server Failover Cluster for high availability and disaster recovery \(HADR\) with SQL Server on Amazon EC2\. SQL Server on Amazon EC2 supports both failover cluster instances \(SQL FCIs\) and Always On availability groups \(AG\)\.

For more information see [How do I create a SQL Server Always On availability group cluster in the AWS Cloud?](http://aws.amazon.com/premiumsupport/knowledge-center/ec2-windows-sql-server-always-on-cluster/) in the AWS knowledge center\.

**Instance**  
A SQL Server on Amazon EC2 instance is a virtual \(or bare metal\) server that runs in the AWS Cloud\.

A SQL Server on Amazon EC2 instance is provisioned on demand\. The subscriber rents the virtual server by the hour/minute/second, and can use it to deploy specific configurations of SQL Server\. For more information about On\-Demand instances, see [On\-Demand instances](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/ec2-on-demand-instances.html) in the *Amazon EC2 User Guide*\.

An Amazon EC2 Dedicated Hosts is a physical server with EC2 instance capacity that is fully dedicated to your use\. Dedicated Hosts allow you to use your existing per\-socket, per\-core, or per\-VM Microsoft SQL Server software licenses\. For more information about Dedicated Hosts, see [Dedicated Hosts](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/dedicated-hosts-overview.html) in the *Amazon EC2 User Guide*\.

**Instance types**  
AWS provides various types of instances with different CPU, memory, storage, and networking configurations to support your application requirements\. Each instance type is available in various sizes to address specific workload requirements\. Instance types are grouped into families according to target application profiles, such as general purpose, compute\-optimized, memory\-optimized, and storage\-optimized\. The memory\-optimized family of instances is a popular choice for SQL Server on Amazon EC2 because instances in this family have a high memory to CPU ratio for optimal performance\. You can choose bare metal instances to support capabilities such as [Always Encrypted with secure enclaves on Amazon EC2 bare metal instances](http://aws.amazon.com/blogs/modernizing-with-aws/sql-server-always-encrypted-with-secure-enclaves/)\. For more information about individual and families of instance types, see [Amazon EC2 Instance Types](http://aws.amazon.com/ec2/instance-types/) in the AWS product pages\.

**Launching SQL Server on Amazon EC2**  
SQL Server on Amazon EC2 instances can be launched directly from the [Amazon EC2 console](https://console.aws.amazon.com/ec2), with AWS CloudFormation, by using [AWS Tools for PowerShell](https://docs.aws.amazon.com/powershell/index.html), or by using the [AWS CLI](https://docs.aws.amazon.com/cli/index.html)\. For a guided deployment of Microsoft SQL Server, use [AWS Launch Wizard](https://docs.aws.amazon.com/launchwizard/latest/userguide/launch-wizard-sql.html)\.

**Security**  
AWS supports all security standards and compliance certifications, such as PCI\-DSS, HIPAA/HITECH, FedRAMP, GDPR, FIPS, FIPS 140\-2, and more\. These standards enable you to build a fully compliant application on Amazon EC2\. AWS also supports all SQL Server security features such as [Transparent Data Encryption](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/transparent-data-encryption?view=sql-server-ver16) \(TDE\) and [Always Encrypted with Secure Enclaves](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/always-encrypted-enclaves?view=sql-server-ver16) \(when using bare metal instances\)\.

Security and compliance is a shared responsibility between you and AWS\. This shared model helps to relieve your operational burden because AWS operates, manages, and controls the components from the host operating system and virtualization layer to the physical security of the facilities in which the service operates\.

For SQL Server on Amazon EC2, you assume responsibility and management of the guest operating system, including updates and security patches, other associated application software, and the configuration of AWS provided security group firewalls\.

For more information about the shared responsibility model, see [Shared Responsibility Model](http://aws.amazon.com/compliance/shared-responsibility-model/)\.

**Storage**  
AWS provides many storage options to host your database files\. In addition to EBS volume types, you can attach volumes to SQL Server on Amazon EC2 instances using an Amazon FSx managed file system service, such as FSx for Windows File Server and Amazon FSx for NetApp ONTAP\. Some instance types provide an Amazon EC2 instance store which provides temporary block level storage on NVMe solid state drive \(SSD\) disks that are physically attached to the host computer\. For more information, see [Best practices for deploying Microsoft SQL Server on Amazon EC2](https://docs.aws.amazon.com/prescriptive-guidance/latest/sql-server-ec2-best-practices/welcome.html) in the *AWS Prescriptive Guidance*\.