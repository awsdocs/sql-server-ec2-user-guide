# Licensing options<a name="sql-server-on-ec2-licensing-options"></a>

You can launch Amazon Elastic Compute Cloud \(Amazon EC2\) instances with Microsoft SQL Server licenses included from AWS, or you can bring your own SQL Server licenses for use on AWS\. You can perform a license type conversion for SQL Server in certain configurations if your needs change\. For the most license flexibility, you can import your VM into AWS\. For more information, see [Eligible license types for license type conversion](https://docs.aws.amazon.com/license-manager/latest/userguide/conversion-types.html) in the *AWS License Manager User Guide*\.

**Topics**
+ [License\-included](#sql-server-on-ec2-licensing-options-included)
+ [BYOL](#sql-server-on-ec2-licensing-options-byol)

## License\-included<a name="sql-server-on-ec2-licensing-options-included"></a>

Windows Server with currently supported versions of Microsoft SQL Server AMIs are available from AWS in a variety of combinations\. AWS provides these AMIs with SQL Server software and operating system updates already installed\. When you purchase an Amazon EC2 instance with a Windows Server AMI, licensing costs and compliance are handled for you\. For more information, see [Find a SQL Server license\-included AMI](sql-server-on-ec2-amis.md)\.

Amazon EC2 offers a variety of instance types and sizes that you can configure for your target workload\. Amazon EC2 AMIs with Windows Server require no Client Access Licenses \(CALs\)\. They also include two Microsoft Remote Desktop Services licenses for administrative purposes\.

For SQL Server license\-included AMIs, use the installation and setup media included in `C:\SQLServerSetup` to make changes to the default installation, add new features, or install additional named instances\.

## BYOL<a name="sql-server-on-ec2-licensing-options-byol"></a>

When you launch a SQL Server instance from an imported AMI, you can bring your existing licenses with the Bring Your Own License model \(BYOL\), and let AWS manage them to ensure compliance with licensing rules that you set\. After you import your licensed image, and it is available as a private AMI in your AWS account on the Amazon EC2 console, you can use the AWS License Manager service to create a license configuration\.

After you create the license configuration, you must associate the AMI that contains your licensed operating system image with the configuration\. Then, you must create a host resource group and associate it with the license configuration\. After you associate your host resource group with the configuration, License Manager automatically manages your hosts when you launch instances into a host resource group, and ensures that you do not exceed your configured license count limits\. For more information, see the [Getting started](https://docs.aws.amazon.com/license-manager/latest/userguide/getting-started.html) section of the *License Manager User Guide*\.

You can also bring your own SQL Server licenses with Active Software Assurance to default \(shared\) tenant Amazon EC2 through Microsoft License Mobility through Software Assurance\. For information about how to sign up for Microsoft License Mobility, see [License Mobility](http://aws.amazon.com/windows/resources/licensemobility/)\.