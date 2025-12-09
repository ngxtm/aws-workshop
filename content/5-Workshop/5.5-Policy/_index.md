---
title : "Development & Deployment"
date: 2025-09-09
weight : 5
chapter : false
pre : " <b> 5.5. </b> "

---

#### Overview

This section covers the development workflow and deployment process for MapVibe applications.

#### Development Workflow

1. **Local Development**
   - Run frontend apps locally with `bun run dev`
   - Test API locally with `bun run dev` in `apps/api`
   - Use local database or connect to dev RDS

2. **Build Process**
   - Build all packages: `bun run build`
   - Build specific app: `cd apps/web && bun run build`

3. **Testing**
   - Type checking: `bun run type-check`
   - Linting: `bun run lint`

#### Deployment Process

##### Deploy Infrastructure

```bash
cd infrastructure/terraform
terraform apply
```

##### Deploy API

```bash
# Build API
cd apps/api
bun run build

# Deploy using script
bun run deploy
# Or use: pwsh ../../scripts/deploy-api.ps1
```

##### Deploy Frontend (Web)

```bash
# Build frontend
cd apps/web
bun run build

# Deploy to S3 + CloudFront
bun run deploy
```

##### Deploy Admin Dashboard

```bash
# Build admin
cd apps/admin
bun run build

# Deploy using script
pwsh ../../scripts/deploy-admin.ps1
```

##### Run Database Migrations

```bash
pwsh scripts/deploy-migrate.ps1
```

#### Deployment Scripts

The `scripts/` directory contains PowerShell scripts for deployment:
- `deploy-api.ps1` - Deploy API Lambda
- `deploy-admin.ps1` - Deploy admin dashboard
- `deploy-migrate.ps1` - Run database migrations
- `build-all-lambdas.ps1` - Build all Lambda functions

#### Best Practices

1. **Always test locally** before deploying
2. **Use environment-specific configurations**
3. **Monitor CloudWatch logs** after deployment
4. **Verify deployments** by testing endpoints
5. **Keep infrastructure in sync** with code changes

#### Connect to an EC2 instance and verify connectivity to S3

1. Start a new AWS Session Manager session on the instance named Test-Gateway-Endpoint. From the session, verify that you can list the contents of the bucket you created in Part 1: Access S3 from VPC:

```
aws s3 ls s3://\<your-bucket-name\>
```

![test](/images/5-Workshop/5.5-Policy/test1.png)

The bucket contents include the two 1 GB files uploaded in earlier.

2. Create a new S3 bucket; follow the naming pattern you used in Part 1, but add a '-2' to the name. Leave other fields as default and click create

![create bucket](/images/5-Workshop/5.5-Policy/create-bucket.png)

Successfully create bucket

![Success](/images/5-Workshop/5.5-Policy/create-bucket-success.png)

3. Navigate to: Services > VPC > Endpoints, then select the Gateway VPC endpoint you created earlier. Click the Policy tab. Click Edit policy.

![policy](/images/5-Workshop/5.5-Policy/policy1.png)

The default policy allows access to all S3 Buckets through the VPC endpoint.

4. In Edit Policy console, copy & Paste the following policy, then replace yourbucketname-2 with your 2nd bucket name. This policy will allow access through the VPC endpoint to your new bucket, but not any other bucket in Amazon S3. Click Save to apply the policy.

```
{
  "Id": "Policy1631305502445",
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "Stmt1631305501021",
      "Action": "s3:*",
      "Effect": "Allow",
      "Resource": [
      				"arn:aws:s3:::yourbucketname-2",
       				"arn:aws:s3:::yourbucketname-2/*"
       ],
      "Principal": "*"
    }
  ]
}
```

![custom policy](/images/5-Workshop/5.5-Policy/policy2.png)

Successfully customize policy

![success](/static/images/5-Workshop/5.5-Policy/success.png)

5. From your session on the Test-Gateway-Endpoint instance, test access to the S3 bucket you created in Part 1: Access S3 from VPC

```
aws s3 ls s3://<yourbucketname>
```

This command will return an error because access to this bucket is not permitted by your new VPC endpoint policy:

![error](/static/images/5-Workshop/5.5-Policy/error.png)

6. Return to your home directory on your EC2 instance `cd~`

- Create a file `fallocate -l 1G test-bucket2.xyz `
- Copy file to 2nd bucket `aws s3 cp test-bucket2.xyz s3://<your-2nd-bucket-name>`

![success](/static/images/5-Workshop/5.5-Policy/test2.png)

This operation succeeds because it is permitted by the VPC endpoint policy.

![success](/static/images/5-Workshop/5.5-Policy/test2-success.png)

- Then we test access to the first bucket by copy the file to 1st bucket `aws s3 cp test-bucket2.xyz s3://<your-1st-bucket-name>`

![fail](/static/images/5-Workshop/5.5-Policy/test2-fail.png)

This command will return an error because access to this bucket is not permitted by your new VPC endpoint policy.

#### Part 3 Summary:

In this section, you created a VPC endpoint policy for Amazon S3, and used the AWS CLI to test the policy. AWS CLI actions targeted to your original S3 bucket failed because you applied a policy that only allowed access to the second bucket you created. AWS CLI actions targeted for your second bucket succeeded because the policy allowed them. These policies can be useful in situations where you need to control access to resources through VPC endpoints.
