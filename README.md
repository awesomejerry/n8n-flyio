# n8n on Fly.io

This repository contains the configuration for deploying n8n on Fly.io.

## Prerequisites

- [Fly.io CLI](https://fly.io/docs/hands-on/install-flyctl/) installed
- Fly.io account and authentication (`flyctl auth login`)

## Deployment Commands

### Initial Deployment
```bash
# Deploy the application for the first time
fly deploy

# Deploy without high availability (single instance)
fly deploy --ha=false
```

### Scaling Commands
```bash
# Scale to specific number of instances
fly scale count 2

# Scale to zero instances (stop all machines)
fly scale count 0

# Scale with specific VM size and memory
fly scale vm shared-cpu-1x --memory 1024

# Scale with different VM sizes
fly scale vm shared-cpu-2x    # 2 vCPUs, 1GB RAM
fly scale vm performance-1x   # 1 dedicated vCPU, 2GB RAM
fly scale vm performance-2x   # 2 dedicated vCPUs, 4GB RAM
```

### Monitoring and Management
```bash
# Check application status
fly status

# View application logs
fly logs

# SSH into the running machine
fly ssh console

# Open the application in browser
fly open

# Check machine information
fly machine list

# Restart all machines
fly machine restart
```

### Configuration Management
```bash
# Validate fly.toml configuration
fly config validate

# Show current configuration
fly config show

# Set environment variables
fly secrets set N8N_ENCRYPTION_KEY=your-encryption-key

# List all secrets
fly secrets list

# Remove a secret
fly secrets unset SECRET_NAME
```

### Volume Management
```bash
# List volumes
fly volumes list

# Create a new volume
fly volumes create n8n_data --size 1 --region sin

# Extend volume size
fly volumes extend <volume-id> --size 2

# Take volume snapshot
fly volumes snapshots create <volume-id>
```

### Deployment Options
```bash
# Deploy with specific strategy
fly deploy --strategy immediate     # Deploy immediately
fly deploy --strategy rolling       # Rolling deployment (default)
fly deploy --strategy canary        # Canary deployment

# Deploy with build arguments
fly deploy --build-arg NODE_ENV=production

# Deploy from specific Dockerfile
fly deploy --dockerfile Dockerfile.prod

# Deploy and wait for health checks
fly deploy --wait-timeout 300
```

### Useful Information Commands
```bash
# Show app information
fly info

# Show machine details
fly machine list --json

# Show resource usage
fly machine status

# Show app history
fly releases

# Show app regions
fly regions list
```

## Current Configuration

- **App Name**: n8n-flyio-awesomejerry
- **Region**: Singapore (sin)
- **Image**: n8nio/n8n:latest
- **Port**: 5678
- **Volume**: n8n_data mounted at `/home/node/.n8n`
- **VM Size**: shared-cpu-1x with 512MB memory
- **Auto-scaling**: Enabled (min 0 machines)

## Common Workflows

### Development Deployment
```bash
# Deploy without HA for cost savings
fly deploy --ha=false

# Scale down when not in use
fly scale count 0
```

### Production Deployment
```bash
# Deploy with high availability
fly deploy

# Scale to multiple instances for redundancy
fly scale count 2
```

### Troubleshooting
```bash
# Check logs for issues
fly logs --follow

# SSH into machine for debugging
fly ssh console

# Restart if needed
fly machine restart
```

## Notes

- The application uses auto-start/stop machines to save costs
- Data is persisted in the `n8n_data` volume
- HTTPS is enforced by default
- Webhook URL is configured for the Fly.io domain
