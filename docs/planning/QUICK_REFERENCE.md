# ⚡ Quick Reference Guide - Essential Commands & Concepts

**One-page reference for daily development**

---

## 🔧 AWS CLI Commands

### S3 Operations
```bash
# Create bucket
aws s3 mb s3://my-bucket-name

# List buckets
aws s3 ls

# Upload file
aws s3 cp file.txt s3://my-bucket/

# Download file
aws s3 cp s3://my-bucket/file.txt ./

# Sync directory
aws s3 sync ./local-dir s3://my-bucket/remote-dir

# Delete object
aws s3 rm s3://my-bucket/file.txt

# Delete bucket (must be empty)
aws s3 rb s3://my-bucket-name
```

### Lambda Operations
```bash
# List functions
aws lambda list-functions

# Invoke function
aws lambda invoke \
  --function-name my-function \
  --payload '{"key":"value"}' \
  response.json

# Update function code
aws lambda update-function-code \
  --function-name my-function \
  --zip-file fileb://function.zip

# View logs
aws logs tail /aws/lambda/my-function --follow
```

### DynamoDB Operations
```bash
# List tables
aws dynamodb list-tables

# Get item
aws dynamodb get-item \
  --table-name MyTable \
  --key '{"id": {"S": "123"}}'

# Put item
aws dynamodb put-item \
  --table-name MyTable \
  --item '{"id":{"S":"123"},"name":{"S":"John"}}'

# Scan table
aws dynamodb scan --table-name MyTable
```

### IAM Operations
```bash
# List users
aws iam list-users

# Get current identity
aws sts get-caller-identity

# List roles
aws iam list-roles

# Attach policy to role
aws iam attach-role-policy \
  --role-name my-role \
  --policy-arn arn:aws:iam::aws:policy/AmazonS3ReadOnlyAccess
```

---

## 🐳 Docker Commands

### Basic Operations
```bash
# Build image
docker build -t my-app:latest .

# Run container
docker run -d -p 8080:8080 --name my-app my-app:latest

# Stop container
docker stop my-app

# Remove container
docker rm my-app

# Remove image
docker rmi my-app:latest

# List containers
docker ps -a

# List images
docker images

# View logs
docker logs -f my-app

# Execute command in container
docker exec -it my-app /bin/bash
```

### Docker Compose
```bash
# Start services
docker-compose up -d

# Stop services
docker-compose down

# View logs
docker-compose logs -f

# Restart service
docker-compose restart service-name

# Build and start
docker-compose up --build
```

---

## 🔄 Git Commands

### Daily Workflow
```bash
# Check status
git status

# Add files
git add .
git add specific-file.txt

# Commit
git commit -m "Descriptive message"

# Push
git push origin branch-name

# Pull
git pull origin main

# Create branch
git checkout -b new-feature

# Switch branch
git checkout existing-branch

# View diff
git diff
git diff --staged

# View history
git log --oneline -10

# Undo last commit (keep changes)
git reset --soft HEAD~1

# Discard local changes
git checkout -- filename
```

---

## 🏗️ Terraform Commands

### Basic Workflow
```bash
# Initialize
terraform init

# Format code
terraform fmt

# Validate
terraform validate

# Plan changes
terraform plan

# Apply changes
terraform apply
terraform apply -auto-approve

# Destroy resources
terraform destroy

# Show current state
terraform show

# List resources
terraform state list

# Import existing resource
terraform import aws_s3_bucket.example my-bucket
```

---

## ☸️ Kafka Commands

### Topic Management
```bash
# Create topic
kafka-topics --create \
  --topic my-topic \
  --bootstrap-server localhost:9092 \
  --partitions 3 \
  --replication-factor 1

# List topics
kafka-topics --list \
  --bootstrap-server localhost:9092

# Describe topic
kafka-topics --describe \
  --topic my-topic \
  --bootstrap-server localhost:9092

# Delete topic
kafka-topics --delete \
  --topic my-topic \
  --bootstrap-server localhost:9092
```

### Producer/Consumer
```bash
# Console producer
kafka-console-producer \
  --topic my-topic \
  --bootstrap-server localhost:9092

# Console consumer
kafka-console-consumer \
  --topic my-topic \
  --bootstrap-server localhost:9092 \
  --from-beginning

# Consumer groups
kafka-consumer-groups --list \
  --bootstrap-server localhost:9092
```

---

## 🐍 Python - Common Code Snippets

### AWS SDK (Boto3)
```python
import boto3

# S3
s3 = boto3.client('s3')
s3.upload_file('local.txt', 'bucket', 'remote.txt')
s3.download_file('bucket', 'remote.txt', 'local.txt')

# Lambda
lambda_client = boto3.client('lambda')
response = lambda_client.invoke(
    FunctionName='my-function',
    Payload='{"key": "value"}'
)

# DynamoDB
dynamodb = boto3.resource('dynamodb')
table = dynamodb.Table('MyTable')
table.put_item(Item={'id': '123', 'name': 'John'})
response = table.get_item(Key={'id': '123'})

# CloudWatch
cloudwatch = boto3.client('cloudwatch')
cloudwatch.put_metric_data(
    Namespace='MyApp',
    MetricData=[{
        'MetricName': 'RequestCount',
        'Value': 1,
        'Unit': 'Count'
    }]
)
```

### Kafka (Python)
```python
from kafka import KafkaProducer, KafkaConsumer
import json

# Producer
producer = KafkaProducer(
    bootstrap_servers=['localhost:9092'],
    value_serializer=lambda v: json.dumps(v).encode('utf-8')
)
producer.send('my-topic', {'key': 'value'})

# Consumer
consumer = KafkaConsumer(
    'my-topic',
    bootstrap_servers=['localhost:9092'],
    value_deserializer=lambda m: json.loads(m.decode('utf-8'))
)
for message in consumer:
    print(message.value)
```

### Qdrant (Vector DB)
```python
from qdrant_client import QdrantClient
from qdrant_client.models import Distance, VectorParams, PointStruct

client = QdrantClient("localhost", port=6333)

# Create collection
client.create_collection(
    collection_name="my_collection",
    vectors_config=VectorParams(size=768, distance=Distance.COSINE)
)

# Insert vectors
client.upsert(
    collection_name="my_collection",
    points=[
        PointStruct(id=1, vector=[0.1]*768, payload={"text": "example"})
    ]
)

# Search
results = client.search(
    collection_name="my_collection",
    query_vector=[0.1]*768,
    limit=5
)
```

---

## 📊 Monitoring Commands

### CloudWatch Logs
```bash
# Tail logs
aws logs tail /aws/lambda/my-function --follow

# Filter logs
aws logs filter-log-events \
  --log-group-name /aws/lambda/my-function \
  --filter-pattern "ERROR"

# Get log streams
aws logs describe-log-streams \
  --log-group-name /aws/lambda/my-function
```

### Cost Monitoring
```bash
# Get cost and usage
aws ce get-cost-and-usage \
  --time-period Start=2024-11-01,End=2024-11-30 \
  --granularity DAILY \
  --metrics BlendedCost

# Get cost by service
aws ce get-cost-and-usage \
  --time-period Start=2024-11-01,End=2024-11-30 \
  --granularity MONTHLY \
  --metrics BlendedCost \
  --group-by Type=DIMENSION,Key=SERVICE
```

---

## 🔐 Security Best Practices

### IAM Policy Example
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:GetObject",
        "s3:PutObject"
      ],
      "Resource": "arn:aws:s3:::my-bucket/*"
    }
  ]
}
```

### Lambda Execution Role
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "lambda.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

---

## 🚨 Troubleshooting Quick Fixes

### AWS CLI Not Working
```bash
# Reconfigure
aws configure

# Check credentials
aws sts get-caller-identity

# Check region
aws configure get region
```

### Docker Issues
```bash
# Clean up
docker system prune -a

# Restart Docker service
sudo systemctl restart docker

# Check logs
docker logs container-name
```

### Lambda Timeout
```python
# Increase timeout in Lambda console: Configuration > General > Timeout
# Or via CLI:
aws lambda update-function-configuration \
  --function-name my-function \
  --timeout 300
```

### Out of Memory
```python
# Increase memory in Lambda console: Configuration > General > Memory
# Or via CLI:
aws lambda update-function-configuration \
  --function-name my-function \
  --memory-size 512
```

---

## 📝 Environment Variables

### Local Development (.env)
```bash
AWS_REGION=us-east-1
AWS_ACCESS_KEY_ID=your-access-key
AWS_SECRET_ACCESS_KEY=your-secret-key

KAFKA_BOOTSTRAP_SERVERS=localhost:9092
QDRANT_HOST=localhost
QDRANT_PORT=6333

LLM_API_KEY=your-llm-api-key
MODEL_NAME=claude-3-sonnet-20240229

DYNAMODB_TABLE_NAME=tickets
S3_BUCKET_NAME=jira-tickets-bucket
```

### Loading in Python
```python
import os
from dotenv import load_dotenv

load_dotenv()

AWS_REGION = os.getenv('AWS_REGION')
KAFKA_SERVERS = os.getenv('KAFKA_BOOTSTRAP_SERVERS')
```

---

## 🔄 CI/CD Pipeline YAML

### GitHub Actions Basic Template
```yaml
name: Deploy

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      
      - name: Configure AWS
        uses: aws-actions/configure-aws-credentials@v1
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: us-east-1
      
      - name: Deploy Lambda
        run: |
          zip -r function.zip .
          aws lambda update-function-code \
            --function-name my-function \
            --zip-file fileb://function.zip
```

---

## 📊 Useful Queries & Filters

### CloudWatch Insights Query
```
fields @timestamp, @message
| filter @message like /ERROR/
| sort @timestamp desc
| limit 20
```

### DynamoDB Filter Expression
```python
from boto3.dynamodb.conditions import Key, Attr

table.scan(
    FilterExpression=Attr('status').eq('OPEN') & Attr('priority').eq('HIGH')
)
```

---

## 🎯 Performance Optimization

### Lambda Best Practices
```python
import boto3

# Initialize outside handler (connection pooling)
s3 = boto3.client('s3')

def lambda_handler(event, context):
    # Handler code here
    pass
```

### Batch Operations
```python
# Good: Batch write
with table.batch_writer() as batch:
    for item in items:
        batch.put_item(Item=item)

# Bad: Individual writes
for item in items:
    table.put_item(Item=item)  # Slow!
```

---

## 📞 Emergency Contacts & Links

### AWS Support
- Console: https://console.aws.amazon.com/support/
- Service Health: https://status.aws.amazon.com/

### Documentation Quick Links
- AWS Docs: https://docs.aws.amazon.com/
- Boto3 Docs: https://boto3.amazonaws.com/v1/documentation/api/latest/index.html
- Terraform AWS: https://registry.terraform.io/providers/hashicorp/aws/latest/docs

### Community
- Stack Overflow: https://stackoverflow.com/questions/tagged/amazon-web-services
- AWS re:Post: https://repost.aws/

---

## ⚡ Keyboard Shortcuts

### VS Code
- `Ctrl+P` - Quick file open
- `Ctrl+Shift+P` - Command palette
- `Ctrl+`` - Toggle terminal
- `Ctrl+/` - Toggle comment
- `Ctrl+D` - Select next occurrence

### AWS Console
- `Alt+S` - Search services
- `/` - Focus search

---

## 🎓 Key Concepts at a Glance

### AWS Well-Architected Pillars
1. **Operational Excellence** - Run and monitor systems
2. **Security** - Protect data and systems
3. **Reliability** - Recover from failures
4. **Performance Efficiency** - Use resources efficiently
5. **Cost Optimization** - Avoid unnecessary costs

### Lambda Pricing
- **Requests:** First 1M free, then $0.20 per 1M
- **Duration:** $0.00001667 per GB-second
- **Always Free:** 1M requests + 400,000 GB-seconds/month

### S3 Storage Classes
- **Standard** - Frequent access ($0.023/GB)
- **Infrequent Access** - Less frequent ($0.0125/GB)
- **Glacier** - Archive ($0.004/GB)
- **Intelligent-Tiering** - Auto-optimization

---

**Print this page and keep it handy during development! 📌**

**Last Updated:** 2024-11-13  
**Version:** 1.0
