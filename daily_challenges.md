# Create Key Pair in AWS 
1. command : aws ec2 create-key-pair  --key-name datacenter-kp   --key-type rsa  --region us-east-1   --query 'KeyMaterial'   --output text > datacenter-kp.pem
2. To check all pairs in the region: aws ec2 describe-key-pairs --region us-east-1 --query 'KeyPairs[*].KeyName'  --output table
3. To check the sepcific key: aws ec2 describe-key-pairs   --key-names datacenter-kp  --region us-east-1

# create a security group and add the inbound rule
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

#
