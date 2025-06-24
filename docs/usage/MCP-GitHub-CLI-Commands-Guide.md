# 🤖 MCP GitHub CLI Commands Guide

## 📋 Table of Contents
- [Introduction](#introduction)
- [Setup and Configuration](#setup-and-configuration)
- [MCP GitHub Commands Reference](#mcp-github-commands-reference)
- [Voice Command Integration](#voice-command-integration)
- [Chat Command Integration](#chat-command-integration)
- [Project Management Workflows](#project-management-workflows)
- [Task Management Templates](#task-management-templates)
- [Advanced Automation](#advanced-automation)
- [Troubleshooting](#troubleshooting)

## 🔍 Introduction

This comprehensive guide provides detailed instructions for using GitHub MCP (Model Context Protocol) commands and CLI operations for repository management, issue tracking, and project automation. The guide integrates with the PowerBridge GitHub Toolkit and Cursor AI to create a seamless development workflow.

## 🛠️ Setup and Configuration

### GitHub MCP Server Setup

#### Remote Server Configuration (VS Code 1.101+)
```json
{
  "servers": {
    "github": {
      "type": "http",
      "url": "https://api.githubcopilot.com/mcp/"
    }
  }
}
```

#### Local Server Configuration with Docker
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
  "servers": {
    "github": {
      "command": "docker",
      "args": [
        "run",
        "-i",
        "--rm",
        "-e",
        "GITHUB_PERSONAL_ACCESS_TOKEN",
        "ghcr.io/github/github-mcp-server"
      ],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "${input:github_token}"
      }
    }
  }
}
```

#### WSL Configuration (Windows)
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
        "/home/user/run-github-mcp.sh"
      ]
    }
  }
}
```

### Required Permissions

Create a GitHub Personal Access Token with these permissions:
- `repo` - Full control of repositories
- `workflow` - Update GitHub Action workflows
- `read:org` - Read organization data
- `user` - Update user data
- `project` - Full control of projects

## 📝 MCP GitHub Commands Reference

### Repository Management

| Command | Parameters | Description | Example |
|---------|------------|-------------|---------|
| `mcp_github_create_repository` | name, description, private, autoInit | Create a new repository | `mcp_github_create_repository({ "name": "new-project", "description": "My new project", "private": true, "autoInit": true })` |
| `mcp_github_fork_repository` | owner, repo, organization | Fork a repository | `mcp_github_fork_repository({ "owner": "github", "repo": "github-mcp-server" })` |
| `mcp_github_create_branch` | owner, repo, branch, from_branch | Create a new branch | `mcp_github_create_branch({ "owner": "user", "repo": "project", "branch": "feature/auth", "from_branch": "main" })` |
| `mcp_github_list_branches` | owner, repo, page, perPage | List repository branches | `mcp_github_list_branches({ "owner": "user", "repo": "project" })` |
| `mcp_github_get_commit` | owner, repo, sha | Get commit details | `mcp_github_get_commit({ "owner": "user", "repo": "project", "sha": "main" })` |
| `mcp_github_list_commits` | owner, repo, sha, page, perPage | List commits | `mcp_github_list_commits({ "owner": "user", "repo": "project", "sha": "main" })` |

### File Operations

| Command | Parameters | Description | Example |
|---------|------------|-------------|---------|
| `mcp_github_get_file_contents` | owner, repo, path, branch | Get file contents | `mcp_github_get_file_contents({ "owner": "user", "repo": "project", "path": "README.md" })` |
| `mcp_github_create_or_update_file` | owner, repo, path, content, message, branch, sha | Create or update file | `mcp_github_create_or_update_file({ "owner": "user", "repo": "project", "path": "README.md", "content": "# Project", "message": "Update README", "branch": "main" })` |
| `mcp_github_delete_file` | owner, repo, path, message, branch | Delete a file | `mcp_github_delete_file({ "owner": "user", "repo": "project", "path": "old-file.md", "message": "Remove old file", "branch": "main" })` |
| `mcp_github_push_files` | owner, repo, branch, files, message | Push multiple files | `mcp_github_push_files({ "owner": "user", "repo": "project", "branch": "main", "files": [{"path": "file1.md", "content": "content"}], "message": "Add files" })` |

### Issue Management

| Command | Parameters | Description | Example |
|---------|------------|-------------|---------|
| `mcp_github_create_issue` | owner, repo, title, body, labels, assignees, milestone | Create an issue | `mcp_github_create_issue({ "owner": "user", "repo": "project", "title": "[BAD-001] Implement Auth", "body": "Details...", "labels": ["enhancement"] })` |
| `mcp_github_get_issue` | owner, repo, issue_number | Get issue details | `mcp_github_get_issue({ "owner": "user", "repo": "project", "issue_number": 1 })` |
| `mcp_github_update_issue` | owner, repo, issue_number, title, body, state, labels, assignees, milestone | Update an issue | `mcp_github_update_issue({ "owner": "user", "repo": "project", "issue_number": 1, "state": "closed" })` |
| `mcp_github_add_issue_comment` | owner, repo, issue_number, body | Comment on issue | `mcp_github_add_issue_comment({ "owner": "user", "repo": "project", "issue_number": 1, "body": "Comment text" })` |
| `mcp_github_get_issue_comments` | owner, repo, issue_number, page, per_page | Get issue comments | `mcp_github_get_issue_comments({ "owner": "user", "repo": "project", "issue_number": 1 })` |
| `mcp_github_list_issues` | owner, repo, state, labels, sort, direction, since, page, perPage | List issues | `mcp_github_list_issues({ "owner": "user", "repo": "project", "state": "open" })` |

### Pull Request Management

| Command | Parameters | Description | Example |
|---------|------------|-------------|---------|
| `mcp_github_create_pull_request` | owner, repo, title, body, head, base, draft, maintainer_can_modify | Create a PR | `mcp_github_create_pull_request({ "owner": "user", "repo": "project", "title": "Feature: Auth", "body": "Implements...", "head": "feature/auth", "base": "main" })` |
| `mcp_github_get_pull_request` | owner, repo, pullNumber | Get PR details | `mcp_github_get_pull_request({ "owner": "user", "repo": "project", "pullNumber": 1 })` |
| `mcp_github_get_pull_request_files` | owner, repo, pullNumber | Get PR files | `mcp_github_get_pull_request_files({ "owner": "user", "repo": "project", "pullNumber": 1 })` |
| `mcp_github_get_pull_request_diff` | owner, repo, pullNumber | Get PR diff | `mcp_github_get_pull_request_diff({ "owner": "user", "repo": "project", "pullNumber": 1 })` |
| `mcp_github_get_pull_request_status` | owner, repo, pullNumber | Get PR status | `mcp_github_get_pull_request_status({ "owner": "user", "repo": "project", "pullNumber": 1 })` |
| `mcp_github_merge_pull_request` | owner, repo, pullNumber, commit_title, commit_message, merge_method | Merge a PR | `mcp_github_merge_pull_request({ "owner": "user", "repo": "project", "pullNumber": 1, "merge_method": "squash" })` |
| `mcp_github_list_pull_requests` | owner, repo, state, head, base, sort, direction, page, perPage | List PRs | `mcp_github_list_pull_requests({ "owner": "user", "repo": "project", "state": "open" })` |

### Search Operations

| Command | Parameters | Description | Example |
|---------|------------|-------------|---------|
| `mcp_github_search_code` | q, sort, order, page, perPage | Search code | `mcp_github_search_code({ "q": "function in:file language:javascript repo:user/project" })` |
| `mcp_github_search_issues` | q, sort, order, page, perPage | Search issues | `mcp_github_search_issues({ "q": "is:issue is:open label:bug repo:user/project" })` |
| `mcp_github_search_repositories` | query, page, perPage | Search repos | `mcp_github_search_repositories({ "query": "topic:javascript stars:>1000" })` |
| `mcp_github_search_users` | q, sort, order, page, perPage | Search users | `mcp_github_search_users({ "q": "location:london language:javascript" })` |

### Security Operations

| Command | Parameters | Description | Example |
|---------|------------|-------------|---------|
| `mcp_github_get_code_scanning_alert` | owner, repo, alertNumber | Get code scanning alert | `mcp_github_get_code_scanning_alert({ "owner": "user", "repo": "project", "alertNumber": 1 })` |
| `mcp_github_list_code_scanning_alerts` | owner, repo, ref, tool_name, state, severity | List code scanning alerts | `mcp_github_list_code_scanning_alerts({ "owner": "user", "repo": "project", "state": "open" })` |
| `mcp_github_get_secret_scanning_alert` | owner, repo, alertNumber | Get secret scanning alert | `mcp_github_get_secret_scanning_alert({ "owner": "user", "repo": "project", "alertNumber": 1 })` |
| `mcp_github_list_secret_scanning_alerts` | owner, repo, state, secret_type, resolution | List secret scanning alerts | `mcp_github_list_secret_scanning_alerts({ "owner": "user", "repo": "project", "state": "open" })` |

### User Operations

| Command | Parameters | Description | Example |
|---------|------------|-------------|---------|
| `mcp_github_get_me` | reason | Get authenticated user | `mcp_github_get_me({ "reason": "Check permissions" })` |

### Notification Operations

| Command | Parameters | Description | Example |
|---------|------------|-------------|---------|
| `mcp_github_list_notifications` | filter, owner, repo, since, before, page, perPage | List notifications | `mcp_github_list_notifications({ "filter": "default" })` |
| `mcp_github_get_notification_details` | notificationID | Get notification details | `mcp_github_get_notification_details({ "notificationID": "12345" })` |
| `mcp_github_dismiss_notification` | threadID, state | Dismiss notification | `mcp_github_dismiss_notification({ "threadID": "12345", "state": "read" })` |

## 🎤 Voice Command Integration

### Repository Management Voice Commands

```bash
# Create a new repository
"Create a new GitHub repository called project-name with description 'Project description'"

# Create a new branch
"Create a branch called feature/auth from main in the user/project repository"

# Clone a repository
"Clone the github/github-mcp-server repository"
```

### Issue Management Voice Commands

```bash
# Create a new task/issue
"Create a task to implement user authentication with JWT"

# Update issue status
"Mark issue BAD-001 as in progress"

# Assign issue
"Assign issue BAD-001 to @developer"

# Add labels
"Add security and backend labels to issue BAD-001"
```

### Project Management Voice Commands

```bash
# Create a project
"Create a new GitHub project called Authentication System"

# Add to project
"Add issue BAD-001 to the Authentication project board"

# Move card
"Move issue BAD-001 to In Progress column"

# Create sprint
"Create sprint 1 with duration of 2 weeks"
```

## 💬 Chat Command Integration

### Repository Management Chat Commands

```bash
# Create repository
/repo create --name "project-name" --description "Project description" --private

# Create branch
/branch create --repo "user/project" --name "feature/auth" --from "main"

# Push changes
/repo push --files "README.md,index.js" --message "Initial commit"
```

### Issue Management Chat Commands

```bash
# Create issue
/issue create --title "[BAD-001] Implement Auth" --body "Details..." --labels "enhancement,security"

# Update issue
/issue update BAD-001 --status "In Progress" --assignee "@developer"

# Comment on issue
/issue comment BAD-001 "Added JWT implementation"
```

### Project Management Chat Commands

```bash
# Create project
/project create "Authentication System" --template "kanban"

# Add to project
/project add BAD-001 --column "To Do"

# Create sprint
/sprint create "Sprint 1" --duration 2w --goal "Complete authentication system"
```

## 📊 Project Management Workflows

### Repository Creation Workflow

1. **Create Repository**
   ```bash
   # Voice command
   "Create a new GitHub repository called auth-service with description 'Authentication service'"
   
   # MCP command
   mcp_github_create_repository({ "name": "auth-service", "description": "Authentication service", "private": true, "autoInit": true })
   ```

2. **Create Project Structure**
   ```bash
   # Push initial files
   mcp_github_push_files({
     "owner": "user",
     "repo": "auth-service",
     "branch": "main",
     "files": [
       {"path": "README.md", "content": "# Authentication Service\n\nService for user authentication."},
       {"path": ".gitignore", "content": "node_modules/\n.env\n"},
       {"path": "package.json", "content": "{\n  \"name\": \"auth-service\",\n  \"version\": \"1.0.0\"\n}"}
     ],
     "message": "Initial project structure"
   })
   ```

3. **Create Project Board**
   ```bash
   # Voice command
   "Create a new project board for auth-service with columns To Do, In Progress, and Done"
   ```

### Issue Creation Workflow

1. **Create Issue Template**
   ```bash
   # Create issue template file
   mcp_github_create_or_update_file({
     "owner": "user",
     "repo": "auth-service",
     "path": ".github/ISSUE_TEMPLATE/feature.md",
     "content": "---\nname: Feature\ndescription: Feature request\n---\n\n## Description\n\n## Requirements\n\n## Acceptance Criteria",
     "message": "Add feature issue template",
     "branch": "main"
   })
   ```

2. **Create Issue**
   ```bash
   # Voice command
   "Create a task to implement JWT authentication with requirements must support refresh tokens and assign to @security-team"
   
   # MCP command
   mcp_github_create_issue({
     "owner": "user",
     "repo": "auth-service",
     "title": "[BAD-001] Implement JWT Authentication",
     "body": "## Description\nImplement JWT authentication\n\n## Requirements\n- [ ] Support refresh tokens\n- [ ] Secure storage\n- [ ] Token expiration\n\n## Acceptance Criteria\n- [ ] Users can authenticate\n- [ ] Tokens can be refreshed\n- [ ] Expired tokens are rejected",
     "labels": ["enhancement", "security"],
     "assignees": ["security-team"]
   })
   ```

3. **Add to Project**
   ```bash
   # Voice command
   "Add issue BAD-001 to the Authentication project board in To Do column"
   ```

### Sprint Management Workflow

1. **Create Sprint**
   ```bash
   # Voice command
   "Create sprint 1 with duration of 2 weeks and goal to complete authentication system"
   ```

2. **Add Issues to Sprint**
   ```bash
   # Voice command
   "Add issues BAD-001, BAD-002, and BAD-003 to sprint 1"
   ```

3. **Start Sprint**
   ```bash
   # Voice command
   "Start sprint 1 and notify the team"
   ```

4. **Track Progress**
   ```bash
   # Voice command
   "Show progress on sprint 1"
   ```

## 📝 Task Management Templates

### Standard Issue Template

```markdown
[BAD-XXX] 🔧 Task Title

## 🚀 Feature Description
Detailed description of the feature or task.

## 📋 Requirements
- [ ] Requirement 1
- [ ] Requirement 2
- [ ] Requirement 3

## 📝 Implementation Steps
1. Step 1
2. Step 2
3. Step 3

## 🎨 UI/UX Requirements
- Design specification 1
- Design specification 2

## 🔗 Related Components
- Component 1
- Component 2

## 💻 Technical Specifications
```code
// Code example
```

## 🧪 Testing Requirements
- Test case 1
- Test case 2

## 📚 Documentation Requirements
- Update API docs
- Update user guide

## ⚡ Performance Specifications
- Performance metric 1
- Performance metric 2

## 🔒 Security Considerations
- Security requirement 1
- Security requirement 2

## ✅ Acceptance Criteria
- [ ] Criterion 1
- [ ] Criterion 2
- [ ] Criterion 3
```

### Task ID Assignment

1. **Format**: `[BAD-XXX]` where XXX is a sequential number
2. **Check existing issues** to determine the next available number
3. **Never reuse** task IDs, even if an old task is closed/deleted

### Task Labeling System

| Category | Available Labels |
|----------|------------------|
| **Type** | `enhancement`, `bug`, `feature`, `documentation`, `refactor` |
| **Area** | `frontend`, `backend`, `design`, `devops`, `testing` |
| **Priority** | `high-priority`, `medium`, `low` |
| **Status** | `blocked`, `in-progress`, `ready-for-review` |

## 🔧 Advanced Automation

### GitHub Actions Integration

Create a workflow file for automated issue management:

```yaml
# .github/workflows/issue-automation.yml
name: Issue Automation
on:
  issues:
    types: [opened, edited, labeled, unlabeled, assigned, unassigned]

jobs:
  process_issue:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Process Issue
        uses: actions/github-script@v6
        with:
          github-token: ${{ secrets.GITHUB_TOKEN }}
          script: |
            const issue = context.payload.issue;
            
            // Add to project board
            if (issue.labels.some(label => label.name === 'feature')) {
              // Add to feature board
            }
            
            // Update task-log.md
            if (issue.title.startsWith('[BAD-')) {
              // Update task log
            }
```

### Automated Documentation Updates

Create a script to update documentation based on issue changes:

```javascript
// scripts/update-docs.js
async function updateTaskLog(issue) {
  const taskId = issue.title.match(/\[BAD-(\d+)\]/)[1];
  const status = getIssueStatus(issue);
  
  // Update task-log.md
  const taskLog = fs.readFileSync('task-log.md', 'utf8');
  const updatedTaskLog = updateTaskStatus(taskLog, taskId, status);
  fs.writeFileSync('task-log.md', updatedTaskLog);
  
  // Add comment to issue
  await github.issues.createComment({
    owner: context.repo.owner,
    repo: context.repo.repo,
    issue_number: issue.number,
    body: `Updated task-log.md with status: ${status}`
  });
}
```

## ❗ Troubleshooting

### Common MCP Server Issues

1. **Connection Problems**
   - Error: "Failed to create MCP server"
   - Solution: Check GitHub token permissions and expiration

2. **Command Execution Failures**
   - Error: "Failed to execute command"
   - Solution: Verify parameter format and required fields

3. **Permission Issues**
   - Error: "Resource not accessible by integration"
   - Solution: Update GitHub token with required permissions

### Debugging Process

1. Check MCP server logs
2. Verify GitHub token permissions
3. Test command with minimal parameters
4. Check for rate limiting issues
5. Verify repository access

---

Made with Power, Love, and AI •  ⚡️❤️🤖 •  POWERBRIDGE.AI 