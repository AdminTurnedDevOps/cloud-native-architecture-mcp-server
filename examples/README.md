# Example Architecture Diagrams

This directory contains example JSON configurations for each of the three tools provided by the Cloud Native Architecture MCP Server.

## Usage

When using this MCP server with an MCP client (like Claude Desktop or AgentGateway), you can reference these examples to build architecture diagrams.

### Kubernetes Example

[kubernetes_example.json](./kubernetes_example.json) - A microservices platform with:
- API Gateway with autoscaling
- Multiple microservices (Auth, User, Order)
- MongoDB StatefulSet with persistent storage
- Ingress controller
- ConfigMaps and Secrets

### AWS Example

[aws_example.json](./aws_example.json) - A scalable web application with:
- Route53 DNS and CloudFront CDN
- Application Load Balancer
- EC2 web servers in a VPC
- RDS PostgreSQL with read replica
- ElastiCache Redis cluster
- Lambda functions with SQS
- S3 for static assets and uploads

### GCP Example

[gcp_example.json](./gcp_example.json) - A data processing platform with:
- Cloud DNS and Global Load Balancing
- Compute Engine API servers
- Cloud Storage for data lakes
- Cloud Functions for event processing
- Pub/Sub messaging
- Dataflow for ETL pipelines
- BigQuery for analytics
- Cloud SQL and Firestore databases

## Testing Locally

You can use these examples to test the MCP server:

1. Install the server:
```bash
pip install -e .
```

2. Run the server and use an MCP client to call the tools with these JSON files as input.

Alternatively, use the MCP Inspector:
```bash
npx @modelcontextprotocol/inspector python -m cloud_native_architecture_mcp.server
```

Then paste the JSON content from these examples into the tool call parameters.
