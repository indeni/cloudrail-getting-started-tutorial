# Cloudrail - Getting Started Demo
This repository contains example terraform resources (all contained in main.tf) for running a cloudrail scan.

## Running A Scan
To run a static scan on this repository, simply run the following command

```bash
cloudrail run
```

## Cloudrail Installation
If you haven't yet installed Cloudrail, [you can do so here](https://cloudrail.app/start/).


## Example Terraform Architecture
In this test case we have two EC2 instances - one in a public subnet, one in a private subnet - using the same IAM role.

This is a dangerous situation. Most likely, the private instance is given certain elevated permissions in order to
handle confidential data. Unfortunately, the same permissions are given to a public EC2, which is more susceptible to hacking.

Cloudrail does a deep context analysis to understand where each EC2 is located, and then checks if they are using the same role.

The result:

```
Rule: EC2(s) within the public and private subnets should not share identical IAM roles
 - 1 Resources Exposed:
-----------------------------------------------
   - Exposed Resource: [aws_instance.priv_ins] (main.tf:107)
     Violating Resource: [aws_iam_role.test_role]  (main.tf:51)

     Evidence:
         Instance ['aws_instance.pub_ins']
             | Instance is publicly exposed
             | Instance uses IAM role aws_iam_role.test_role
             | Private EC2 instance shares IAM role aws_iam_role.test_role as well
         Instance aws_instance.priv_ins
```

## More Information
For more information and how to use cloudrail for specific situations, [check our our documentation](https://docs.cloudrail.app).
