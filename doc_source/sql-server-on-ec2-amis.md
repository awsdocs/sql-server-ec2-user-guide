# Find a SQL Server license\-included AMI<a name="sql-server-on-ec2-amis"></a>

This topic describes how you can find Microsoft SQL Server license\-included AMIs that you own or are provided by AWS using the Amazon Elastic Compute Cloud \(Amazon EC2\) console, the AWS Tools for PowerShell, or the AWS CLI\. You can also search the AWS Marketplace for SQL Server license\-included AMIs provided by AWS\. As you select a SQL Server license\-included AMI, consider the following requirements you might have for the instances that you'll launch:
+ The AWS Region
+ The operating system
+ The architecture: 64\-bit \(`x86_64`\)
+ The [root device](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/RootDeviceStorage.html) type: Amazon EBS\-backed \(`EBS`\)
+ The provider \(for example, Amazon Web Services\)
+ Additional software \(for example, SQL Server\)

## SQL Server license\-included AMI discovery options<a name="finding-an-ami"></a>

------
#### [ AWS Marketplace ]

To view a list of SQL Server AMIs available from AWS in AWS Marketplace, see [Windows AMIs](http://aws.amazon.com/marketplace/search/results?searchTerms=sql+server)\.

------
#### [ Console ]

You can find SQL Server license\-included AMIs using the Amazon EC2 console\. You can select from the list of AMIs when you use the launch instance wizard to launch an instance, or you can search through all available AMIs using the **Images** page\. AMI IDs are unique to each AWS Region\.

**To find a SQL Server license\-included AMI using the launch instance wizard**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. From the navigation bar, select the Region in which to launch your instances\. You can select any Region that's available to you, regardless of your location\.

1. From the console dashboard, choose **Launch instances**\.

1. Under **Application and OS Images \(Amazon Machine Image\)**, enter `SQL` in the search bar and choose **Enter**\. You will be taken to the **AMIs** page, where you can browse and choose from AMIs with SQL Server included\. You can choose from AMIs under the **Quickstart AMIs**, **My AMIs**, **AWS Marketplace AMIs**, and the **Community AMIs** tabs\. You can filter by cost, operating system, and architecture\.

1. To launch an instance from this AMI, select it and then choose **Launch instance**\. For more information about launching an instance using the console, see [Launch an instance using the new launch instance wizard](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/ec2-launch-instance-wizard.html)\. If you're not ready to launch the instance now, take note of the AMI ID for later\.

**To find a SQL Server AMI using the AMIs page**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. From the navigation bar, select the Region in which to launch your instances\. You can select any Region that's available to you, regardless of your location\.

1. In the navigation pane, choose **AMI Catalog**\.

1. Enter `SQL` in the search bar and choose **Enter**\. You can choose from SQL Server license\-included AMIs under the **Quickstart AMIs**, **My AMIs**, **AWS Marketplace AMIs**, and the **Community AMIs** tabs\. You can filter by cost, operating system, and architecture\.

1. To launch an instance from this AMI, select it and then choose **Launch instance **\. For more information about launching an instance using the console, see [Launching your instance from an AMI]()\. If you're not ready to launch the instance now, take note of the AMI ID for later\.

------
#### [ PowerShell ]

You can use cmdlets for Amazon EC2 to list only the Windows AMIs that matches your requirements\. After locating an AMI that matches your requirements, take note of its ID so that you can use it to launch instances\. For more information, see [Launch an Instance Using Windows PowerShell](https://docs.aws.amazon.com/powershell/latest/userguide/pstools-ec2-launch.html) in the *AWS Tools for Windows PowerShell User Guide*\.

You can use the `Get-EC2Image` cmdlet to list SQL Server license\-included AMIs\. The following commands filter for AMIs owned by you, or Amazon, with *SQL* in their name:

```
$name_values = New-Object 'collections.generic.list[string]'
$name_values.add("*SQL*")
$filter_name = New-Object Amazon.EC2.Model.Filter -Property @{Name = "name"; Values = $name_values}
Get-EC2Image -Owner amazon, self -Filter $filter_name
```

For more information and examples, see [Find an AMI Using Windows PowerShell](https://docs.aws.amazon.com/powershell/latest/userguide/pstools-ec2-get-amis.html) in the *AWS Tools for Windows PowerShell User Guide*\.

------
#### [ AWS CLI ]

You can use AWS CLI commands for Amazon EC2 to list only the SQL Server license\-included AMIs that match your requirements\. After locating an AMI that matches your requirements, take note of its ID so that you can use it to launch instances\. For more information, see [Launching an Instance Using the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-ec2-launch.html#launching-instances) in the *AWS Command Line Interface User Guide*\.

The [describe\-images](https://docs.aws.amazon.com/cli/latest/reference/ec2/describe-images.html) command supports filtering parameters\. For example, use the `--owners` parameter with `amazon` to display public AMIs owned by Amazon or `self` to list AMIs you own\. You can specify multiple values for the `--owners` parameter as in the following example:

```
aws ec2 describe-images --owners self amazon
```

You can add the following filter to the previous command to display only SQL Server license\-included AMIs:

```
--filters "Name=name,Values=*SQL*"
```

You can use the following filter with the command to display only AMIs backed by Amazon EBS:

```
--filters "Name=root-device-type,Values=ebs"
```

You can combine multiple filters together\. For example, this command will list all AMIs owned by you or Amazon with *SQL* in the AMI name and the `--root-device-type` parameter as `ebs`:

```
aws ec2 describe-images --owners self amazon --filters "Name=name,Values=*SQL*" "Name=root-device-type,Values=ebs"
```

**Note**  
Omitting the `--owners` flag from the describe\-images command will return all images for which you have launch permissions, regardless of ownership\.

------

## SQL Server license\-included AMI version history<a name="windows-ami-version-history"></a>

To view changes to each release of the AWS Windows AMIs, including SQL Server updates, see the [AWS Windows AMI version history](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/ec2-windows-ami-version-history.html) in the *Amazon EC2 User Guide*\.