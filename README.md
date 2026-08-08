# Vapor CE

Vapor CE (Community Edition) is a comprehensive Linux OS management system, inspired by [Cockpit](https://github.com/cockpit-project/cockpit), designed to provide a modern web interface for server administration, container orchestration, virtualization management, and Kubernetes management.

## Features

- **System Monitoring**: Real-time CPU, memory, disk, and network monitoring
- **Service Management**: SystemD service control and monitoring
- **Virtualization**: Full KVM/QEMU virtual machine management via libvirt
- **Container Management**: Docker container and image management
- **Kubernetes Integration**: Cluster management and workload deployment
- **Storage Management**: LVM, disk, and storage pool management
- **Network Configuration**: Network interface and firewall management
- **User Management**: System user and group administration
- **Web Terminal**: Browser-based SSH terminal access
- **Log Viewer**: Real-time system log monitoring

## System Requirements

### Minimum Hardware

| Component | Requirement |
|-----------|-------------|
| Architecture | x86_64 (amd64) |
| CPU | 2 cores |
| RAM | 2 GB |
| Disk | 1 GB free space |
| Network | Active network connection |

### Supported Operating Systems

Vapor requires **libvirt 8.0.0+** and **QEMU 6.2+** for full virtualization features including UEFI and Secure Boot support.

| Distribution | Version | QEMU | libvirt | Status |
|--------------|---------|------|---------|--------|
| **Ubuntu** | 24.04 LTS | 8.2.2 | 10.0.0 | ✅ Recommended |
| | 22.04 LTS | 6.2.0 | 8.0.0 | ✅ Supported |
| | 20.04 LTS | 4.2.1 | 6.0.0 | ❌ Not supported (QEMU too old) |
| **Debian** | 13 (Trixie) | 10.0.8 | 12.0.0 | ✅ Supported |
| **Debian** | 12 (Bookworm) | 7.2.0 | 9.0.0 | ✅ Supported |
| | 11 (Bullseye) | 5.2.0 | 7.0.0 | ❌ Not supported (QEMU/libvirt too old) |
| **RHEL/Rocky/Alma** | 9.x | 7.0+ | 9.0.0+ | ✅ Supported |
| | 8.x | 4.2.0 | 8.6.0 | ❌ Not supported (QEMU too old) |
| **Fedora** | 40+ | 8.2+ | 10.0.0+ | ✅ Supported |
| | 39 | 8.1.0 | 9.6.0 | ✅ Supported |
| **CentOS Stream** | 9 | 7.0+ | 9.0.0+ | ✅ Supported |

> **Note**: QEMU 6.2+ is required for proper UEFI display output. Older QEMU versions have display initialization issues with UEFI VMs.

## Installation

### Quick Installation (Recommended)

The interactive installer uses Ansible to set up Vapor and optional components:

```bash
# Download the install script
curl -fsSL https://raw.githubusercontent.com/awanio/vapor-ce/main/scripts/install.sh -o install.sh

# Make it executable
chmod +x install.sh

# Run the installer (requires sudo)
# Latest default version (from the role):
sudo ./install.sh

# Or install a specific GitHub release tag, e.g. v0.1.2:
sudo ./install.sh -v v0.1.4
```

The installer will prompt you to select which components to install:
- **Libvirt/KVM**: Virtual machine management
- **Container Runtime**: Docker or Containerd
- **Kubernetes**: With version selection (v1.29 - v1.34)
- **Helm**: Kubernetes package manager

### Non-Interactive Installation

For automated deployments, use environment variables:

```bash
# Set installation options
export AUTO_INSTALL_DEPS=y
export INSTALL_LIBVIRT=y
export INSTALL_DOCKER=y
export INSTALL_K8S=y
export K8S_VERSION=1.30

# Optionally pin a specific Vapor release tag (e.g. v0.1.1)
# If omitted, the default vapor_version from the Ansible role is used.
# Note: when piping via bash, pass CLI args with -s --

# Run installer (latest default)
curl -fsSL https://raw.githubusercontent.com/awanio/vapor-ce/main/scripts/install.sh | sudo -E bash

# Run installer for a specific version
curl -fsSL https://raw.githubusercontent.com/awanio/vapor-ce/main/scripts/install.sh | sudo -E bash -s -- -v v0.1.1
```

### Building from Source

```bash
# Clone the repository
git clone https://github.com/awanio/vapor-ce.git
cd vapor

# Build (requires Go 1.21+ and Node.js 18+)
make build

# Install system-wide
sudo make install

# Or run directly
./bin/vapor
```

## Configuration

Configuration file is located at `/etc/vapor/vapor.conf`.

Default settings:
```ini
[server]
host = 0.0.0.0
port = 7770
tls = true

[auth]
session_timeout = 3600

[logging]
level = info
```

## Accessing the Web UI

After installation, access Vapor at:
```
https://<server-ip>:7770
```

Default credentials are your system's root or sudo user credentials (PAM authentication).

## API Documentation

- **Swagger UI**: Available at `/docs` endpoint (e.g., `https://localhost:7770/docs`)
- **OpenAPI Spec**: Located at `api/openapi.yaml`

## WebSocket Endpoints

- **Terminal**: `/ws/terminal` - Interactive shell sessions
- **Events**: `/ws/events` - Real-time system events
- **Logs**: `/ws/logs` - Live log streaming

## Development

### Prerequisites

- Go 1.24+
- Node.js 22+
- libvirt-dev (for virtualization support)

### Development Setup

```bash
# Clone repository
git clone https://github.com/awanio/vapor-ce.git
cd vapor

# Install dependencies
make deps

# Run in development mode
make dev
```

### Project Structure

```
vapor/
├── api/           # OpenAPI specifications
├── cmd/           # Application entry points
├── internal/      # Internal packages
├── web/           # Frontend (Lit/TypeScript)
├── scripts/       # Installation and utility scripts
├── ansible/       # Ansible playbooks for installation
└── docs/          # Documentation
```

## Security Notes

- TLS is enabled by default
- Uses PAM for authentication
- Session tokens are JWT-based
- All API endpoints require authentication

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

## License

MIT License - see [LICENSE](LICENSE) for details.

## Acknowledgments

- Inspired by [Cockpit](https://cockpit-project.org/)
- Built with [Go](https://golang.org/), [Lit](https://lit.dev/), and [libvirt](https://libvirt.org/)
