## Create Key Pair in AWS 
1. command : aws ec2 create-key-pair  --key-name datacenter-kp   --key-type rsa  --region us-east-1   --query 'KeyMaterial'   --output text > datacenter-kp.pem
2. To check all pairs in the region: aws ec2 describe-key-pairs --region us-east-1 --query 'KeyPairs[*].KeyName'  --output table
3. To check the sepcific key: aws ec2 describe-key-pairs   --key-names datacenter-kp  --region us-east-1

## Create a security group and add the inbound rule
1. Verify the VPC ID: aws ec2 describe-vpcs \
  --filters "Name=isDefault,Values=true" \
  --query "Vpcs[0].VpcId" \
  --output text \
  --region us-east-1
2. Create the security group with the VPC ID: aws ec2 create-security-group \
  --group-name devops-sg \
  --description "Security group for Nautilus App Servers" \
  --vpc-id <VPC_ID> \
  --region us-east-1
3. Add HTTP Inbound rule (port 80): aws ec2 authorize-security-group-ingress \
  --group-name devops-sg \
  --protocol tcp \
  --port 80 \
  --cidr 0.0.0.0/0 \
  --region us-east-1
4. Add SSH Inbound Rule (port 22): aws ec2 authorize-security-group-ingress \
  --group-name devops-sg \
  --protocol tcp \
  --port 22 \
  --cidr 0.0.0.0/0 \
  --region us-east-1
5. Verify the security group creation: aws ec2 describe-security-groups \
  --group-names devops-sg \
  --region us-east-1

## Create a subnet
1. Find the default VPC ID in the region: aws ec2 describe-vpcs \
  --filters "Name=isDefault,Values=true" \
  --query "Vpcs[0].VpcId" \
  --output text \
  --region us-east-1
2. Create a subnet with a CIDR block to not overlap with the others: aws ec2 create-subnet \
  --vpc-id <VPC_ID> \
  --cidr-block 172.31.100.0/24 \
  --availability-zone us-east-1a \
  --region us-east-1
3. naming the subnet using the subnet-id: aws ec2 create-tags \
  --resources <SUBNET_ID> \
  --tags Key=Name,Value=xfusion-subnet \
  --region us-east-1
4. Verifying the subnet: aws ec2 describe-subnets \
  --filters "Name=tag:Name,Values=xfusion-subnet" \
  --region us-east-1

## S3 Versioning
1. S3 Versioning is used to preserve, retrieve, and restore every version of an object in a bucket. It protects against accidental deletion, unintended overwrites, and provides a simple recovery mechanism for data changes.
2. Enable S3 Versioning -> aws s3api put-bucket-versioning \
  --bucket nautilus-s3-11491 \
  --versioning-configuration Status=Enabled \
  --region us-east-1
3. Verify -> aws s3api get-bucket-versioning \
  --bucket nautilus-s3-11491 \
  --region us-east-1

## Creating EBS Volume
1. Creating the EBS Volume -> aws ec2 create-volume \
  --size 2 \ (The size is in GB)
  --volume-type gp3 \
  --availability-zone us-east-1a \
  --region us-east-1
2. Add tag name as required -> aws ec2 create-tags \
  --resources <VOLUME_ID> \
  --tags Key=Name,Value=nautilus-volume \
  --region us-east-1
3. Verify using Query -> aws ec2 describe-volumes \
  --filters "Name=tag:Name,Values=nautilus-volume" \
  --query "Volumes[*].[VolumeId,Size,VolumeType,AvailabilityZone]" \
  --output table \
  --region us-east-1
