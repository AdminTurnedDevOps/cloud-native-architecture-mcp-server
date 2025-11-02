# Cloud Native Architecture MCP Server

An MCP (Model Context Protocol) server that provides tools to generate architecture diagrams for cloud-native infrastructure:

- **Kubernetes** cluster diagrams
- **AWS** infrastructure diagrams
- **GCP** infrastructure diagrams

Built using the [Diagrams](https://diagrams.mingrammer.com/) library to create professional, visual architecture diagrams programmatically.

## Features

- **Three specialized tools** for different cloud platforms
- **Visual diagram generation** with proper cloud provider icons
- **Cluster/VPC grouping** support for organizing components
- **Connection mapping** between components
- **Returns diagrams as images** directly in MCP responses

## Installation

### From PyPI (Recommended)

```bash
pip install cloud-native-architecture-mcp
```

Or use with `uvx` for on-demand execution:

```bash
uvx cloud-native-architecture-mcp
```

### From Source

```bash
git clone https://github.com/yourusername/cloud-native-architecture-mcp-server
cd cloud-native-architecture-mcp-server
pip install -e .
```

## Prerequisites

This package requires **Graphviz** to be installed on your system:

**macOS:**
```bash
brew install graphviz
```

**Ubuntu/Debian:**
```bash
sudo apt-get install graphviz
```

**Windows:**
Download from [graphviz.org](https://graphviz.org/download/)

## Usage with MCP Clients

### Claude Desktop

Add to your `claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "cloud-architecture": {
      "command": "uvx",
      "args": ["cloud-native-architecture-mcp"]
    }
  }
}
```

### AgentGateway

Add to your AgentGateway configuration:

```yaml
mcp_servers:
  - name: cloud-architecture
    stdio:
      cmd: uvx
      args: ["cloud-native-architecture-mcp"]
```

## Available Tools

### 1. build-kubernetes-diagram

Build Kubernetes architecture diagrams with support for:
- Deployments, StatefulSets, DaemonSets, Jobs, Pods
- Services, Ingress
- PVCs, PVs, StorageClass
- ConfigMaps, Secrets
- HPA (Horizontal Pod Autoscaler)
- Namespace clustering

**Example Input:**

```json
{
  "name": "microservices-app",
  "components": [
    {"type": "deployment", "name": "api-server", "replicas": 3},
    {"type": "service", "name": "api-svc"},
    {"type": "ingress", "name": "main-ingress"},
    {"type": "deployment", "name": "worker", "replicas": 2},
    {"type": "pvc", "name": "shared-storage"}
  ],
  "clusters": [
    {
      "name": "Production Namespace",
      "components": ["api-server", "api-svc", "worker"]
    }
  ],
  "connections": [
    {"from": "main-ingress", "to": "api-svc", "label": "HTTPS"},
    {"from": "api-svc", "to": "api-server"},
    {"from": "api-server", "to": "shared-storage"}
  ]
}
```

### 2. build-aws-diagram

Build AWS infrastructure diagrams with support for:
- Compute: EC2, ECS, EKS, Lambda
- Database: RDS, DynamoDB, ElastiCache, Redshift
- Storage: S3, EBS, EFS
- Network: ALB, NLB, ELB, CloudFront, Route53, VPC
- Integration: SQS, SNS, EventBridge
- VPC grouping

**Example Input:**

```json
{
  "name": "webapp-infrastructure",
  "components": [
    {"type": "route53", "name": "dns"},
    {"type": "alb", "name": "load-balancer"},
    {"type": "ec2", "name": "web-server"},
    {"type": "rds", "name": "postgres-db"},
    {"type": "s3", "name": "assets-bucket"},
    {"type": "elasticache", "name": "redis-cache"}
  ],
  "vpcs": [
    {
      "name": "Production VPC",
      "components": ["web-server", "postgres-db", "redis-cache", "load-balancer"]
    }
  ],
  "connections": [
    {"from": "dns", "to": "load-balancer"},
    {"from": "load-balancer", "to": "web-server"},
    {"from": "web-server", "to": "postgres-db"},
    {"from": "web-server", "to": "redis-cache"},
    {"from": "web-server", "to": "assets-bucket"}
  ]
}
```

### 3. build-gcp-diagram

Build GCP infrastructure diagrams with support for:
- Compute: GCE, GKE, Cloud Functions
- Database: Cloud SQL, Firestore, BigTable, Spanner
- Storage: GCS, Persistent Disk
- Network: Load Balancing, Cloud DNS, VPC
- Analytics: BigQuery, Dataflow, Pub/Sub
- VPC/Network grouping

**Example Input:**

```json
{
  "name": "data-processing-pipeline",
  "components": [
    {"type": "gcs", "name": "input-bucket"},
    {"type": "functions", "name": "process-files"},
    {"type": "pubsub", "name": "events"},
    {"type": "dataflow", "name": "etl-pipeline"},
    {"type": "bigquery", "name": "data-warehouse"}
  ],
  "connections": [
    {"from": "input-bucket", "to": "process-files", "label": "trigger"},
    {"from": "process-files", "to": "events"},
    {"from": "events", "to": "etl-pipeline"},
    {"from": "etl-pipeline", "to": "data-warehouse"}
  ]
}
```

## Development

### Setup Development Environment

```bash
# Clone the repository
git clone https://github.com/yourusername/cloud-native-architecture-mcp-server
cd cloud-native-architecture-mcp-server

# Create virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install in development mode with dev dependencies
pip install -e ".[dev]"
```

### Testing Locally

You can test the MCP server directly:

```bash
# Run the server
python -m cloud_native_architecture_mcp.server
```

Or use with an MCP client like the MCP Inspector:

```bash
npx @modelcontextprotocol/inspector uvx cloud-native-architecture-mcp
```

## Publishing to PyPI

### Prerequisites

1. Create accounts on:
   - [PyPI](https://pypi.org) (production)
   - [TestPyPI](https://test.pypi.org) (testing)

2. Install build tools:
```bash
pip install build twine
```

### Build and Publish

1. **Build the package:**
```bash
python -m build
```

2. **Test on TestPyPI first:**
```bash
twine upload --repository testpypi dist/*
```

3. **Install from TestPyPI to verify:**
```bash
pip install --index-url https://test.pypi.org/simple/ cloud-native-architecture-mcp
```

4. **Publish to PyPI:**
```bash
twine upload dist/*
```

5. **Verify installation:**
```bash
pip install cloud-native-architecture-mcp
```

## Architecture

```
cloud-native-architecture-mcp-server/
├── src/
│   └── cloud_native_architecture_mcp/
│       ├── __init__.py
│       └── server.py          # Main MCP server implementation
├── pyproject.toml             # Package configuration
├── README.md
└── LICENSE
```

## How It Works

1. **MCP Client** (Claude Desktop, AgentGateway, etc.) calls one of the three tools
2. **MCP Server** receives the component configuration (JSON)
3. **Diagrams Library** generates the architecture diagram using Graphviz
4. **Server returns** the diagram as a base64-encoded PNG image
5. **Client displays** the visual diagram to the user

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

MIT License - see [LICENSE](LICENSE) file for details

## Education

### Resources

- [MCP Documentation](https://modelcontextprotocol.io/)
- [Diagrams Documentation](https://diagrams.mingrammer.com/)
- [PyPI Publishing Guide](https://packaging.python.org/tutorials/packaging-projects/)

## Support

For issues, questions, or contributions, please visit the [GitHub repository](https://github.com/yourusername/cloud-native-architecture-mcp-server/issues).

### Why So Much JSON?
MCP communicates via the JSON-RPC protocol. All tools within an MCP Server, parameters, and responses are transmitted as JSON. If the JSON didn't exist, the MCP client wouldn't know what parameters to send and what the tool actually does. JSON-RPC is the MCP communication protocol