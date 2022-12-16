# Licensing considerations<a name="sql-server-on-ec2-licensing-considerations"></a>

There are many considerations for cost effectively licensing your Microsoft SQL Server on Amazon Elastic Compute Cloud \(Amazon EC2\) workload\. Your use case, and existing license agreements, will determine whether to bring your own license to AWS with the Bring Your Own License model \(BYOL\) or to use license included AMIs from AWS\. The following topics should help determine which approach you might take\. For more information, see [Licensing – SQL Server](http://aws.amazon.com/windows/faq/#licensing-sql-q) on the *Amazon Web Services and Microsoft Frequently Asked Questions* page\.

**Topics**
+ [Choose a SQL Server edition](#sql-server-on-ec2-licensing-considerations-editions)
+ [Purchase SQL Server from AWS](#sql-server-on-ec2-licensing-considerations-purchasing)
+ [Use BYOL for SQL Server on AWS](#sql-server-on-ec2-licensing-considerations-byol)
+ [Quantify license requirements](#sql-server-on-ec2-licensing-considerations-quantify)
+ [License Mobility with SQL Server](#sql-server-on-ec2-licensing-considerations-mobility)
+ [Track BYOL license consumption](#sql-server-on-ec2-licensing-considerations-track)
+ [SQL Server CALs](#sql-server-on-ec2-licensing-considerations-cals)
+ [Licensing for passive failover](#sql-server-on-ec2-licensing-considerations-failover)

## Choose a SQL Server edition<a name="sql-server-on-ec2-licensing-considerations-editions"></a>

The edition of SQL Server that is used will determine the supported features your implementation will have available\. For example, the edition determines the maximum compute capacity used by a single instance of the SQL Server Database Engine, and the high availability options you might implement\. For a comparison of SQL Server editions and supported features, see [Editions and supported features of SQL Server 2022](https://learn.microsoft.com/en-us/sql/sql-server/what-s-new-in-sql-server-2022?view=sql-server-ver16) in the Microsoft documentation\.

## Purchase SQL Server from AWS<a name="sql-server-on-ec2-licensing-considerations-purchasing"></a>

You can utilize Microsoft SQL Server licenses included from AWS\. You can choose any of the following editions for your use on Amazon EC2 instances\.
+ SQL Server Express
+ SQL Server Web
+ SQL Server Standard
+ SQL Server Enterprise

**Note**  
SQL Server Developer edition is eligible for use in non\-production, development, and test workloads\. Once downloaded from Microsoft, you can bring and install SQL Server Developer edition on Amazon EC2 instances in the AWS Cloud\. Dedicated infrastructure is not required for SQL Server Developer edition\. For more information, see [https://www.microsoft.com/en-us/sql-server/sql-server-downloads](https://www.microsoft.com/en-us/sql-server/sql-server-downloads)\.

## Use BYOL for SQL Server on AWS<a name="sql-server-on-ec2-licensing-considerations-byol"></a>

You can use BYOL licenses for SQL Server on AWS\. The requirements differ depending on if the licenses have active Software Assurance\.

**SQL Server licenses with active Software Assurance**  
You can bring your SQL Server licenses with active Software Assurance to default \(shared\) tenant Amazon EC2 through License Mobility benefits\. Microsoft requires that you complete and send a License Mobility verification form which can be downloaded [here](https://www.microsoft.com/en-us/licensing/licensing-programs/software-assurance-license-mobility?activetab=software-assurance-license-mobility-pivot%3aprimaryr2)\. For more information, see [License Mobility](http://aws.amazon.com/windows/resources/licensemobility/)\.

**SQL Server licenses without active Software Assurance**  
SQL Server licenses without Software Assurance can be deployed on Amazon Elastic Compute Cloud Dedicated Hosts if the licenses are purchased prior to 10/1/2019 or added as a true\-up under an active Enterprise Enrollment that was effective prior to 10/1/2019\. In these specific BYOL scenarios, the licenses can only be upgraded to versions that were available prior to 10/1/2019\. For more information, see [Dedicated Hosts](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/dedicated-hosts-overview.html) in the *Amazon EC2 User Guide*, and the [Amazon EC2 Dedicated Hosts FAQs](http://aws.amazon.com/ec2/dedicated-hosts/faqs/)\.

## Quantify the required SQL Server licenses for BYOL<a name="sql-server-on-ec2-licensing-considerations-quantify"></a>

If you are licensing SQL Server under Microsoft License Mobility through Software Assurance, the number of licenses required varies based on the instance type, version of SQL Server, and the Microsoft licensing model you choose\. For assistance with virtual core licensing calculations under the Microsoft Product Terms based on the instance type, see [SQL License Mobility](http://aws.amazon.com/windows/resources/licensemobility/sql/)\.

If you are using Dedicated Hosts, Amazon EC2 provides you with the number of physical cores installed on the Dedicated Host\. Using this information, you can calculate the number of SQL Server licenses that you need to bring in\. For more information, see [Amazon EC2 Dedicated Hosts Pricing](http://aws.amazon.com/ec2/dedicated-hosts/pricing/#host-configuration) and the [SQL Server 2022 licensing guide](https://download.microsoft.com/download/9/3/d/93d32de6-f268-45ed-ba25-2f9a6756b6af/SQL_Server_2022_Licensing_guide.pdf)\.

## License Mobility with SQL Server<a name="sql-server-on-ec2-licensing-considerations-mobility"></a>

SQL Server licenses with active Software Assurance are eligible for Microsoft License Mobility and can be deployed on default or dedicated tenant Amazon EC2\. For more information on bringing SQL Server licenses with active Software Assurance to default tenant EC2, see [Microsoft License Mobility](http://aws.amazon.com/windows/resources/licensemobility/)\.

It is also possible to bring SQL Server licenses without active Software Assurance to EC2 Dedicated Hosts\. To be eligible, the licenses must be purchased prior to October 1, 2019 or added as a true\-up under an active Enterprise Enrollment that was effective prior to October 1, 2019\. For additional FAQs about Dedicated Hosts, see the [Dedicated Hosts](http://aws.amazon.com/windows/faq/#dedicated-hosts) section of the *Amazon Web Services and Microsoft FAQ*\.

## Track BYOL license consumption<a name="sql-server-on-ec2-licensing-considerations-track"></a>

You can use AWS License Manager to manage your software licenses for SQL Server\. With License Manager, you can create license configurations, take inventory of your license\-consuming resources, associate licenses with resources, and track inventory and compliance\. For more information, see [What is AWS License Manager?](https://docs.aws.amazon.com/license-manager/latest/userguide/license-manager.html) in the *AWS License Manager User Guide*\.

## SQL Server client access licenses \(CALs\)<a name="sql-server-on-ec2-licensing-considerations-cals"></a>

When you are using SQL Server on Amazon EC2, license included instances do not require client access licenses \(CALs\) for SQL Server\. An unlimited number of end users can access SQL Server on a license\-included instance\.

When you bring your own SQL Server licenses to Amazon EC2 through Microsoft License Mobility or BYOL, you must continue to follow the licensing rules in place on\-premises\. If you purchased SQL Server under the Server/CAL model, you still require CALs to meet Microsoft licensing requirements, but these CALs would remain on\-premises and enable end user access SQL Server running on AWS\.

## Licensing for passive failover<a name="sql-server-on-ec2-licensing-considerations-failover"></a>

There are various factors to consider when licensing passive failover for SQL Server\. The information in this section pertains only to the SQL Server licenses and not the Windows Server licenses\. In all cases, you must license Windows Server\.

**Using instances that include the license for SQL Server**  
When you purchase SQL Server license included instances on EC2, you must license passive failover instances\.

**Bringing SQL Server licenses with active Software Assurance to default tenant Amazon EC2**  
When you bring SQL Server 2014 and later versions with Software Assurance to default tenant EC2, you must license the virtual cores \(vCPUs\) on the active instance\. In return, Software Assurance permits one passive instance \(equal or lesser size\) where SQL Server licensing is not Amazon EC2 Dedicated Hosts required\.

**Bringing SQL Server to Amazon EC2 Dedicated Instances**  
SQL Server 2014 and later versions require Software Assurance for SQL Server passive failover benefits on dedicated infrastructure\. When you bring SQL Server with Software Assurance, you must license the cores on the active instance/host and are permitted one passive instance/host \(equal or lesser size\) where SQL Server licensing is not required\.

SQL Server 2008 \- SQL Server 2012R2 are eligible for passive failover on an Amazon EC2 Dedicated Hosts infrastructure without active Software Assurance\. In these scenarios, you will license the active instance/host, and it will be permitted one passive instance/host of equal or lesser size where SQL Server licensing is not required\.

There are specific BYOL scenarios that do not require Microsoft License Mobility through Software Assurance\. An Amazon EC2 Dedicated Hosts infrastructure is always required in these scenarios\. To be eligible, the licenses must be purchased prior to October 1, 2019 or added as a true\-up under an active Enterprise Enrollment that was effective prior to October 1, 2019\. In these specific BYOL scenarios, the licenses can only be upgraded to versions that were available prior to October 1, 2019\. 