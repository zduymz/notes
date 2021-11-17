# IAM policy
IAM policies specify what actions are allowed or denied on what AWS resources . You attach IAM policies to IAM users, groups, or roles, which are then subject to the permissions you’ve defined. In other words, *IAM policies define what a principal can do what in your AWS environment*.

```json
{
  "Version": "2012-10-17",
  "Statement":[{
    "Effect": "Allow",
    "Action": "s3:*",
    "Resource": ["arn:aws:s3:::my_bucket",
                 "arn:aws:s3:::my_bucket/*"]
    }
  ]
}
```

# S3 Bucket policy
S3 bucket policies, on the other hand, are attached only to S3 buckets. S3 bucket policies specify what actions are allowed or denied for which principals on the bucket that the bucket policy is attached to (e.g. allow user Alice to PUT but not DELETE objects in the bucket). S3 bucket policies are a type of access control list, or ACL.

* Note: You attach S3 bucket policies at the bucket level (i.e. you can’t attach a bucket policy to an S3 object), but the permissions specified in the bucket policy apply to all the objects in the bucket.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": ["arn:aws:iam::111122223333:user/Alice",
                "arn:aws:iam::111122223333:root"]
      },
      "Action": "s3:*",
      "Resource": ["arn:aws:s3:::my_bucket",
                   "arn:aws:s3:::my_bucket/*"]
    }
  ]
}
```

# What about S3 ACLs?
As a general rule, AWS recommends using S3 bucket policies or IAM policies for access control. `S3 ACLs` is a legacy access control mechanism that predates IAM. However, if you already use S3 ACLs and you find them sufficient, there is no need to change.
  
An S3 ACL is a sub-resource that’s attached to every S3 bucket and object. It defines which AWS accounts or groups are granted access and the type of access. When you create a bucket or an object, Amazon S3 creates a default ACL that grants the resource owner full control over the resource.
  
You can attach S3 ACLs to individual objects within a bucket to manage permissions for those objects. S3 bucket policies and IAM policies define object-level permissions by providing those objects in the Resource element in your policy statements. The statement will apply to those objects in the bucket. Consolidating object-specific permissions into one policy (as opposed to multiple S3 ACLs) makes it simpler for you to determine effective permissions for your users and roles.



# When to use IAM policies vs. S3 policies
Use IAM policies if:

* You need to control access to AWS services other than S3. IAM policies will be easier to manage since you can centrally manage all of your permissions in IAM, instead of spreading them between IAM and S3.
*  You have numerous S3 buckets each with different permissions requirements. IAM policies will be easier to manage since you don’t have to define a large number of S3 bucket policies and can instead rely on fewer, more detailed IAM policies.
* You prefer to keep access control policies in the IAM environment.
  
Use S3 bucket policies if:

* You want a simple way to grant cross-account access to your S3 environment, without using IAM roles.
* Your IAM policies bump up against the size limit (up to 2 kb for users, 5 kb for groups, and 10 kb for roles). S3 supports bucket policies of up 20 kb.
* You prefer to keep access control policies in the S3 environment.
  
If you’re still unsure of which to use, consider which audit question is most important to you:

* If you’re more interested in `What can this user do in AWS?` then IAM policies are probably the way to go. You can easily answer this by looking up an IAM user and then examining their IAM policies to see what rights they have.
* If you’re more interested in `Who can access this S3 bucket?` then S3 bucket policies will likely suit you better. You can easily answer this by looking up a bucket and examining the bucket policy.

# How does authorization work with multiple access control mechanisms?
Whenever an AWS principal issues a request to S3, the authorization decision depends on the `union of all the IAM policies, S3 bucket policies, and S3 ACLs that apply`.
  
In accordance with the principle of `least-privilege`, decisions default to DENY and an explicit DENY always trumps an ALLOW. For example, if an IAM policy grants access to an object, the S3 bucket policies denies access to that object, and there is no S3 ACL, then access will be denied. Similarly, if no method specifies an ALLOW, then the request will be denied by default. Only if no method specifies a DENY and one or more methods specify an ALLOW will the request be allowed.
  
![policy-evaluation-logic](https://nodramadevops.com/wp-content/uploads/2019/11/AWS-PolicyEvaluationHorizontal.png)
  
[Original post](https://aws.amazon.com/blogs/security/iam-policies-and-bucket-policies-and-acls-oh-my-controlling-access-to-s3-resources/)