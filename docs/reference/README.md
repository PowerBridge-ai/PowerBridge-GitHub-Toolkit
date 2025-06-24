# 📖 Reference Guide

> Comprehensive reference for GitHub MCP tools, GitHub CLI commands, configuration options, voice commands, and workflows.

## 📋 Table of Contents

- [🔍 Introduction](#-introduction)
- [🧩 GitHub MCP Tools](#-github-mcp-tools)
- [📟 GitHub CLI Commands](#-github-cli-commands)
- [⚙️ Configuration Options](#-configuration-options)
- [🎤 Voice Commands](#-voice-commands)
- [🔄 Workflows](#-workflows)
- [📚 Cursor Rules](#-cursor-rules)

## 🔍 Introduction [⬆️](#-table-of-contents)

This reference guide provides detailed information about the tools, commands, and features available in the PowerBridge GitHub Toolkit. Use this guide as a comprehensive resource for understanding and utilizing the toolkit's capabilities.

## 🧩 GitHub MCP Tools [⬆️](#-table-of-contents)

GitHub Model Context Protocol (MCP) provides tools for interacting with GitHub repositories, issues, pull requests, and more directly from Cursor IDE.

### Repository Tools

| Tool | Description | Parameters |
|------|-------------|------------|
| `mcp_github_create_repository` | Create a new GitHub repository | `name`, `description`, `private`, `autoInit` |
| `mcp_github_get_repository` | Get details of a GitHub repository | `owner`, `repo` |
| `mcp_github_list_repositories` | List repositories for a user or organization | `username`, `type`, `sort`, `direction` |
| `mcp_github_fork_repository` | Fork a repository to your account | `owner`, `repo`, `organization` |

### Issue Tools

| Tool | Description | Parameters |
|------|-------------|------------|
| `mcp_github_create_issue` | Create a new issue | `owner`, `repo`, `title`, `body`, `labels`, `assignees` |
| `mcp_github_get_issue` | Get details of an issue | `owner`, `repo`, `issue_number` |
| `mcp_github_list_issues` | List issues in a repository | `owner`, `repo`, `state`, `labels`, `sort` |
| `mcp_github_update_issue` | Update an existing issue | `owner`, `repo`, `issue_number`, `title`, `body`, `state` |

### Pull Request Tools

| Tool | Description | Parameters |
|------|-------------|------------|
| `mcp_github_create_pull_request` | Create a new pull request | `owner`, `repo`, `title`, `body`, `head`, `base` |
| `mcp_github_get_pull_request` | Get details of a pull request | `owner`, `repo`, `pullNumber` |
| `mcp_github_list_pull_requests` | List pull requests in a repository | `owner`, `repo`, `state`, `head`, `base` |
| `mcp_github_merge_pull_request` | Merge a pull request | `owner`, `repo`, `pullNumber`, `commit_message` |

### Content Tools

| Tool | Description | Parameters |
|------|-------------|------------|
| `mcp_github_get_file_contents` | Get contents of a file | `owner`, `repo`, `path`, `branch` |
| `mcp_github_create_or_update_file` | Create or update a file | `owner`, `repo`, `path`, `content`, `message`, `branch` |
| `mcp_github_delete_file` | Delete a file | `owner`, `repo`, `path`, `message`, `branch` |

## 📟 GitHub CLI Commands [⬆️](#-table-of-contents)

GitHub CLI (`gh`) provides a command-line interface for working with GitHub.

### Repository Commands

```bash
# Create a repository
gh repo create [name] [flags]

# Clone a repository
gh repo clone [repository] [directory] [flags]

# View a repository
gh repo view [repository] [flags]

# List repositories
gh repo list [owner] [flags]

# Fork a repository
gh repo fork [repository] [flags]
```

### Issue Commands

```bash
# Create an issue
gh issue create [flags]

# List issues
gh issue list [flags]

# View an issue
gh issue view [issue-number] [flags]

# Close an issue
gh issue close [issue-number] [flags]

# Reopen an issue
gh issue reopen [issue-number] [flags]
```

### Pull Request Commands

```bash
# Create a pull request
gh pr create [flags]

# List pull requests
gh pr list [flags]

# View a pull request
gh pr view [pull-request-number] [flags]

# Check out a pull request
gh pr checkout [pull-request-number] [flags]

# Merge a pull request
gh pr merge [pull-request-number] [flags]
```

## ⚙️ Configuration Options [⬆️](#-table-of-contents)

### GitHub MCP Configuration

The GitHub MCP server can be configured using the following options:

```json
{
  "github": {
    "token": "your-github-token",
    "username": "your-github-username",
    "email": "your-github-email"
  },
  "mcp": {
    "port": 3000,
    "host": "localhost",
    "logLevel": "info"
  }
}
```

### GitHub CLI Configuration

GitHub CLI can be configured using the following commands:

```bash
# Set your GitHub token
gh auth login

# Configure git protocol
gh config set git_protocol ssh

# Set default editor
gh config set editor vim

# Set browser for opening links
gh config set browser firefox
```

## 🎤 Voice Commands [⬆️](#-table-of-contents)

The PowerBridge GitHub Toolkit supports voice commands for common GitHub operations.

### Repository Voice Commands

| Command | Description | Example |
|---------|-------------|---------|
| Create repository | Create a new GitHub repository | "Create repository my-project private in organization my-org" |
| Clone repository | Clone a GitHub repository | "Clone repository powerbridge/github-toolkit" |
| Fork repository | Fork a GitHub repository | "Fork repository microsoft/vscode" |

### Issue Voice Commands

| Command | Description | Example |
|---------|-------------|---------|
| Create issue | Create a new issue | "Create issue 'Fix login bug' with body 'The login form is not working'" |
| List issues | List issues in a repository | "List issues with label bug" |
| Close issue | Close an issue | "Close issue 42 with comment 'Fixed in latest release'" |

### Pull Request Voice Commands

| Command | Description | Example |
|---------|-------------|---------|
| Create pull request | Create a new pull request | "Create pull request 'Add login feature' from branch feature/login" |
| List pull requests | List pull requests in a repository | "List pull requests assigned to me" |
| Merge pull request | Merge a pull request | "Merge pull request 42" |

### Workflow Voice Commands

| Command | Description | Example |
|---------|-------------|---------|
| Initialize project | Initialize a new project | "Initialize project my-project private in organization my-org" |
| Setup development workflow | Set up a development workflow | "Setup development workflow with branch develop" |
| Create release | Create a new release | "Create release v1.0.0 with notes 'Initial release'" |

## 🔄 Workflows [⬆️](#-table-of-contents)

The PowerBridge GitHub Toolkit includes predefined workflows for common tasks.

### Documentation Workflow

1. Initialize documentation structure
2. Create core documentation files
3. Set up documentation validation
4. Configure automatic updates
5. Publish documentation

### Development Workflow

1. Initialize project repository
2. Set up branch protection rules
3. Configure CI/CD pipeline
4. Implement feature branches
5. Create pull request templates
6. Set up code review process

### Project Management Workflow

1. Create project board
2. Define issue templates
3. Set up milestone tracking
4. Configure automated issue assignment
5. Set up status reporting

## 📚 Cursor Rules [⬆️](#-table-of-contents)

The PowerBridge GitHub Toolkit includes custom Cursor IDE rules that enhance your development workflow.

### AI Documentation Workflow

The [AI Documentation Workflow](../../cursor-rules/ai-documentation-workflow.cursor) rule automates documentation creation and maintenance.

```javascript
// Create project documentation
const docs = DocumentationWorkflow.createProjectDocs('my-project');

// Generate API documentation
await docs.generateApiDocs();

// Update documentation with changes
await docs.updateFromChanges();
```

### AI Documentation Formatting

The [AI Documentation Formatting](../../cursor-rules/ai-documentation-formatting.cursor) rule standardizes document formatting with consistent styles.

```javascript
// Format a README file
const readmeContent = await DocumentationFormatting.formatReadme('MyProject', {
  tagline: 'A powerful project management tool',
  features: [
    'Feature one description',
    'Feature two description'
  ]
});

// Format a user guide
const userGuideContent = await DocumentationFormatting.formatUserGuide('MyProject');

// Apply standard formatting to any document
const formattedContent = await DocumentationFormatting.formatDocument(existingContent);
```

### AI Debugging Workflow

The [AI Debugging Workflow](../../cursor-rules/ai-debugging-workflow.cursor) rule implements structured debugging with emoji-based logging.

```javascript
// Create a component-specific logger
const logger = DebuggingWorkflow.createLogger('AuthService');

// Log messages with different levels
logger.info('🔑', 'User authentication started');
logger.debug('🔍', 'Checking credentials');
logger.success('✅', 'Authentication successful');
logger.error('❌', 'Authentication failed');

// Use advanced logging features
const advLogger = DebuggingWorkflow.createAdvancedLogger('ApiService');
advLogger.startTimer('fetchData');
// ... perform operation
const duration = advLogger.endTimer('fetchData');
```

### AI Checkpoint System

The [AI Checkpoint System](../../cursor-rules/ai-checkpoint-system.cursor) rule provides checkpoints for saving and restoring work states.

```javascript
// Initialize a checkpoint session
const sessionId = await CheckpointSystem.initializeSession('Document Editing');

// Create a checkpoint
const checkpointId = await CheckpointSystem.createCheckpoint('Initial Draft', {
  content: documentContent
});

// Add notes to the checkpoint history
await CheckpointSystem.addNote('Added introduction section');

// Restore from a checkpoint
const restoredData = await CheckpointSystem.restoreCheckpoint(checkpointId);

// End the session
await CheckpointSystem.endSession('Completed document editing');
```

### GitHub Automation Workflow

The [GitHub Automation Workflow](../../cursor-rules/github-automation-workflow.cursor) rule automates GitHub operations via voice commands.

```javascript
// Create a repository
const repoResult = await GitHubAutomation.repository.createRepository({
  name: 'my-project',
  description: 'A new project',
  isPrivate: true,
  organization: 'my-org'
});

// Create an issue
const issueResult = await GitHubAutomation.issues.createIssue({
  title: 'Fix login bug',
  body: 'The login form is not working',
  labels: ['bug', 'high-priority']
});

// Process voice commands
const result = await GitHubAutomation.voiceCommands.processCommand(
  'create pull request "Add login feature" from branch feature/login'
);
```

---

Made with Power, Love, and AI • ⚡️❤️🤖 •  POWERBRIDGE.AI 