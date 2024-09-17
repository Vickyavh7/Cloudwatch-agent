# Cloudwatch-agent
Step 1: Update the System
(while launching instance make sure to attache IAM role to  instance give access to "Ec2roleforSSM" and "cloudwatchserviceagentpolicy")
First, ensure that your instance is up to date.

sudo yum update -y
Step 2: Download the CloudWatch Agent
Download the CloudWatch Agent package for Amazon Linux 2 from the AWS S3 repository.


sudo yum install amazon-cloudwatch-agent -y

Step 3: Create a CloudWatch Agent Configuration File
You can either create a CloudWatch Agent configuration file manually or use the wizard to generate one.

Option 1: Create the configuration file manually
The CloudWatch Agent configuration file is in JSON format. Here is an example file for basic metrics collection:

Create a configuration file at /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-config-wizard
# sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-config-wizard 

sudo vim /opt/aws/amazon-cloudwatch-agent/etc/amazon-cloudwatch-agent.json
Example configuration:


{
  "agent": {
    "metrics_collection_interval": 60,
    "run_as_user": "root"
  },
  "metrics": {
    "namespace": "CWAgent",
    "append_dimensions": {
      "InstanceId": "${aws:InstanceId}"
    },
    "metrics_collected": {
      "mem": {
        "measurement": [
          "mem_used_percent"
        ],
        "metrics_collection_interval": 60
      },
      "cpu": {
        "measurement": [
          "cpu_usage_idle",
          "cpu_usage_user",
          "cpu_usage_system"
        ],
        "metrics_collection_interval": 60
      }
    }
  }
}
Option 2: Use the CloudWatch Agent Wizard
The wizard helps you generate a configuration file interactively.

Run the configuration wizard:


sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-config-wizard
Follow the prompts to create a configuration that meets your needs.

Step 4: Start the CloudWatch Agent
Once the configuration file is in place, start the CloudWatch Agent.


Check if the CloudWatch Agent is running:


sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -m ec2 -a status

Step 6: View Metrics in CloudWatch
Open the CloudWatch Console in the AWS Management Console.
Under Metrics, check for a custom namespace (such as CWAgent if you used the example configuration).
The collected metrics (such as CPU and memory) should appear there.
