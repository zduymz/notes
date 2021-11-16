# VPC Endpoint
A VPC endpoint is a virtual scalable networking component you create in a VPC and use as a private entry point to supported AWS services and third-party applications  
two types of VPC endpoints can be used to connect to Amazon S3:
* interface VPC endpoint
* gateway VPC endpoint

## Interface VPC endpoint
When configure an interface VPC endpoint, ENI with a private IP address is deployed in your subnet. An Amazon EC2 instance in the VPC can communicate with an Amazon S3 bucket through the ENI and AWS network.  
Using the interface endpoint, applications in your on-premises data center can easily query S3 buckets over `AWS Direct Connect` or `Site-to-Site VPN`.  

## Gateway VPC endpoints
use `prefix lists as the IP route target in a VPC route table`. This routes traffic privately to Amazon S3 or Amazon DynamoDB.  
An EC2 instance in a VPC without internet access can still directly read from and/or write to an Amazon S3 bucket.  
  
Check gateway endpoints were enabled

```
$ aws ec2 describe-prefix-lists
{
    "PrefixLists": [
        {
            "Cidrs": [
                "3.5.76.0/22",
                "3.5.80.0/21",
                "52.92.128.0/17",
                "52.218.128.0/17"
            ],
            "PrefixListId": "pl-12345678",
            "PrefixListName": "com.amazonaws.us-west-2.s3"
        }
    ]
}
```
## Selecting gateway or interface VPC endpoints
* **Cost**
    * Gateway endpoints for S3 are offered at `no cost` and the routes are managed through route tables
    * Interface endpoints are priced at $0.01/per AZ/per hour. Cost depends on the Region. Data transferred through the interface endpoint is charged at $0.01/per GB (depending on Region).
* **Access pattern** S3 access through gateway endpoints is supported only for resources in a specific VPC to which the endpoint is associated. S3 gateway endpoints do not currently support access from resources in a different Region, different VPC, or from an on-premises (non-AWS) environment. However, if you’re willing to manage a complex custom architecture, you can use proxies. In all those scenarios, where access is from resources external to VPC, S3 interface endpoints access S3 in a secure way
* **VPC endpoint architecture** Some customers use `centralized VPC endpoint architecture` patterns. This is where the interface endpoints are all managed in a central hub VPC for accessing the service from multiple spoke VPCs. This architecture helps reduce the complexity and maintenance for multiple interface VPC endpoints across different VPCs. When using an S3 interface endpoint, you must consider the amount of network traffic that would flow through your network from spoke VPCs to hub VPC. If the network connectivity between spoke and hub VPCs are set up using transit gateway, or VPC peering, consider the data processing charges (currently $0.02/GB). If VPC peering is used, there is no charge for data transferred between VPCs in the same Availability Zone. However, data transferred between Availability Zones or between Regions will incur charges.
![figure1](https://d2908q01vomqb2.cloudfront.net/fc074d501302eb2b93e2554793fcaf50b3bf7291/2021/07/20/Fig1-VPC-endpoint.png)

* **Bandwidth considerations**:
    * When setting up an interface endpoint, choose multiple subnets across multiple Availability Zones to implement high availability. The number of ENIs should equal to number of subnets chosen. Interface endpoints offer a throughput of 10 Gbps per ENI with a burst capability of 40 Gbps. If your use case requires higher throughput, contact AWS Support.
    * Gateway endpoints are route table entries that route your traffic directly from the subnet where traffic is originating to the S3 service. Traffic does not flow through an intermediate device or instance. Hence, there is no throughput limit for the gateway endpoint itself.
