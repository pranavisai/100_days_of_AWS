# Create Key Pair in AWS 
1. command : aws ec2 create-key-pair  --key-name datacenter-kp   --key-type rsa  --region us-east-1   --query 'KeyMaterial'   --output text > datacenter-kp.pem
2. To check all pairs in the region: aws ec2 describe-key-pairs --region us-east-1 --query 'KeyPairs[*].KeyName'  --output table
3. To check the sepcific key: aws ec2 describe-key-pairs   --key-names datacenter-kp  --region us-east-1
