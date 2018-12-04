# lambda-vpc-privatelink

AWS CloudFormation script that creates an internet facing API using AWS API Gateway and AWS Lambda, which 
demonstrates the use of VPC PrivateLink EndPoints, by showing how to capture Lambda logging in CloudWatch 
when the Lambda is executing inside a Custom private VPC (i.e. a VPC with no internet access).

Using default parameter settings, an example POST request is of the form:

`curl -d "This is a test" https://<REST API>.execute-api.eu-west-2.amazonaws.com/beta/publish`

The contents of the request's body (`This is a test`) will be captured in the CloudWatch Log Group for the Lambda function,
as part of the contents of the `event` that the Lambda function receives and publishes to its log.

The script demonstrates two concepts:

 * How a VPC PrivateLink Endpoint is configured using Cloudformation, including how the Endpoint's Security Group is configured to allow ingress from the Lambda Security Group.  This shows that AWS Services can be used without traversing the internet, thereby removing any requirement for Internet and NAT Gateways to improve overall security
 * How the API can invoke a Lambda Alias, which points to a specific Lambda Version.  This allows the Lambda function to be changed and tested without affecting the existing behaviour of the API

The script creates the following:

![alt text](https://github.com/gford1000-aws/lambda-vpc-privatelink/blob/master/Lambda%20Access%20to%20VPC%20PrivateLink.png "Script per designer")

## Arguments

| Argument                     | Description                                                                 |
| ---------------------------- |:---------------------------------------------------------------------------:|
| APIName                      | Name of the REST API                                                        |
| APIDescription               | Description of the REST API                                                 |
| StageName                    | Name of the stage to be created                                             |
| LoggingTTL                   | TTL for logs of each Lambda function, saved to specific log groups          |


## Outputs

| Output                  | Description                                                    |
| ----------------------- |:--------------------------------------------------------------:|
| URL                     | The URL of the deployed REST API                               |


## Notes

* Lambda Security Group has minimal permissions - only to make connections for HTTPS
* VPC PrivateLink Security Group only allows ingress from the Lambda Security Group
* VPC PrivateLink Policy is restricted to posting log events
* Since API Gateway is invoking the Alias of the Lambda function, it must be permissioned for the **alias** rather than the function
* The Lambda function runs within 2 private subnets, the minimum number of subnets that can be used with a VPC Privatelink Endpoint

## Licence

This project is released under the MIT license. See [LICENSE](LICENSE) for details.
