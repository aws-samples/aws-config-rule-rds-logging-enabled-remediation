# Using AWS Systems Manager Automation to remediate the rds-logging-enabled AWS Config rule

## Overview
The solution uses [AWS Config](https://aws.amazon.com/config/) rule [rds-logging-enabled](https://docs.aws.amazon.com/config/latest/developerguide/rds-logging-enabled.html) that checks if respective logs of Amazon Relational Database Service (Amazon RDS) are enabled. The rule is NON_COMPLIANT if any log types are not enabled. [AWS Systems Manager Automation](https://docs.aws.amazon.com/systems-manager/latest/userguide/documents.html) document used as remediation action to enable the logs export to cloudwatch for the RDS instances that are marked NON_COMPLIANT.

![Image Alt text](/images/arch-diagram.png)

## Deployment
### Prerequisites
To deploy this  solution, you must first enable [AWS Config](https://docs.aws.amazon.com/config/latest/developerguide/gs-console.html).

Once AWS Config is enabled, use the [AWS CloudFormation](https://aws.amazon.com/cloudformation/) template provided in this project, found here: [rds_logging_config_rule_with_remediate.yml](rds_logging_config_rule_with_remediate.yml). This CloudFormation stack will create the following resources:

* AWS Config rule to scan/remediate RDS instances
* Systems Manager Automation remediation document
* IAM Automation service role and policy used for remediation
* AWS Config remediation configuration

For more details about launching a stack, refer to [Creating a Stack on the AWS CloudFormation Console](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-console-create-stack.html).

### Template Parameters
The stack template includes the following parameters:

| Parameter | Required | Description |
| --- | --- | --- |
| DocumentName | Yes | Name of the SSM Automation document created. |
| RDSlogConfigRuleName | Yes | Name of the AWS Config rule created. |
| AssumeRolePolicyname | Yes | Name of the IAM policy created which will be attached to the IAM role. |

## Usage
Once deployed, navigate to the [AWS Config console](https://console.aws.amazon.com/config) and choose Rules to view the results of the created Config rules. Note that it can take some time for your resources to be recorded by AWS Config.

RDS instances which doesn't have engine specifc logs exported to cloudwatch are marked as **Noncompliant**. Automatic remediation action will be performed on the Noncompliant RDS instances.

You can review details about the remediation actions performed in the [Systems Manager automations console](https://console.aws.amazon.com/systems-manager/automation).

For more details, see [Remediating Noncompliant AWS Resources by AWS Config Rules](https://docs.aws.amazon.com/config/latest/developerguide/remediation.html).

## Limitations
* This Remediation doesnot support SQL Server Express Edition 

## Security

See [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) for more information.

## License

This library is licensed under the MIT-0 License. See the LICENSE file.
