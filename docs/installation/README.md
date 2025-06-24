# 🛠️ Installation Guide

> Detailed instructions for installing and configuring the PowerBridge GitHub Toolkit on different platforms.

## 📋 Table of Contents

- [🔍 Prerequisites](#-prerequisites)
- [🪟 Windows Installation](#-windows-installation)
- [🐧 Linux Installation](#-linux-installation)
- [⚙️ Configuration](#-configuration)
- [✅ Verification](#-verification)
- [🔧 Troubleshooting](#-troubleshooting)

## 🔍 Prerequisites [⬆️](#-table-of-contents)

Before installing the PowerBridge GitHub Toolkit, ensure you have:

1. **GitHub Account**: Active GitHub account with repository access
2. **GitHub Personal Access Token**: Token with appropriate permissions (repo, read:org, workflow)
3. **Cursor IDE**: Latest version installed
4. **Docker**: For running the GitHub MCP server
5. **WSL** (Windows only): Windows Subsystem for Linux with Ubuntu installed

## 🪟 Windows Installation [⬆️](#-table-of-contents)

### 1. Install WSL and Ubuntu [⬆️](#-table-of-contents)

If not already installed:

```powershell
# Install WSL
wsl --install

# Install Ubuntu distribution
wsl --install -d Ubuntu
```

### 2. Install Docker in WSL Ubuntu [⬆️](#-table-of-contents)

Open Ubuntu and run:

```bash
# Update package lists
sudo apt update

# Install prerequisites
sudo apt install -y apt-transport-https ca-certificates curl software-properties-common

# Add Docker's official GPG key
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

# Add Docker repository
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

# Install Docker
sudo apt update
sudo apt install -y docker-ce

# Add your user to the docker group
sudo usermod -aG docker $USER

# Start Docker service
sudo service docker start
```

### 3. Install GitHub CLI [⬆️](#-table-of-contents)

```powershell
# Using winget
winget install --id GitHub.cli

# Or using Chocolatey
choco install gh
```

### 4. Create GitHub MCP Server Script [⬆️](#-table-of-contents)

In your WSL Ubuntu environment, create a script file:

```bash
# Create the script
cat > ~/run-github-mcp.sh << 'EOF'
#!/bin/bash
docker run --rm -i -e GITHUB_TOKEN="$github_token" ghcr.io/github/github-mcp-server:latest server stdio
EOF

# Make it executable
chmod +x ~/run-github-mcp.sh
```

### 5. Configure Cursor MCP Integration [⬆️](#-table-of-contents)

Create or edit the file at `C:\Users\<username>\.cursor\mcp.json`:

```json
{
  "inputs": [
    {
      "type": "promptString",
      "id": "github_token",
      "description": "GitHub Personal Access Token",
      "password": true
    }
  ],
  "mcpServers": {
    "github": {
      "command": "wsl",
      "args": [
        "-d",
        "Ubuntu",
        "--",
        "/home/<username>/run-github-mcp.sh"
      ]
    }
  }
}
```

Replace `<username>` with your WSL Ubuntu username.

## 🐧 Linux Installation [⬆️](#-table-of-contents)

### 1. Install Docker [⬆️](#-table-of-contents)

```bash
# Update package lists
sudo apt update

# Install prerequisites
sudo apt install -y apt-transport-https ca-certificates curl software-properties-common

# Add Docker's official GPG key
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

# Add Docker repository
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

# Install Docker
sudo apt update
sudo apt install -y docker-ce

# Add your user to the docker group
sudo usermod -aG docker $USER

# Start Docker service
sudo systemctl start docker
sudo systemctl enable docker
```

### 2. Install GitHub CLI [⬆️](#-table-of-contents)

```bash
# Add GitHub CLI repository
curl -fsSL https://cli.github.com/packages/githubcli-archive-keyring.gpg | sudo dd of=/usr/share/keyrings/githubcli-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | sudo tee /etc/apt/sources.list.d/github-cli.list > /dev/null

# Install GitHub CLI
sudo apt update
sudo apt install gh
```

### 3. Create GitHub MCP Server Script [⬆️](#-table-of-contents)

```bash
# Create the script
cat > ~/run-github-mcp.sh << 'EOF'
#!/bin/bash
docker run --rm -i -e GITHUB_TOKEN="$github_token" ghcr.io/github/github-mcp-server:latest server stdio
EOF

# Make it executable
chmod +x ~/run-github-mcp.sh
```

### 4. Configure Cursor MCP Integration [⬆️](#-table-of-contents)

Create or edit the file at `~/.cursor/mcp.json`:

```json
{
  "inputs": [
    {
      "type": "promptString",
      "id": "github_token",
      "description": "GitHub Personal Access Token",
      "password": true
    }
  ],
  "mcpServers": {
    "github": {
      "command": "/home/<username>/run-github-mcp.sh",
      "args": []
    }
  }
}
```

Replace `<username>` with your Linux username.

## ⚙️ Configuration [⬆️](#-table-of-contents)

### 🔐 GitHub Authentication [⬆️](#-table-of-contents)

1. **GitHub CLI Authentication**:

```bash
# Authenticate with GitHub
gh auth login
```

2. **Verify GitHub CLI Authentication**:

```bash
# Check authentication status
gh auth status
```

### 🔑 GitHub Personal Access Token [⬆️](#-table-of-contents)

Create a GitHub Personal Access Token with the following permissions:

- `repo`: Full control of repositories
- `read:org`: Read organization data
- `workflow`: Update GitHub Action workflows

## ✅ Verification [⬆️](#-table-of-contents)

### 🧪 Test GitHub MCP Server [⬆️](#-table-of-contents)

1. Start Cursor IDE
2. Open a project with GitHub integration
3. Try using GitHub-related commands in Cursor

### 🧪 Test GitHub CLI [⬆️](#-table-of-contents)

```bash
# List your repositories
gh repo list

# List organizations
gh org list

# Create a test repository
gh repo create test-repo --private --description "Test repository"
```

## 🔧 Troubleshooting [⬆️](#-table-of-contents)

### 🚨 Common Issues [⬆️](#-table-of-contents)

#### 🔒 Authentication Issues [⬆️](#-table-of-contents)

**Problem**: Unable to authenticate with GitHub.

**Solution**: 
1. Verify your GitHub token has the correct permissions
2. Ensure the token is correctly set in the configuration
3. Try re-authenticating with `gh auth login`

#### 🐳 Docker Issues [⬆️](#-table-of-contents)

**Problem**: Docker service not starting or container errors.

**Solution**:
1. Check Docker service status with `sudo service docker status` (WSL) or `sudo systemctl status docker` (Linux)
2. Ensure your user is in the docker group
3. Restart Docker with `sudo service docker restart` (WSL) or `sudo systemctl restart docker` (Linux)

#### 🔌 MCP Server Connection Issues [⬆️](#-table-of-contents)

**Problem**: Cursor cannot connect to the MCP server.

**Solution**:
1. Verify the MCP server script is executable
2. Check the path in `mcp.json` is correct
3. Ensure the GitHub token is being passed correctly

---

Made with Power, Love, and AI • ⚡️❤️🤖 •  POWERBRIDGE.AI 