# 📘 Usage Guide

> Detailed instructions on how to use the PowerBridge GitHub Toolkit for various tasks and workflows.

## 📋 Table of Contents

- [🧩 GitHub MCP in Cursor](#-github-mcp-in-cursor)
- [📟 GitHub CLI Commands](#-github-cli-commands)
- [🎤 Voice Command Integration](#-voice-command-integration)
- [📊 Project Management](#-project-management)
- [📝 Documentation Workflows](#-documentation-workflows)
- [👨‍💻 Development Workflows](#-development-workflows)

## 🧩 GitHub MCP in Cursor [⬆️](#-table-of-contents)

The GitHub Model Context Protocol (MCP) integration allows you to interact with GitHub directly from Cursor IDE.

### 🛠️ Available MCP Tools [⬆️](#-table-of-contents)

The GitHub MCP server provides the following tools in Cursor:

#### 📦 Repository Management [⬆️](#-table-of-contents)
- Create repositories
- List repositories
- View repository details
- Fork repositories
- Clone repositories

#### 🔖 Issues and Pull Requests [⬆️](#-table-of-contents)
- Create and manage issues
- Create and review pull requests
- Add comments
- Merge pull requests

#### 🔍 Code and Search [⬆️](#-table-of-contents)
- Search code across repositories
- View file contents
- Create and edit files

#### 👥 Organization Management [⬆️](#-table-of-contents)
- List organizations
- Manage organization repositories
- Manage teams and members

### 🚀 Using MCP Tools in Cursor [⬆️](#-table-of-contents)

1. **Open Cursor IDE**: Start Cursor and open a project
2. **Access GitHub Tools**: Type `/` to open the command palette
3. **Select GitHub Tool**: Choose the appropriate GitHub command
4. **Execute Command**: Follow the prompts to complete the action

### 📋 Example: Creating a Repository with MCP [⬆️](#-table-of-contents)

1. In Cursor, type `/` to open the command palette
2. Type "create repository" and select the GitHub repository creation tool
3. Follow the prompts to specify repository name, visibility, and other options
4. The repository will be created on GitHub

## 📟 GitHub CLI Commands [⬆️](#-table-of-contents)

GitHub CLI (`gh`) provides a command-line interface for GitHub operations.

### 📦 Repository Management [⬆️](#-table-of-contents)

```bash
# List repositories
gh repo list

# List repositories in an organization
gh repo list <organization>

# Create a repository
gh repo create <name> [--public|--private|--internal]

# Clone a repository
gh repo clone <owner>/<repo>

# View repository
gh repo view <owner>/<repo>

# Fork a repository
gh repo fork <owner>/<repo>
```

### 🔖 Issues and Pull Requests [⬆️](#-table-of-contents)

```bash
# List issues
gh issue list

# Create an issue
gh issue create --title "Issue title" --body "Issue description"

# View issue details
gh issue view <issue-number>

# List pull requests
gh pr list

# Create a pull request
gh pr create --title "PR title" --body "PR description"

# Check out a pull request locally
gh pr checkout <pr-number>

# Review a pull request
gh pr review <pr-number> --approve|--request-changes|--comment
```

### 👥 Organization Management [⬆️](#-table-of-contents)

```bash
# List organizations
gh org list

# List teams in an organization
gh api orgs/<org>/teams

# List members of an organization
gh api orgs/<org>/members
```

### 🔧 Advanced Operations [⬆️](#-table-of-contents)

```bash
# Run custom GitHub API queries
gh api <endpoint>

# Create a GitHub Actions workflow
gh workflow create

# View workflow runs
gh run list

# Create a GitHub release
gh release create <tag> [<files>...]
```

## 🎤 Voice Command Integration [⬆️](#-table-of-contents)

The PowerBridge GitHub Toolkit supports voice commands for common GitHub operations.

### ⚙️ Setting Up Voice Commands [⬆️](#-table-of-contents)

1. **Configure Voice Recognition**: Ensure your system has voice recognition enabled
2. **Define Voice Command Mappings**: Map voice phrases to GitHub CLI commands
3. **Test Voice Commands**: Verify that voice commands are recognized correctly

### 📢 Common Voice Commands [⬆️](#-table-of-contents)

- "Create new repository" → `gh repo create`
- "List my repositories" → `gh repo list`
- "Create issue" → `gh issue create`
- "Show pull requests" → `gh pr list`
- "Clone repository" → `gh repo clone`

### 🔧 Custom Voice Command Mapping [⬆️](#-table-of-contents)

You can create custom voice command mappings by editing the voice configuration file:

```json
{
  "voiceCommands": {
    "start new project": "gh repo create --private",
    "show my tasks": "gh issue list --assignee @me",
    "review code": "gh pr list --review-requested @me"
  }
}
```

## 📊 Project Management [⬆️](#-table-of-contents)

The PowerBridge GitHub Toolkit provides powerful tools for project management.

### 🏗️ Creating a Project Structure [⬆️](#-table-of-contents)

```bash
# Create a new project repository
gh repo create my-project --private --description "My new project"

# Clone the repository
gh repo clone my-project

# Set up project structure
mkdir -p src docs tests
touch README.md LICENSE

# Initialize project
git add .
git commit -m "Initial project structure"
git push
```

### 📋 Managing Tasks with Issues [⬆️](#-table-of-contents)

```bash
# Create a project board
gh api repos/<owner>/<repo>/projects -F name="Project Board" -F body="Main project board"

# Create milestone
gh api repos/<owner>/<repo>/milestones -F title="v1.0" -F description="First release"

# Create issues for tasks
gh issue create --title "Setup CI/CD" --body "Configure GitHub Actions for CI/CD"
gh issue create --title "Create documentation" --body "Write project documentation"
```

### 📈 Tracking Progress [⬆️](#-table-of-contents)

```bash
# View open issues
gh issue list --state open

# View milestone progress
gh api repos/<owner>/<repo>/milestones

# Assign issues
gh issue edit <issue-number> --add-assignee @me
```

## 📝 Documentation Workflows [⬆️](#-table-of-contents)

The PowerBridge GitHub Toolkit streamlines documentation creation and maintenance.

### 📚 Setting Up Documentation [⬆️](#-table-of-contents)

```bash
# Create documentation structure
mkdir -p docs/{getting-started,api,examples,reference}
touch docs/README.md

# Create documentation template
cat > docs/README.md << 'EOF'
# Project Documentation

## Table of Contents

- [Getting Started](getting-started/README.md)
- [API Documentation](api/README.md)
- [Examples](examples/README.md)
- [Reference](reference/README.md)
EOF
```

### ✅ Documentation Validation Process [⬆️](#-table-of-contents)

1. **Create Documentation**: Write documentation files in Markdown format
2. **Validate Structure**: Ensure documentation follows the defined structure
3. **Check Links**: Verify that all internal and external links are valid
4. **Review Content**: Perform content review for accuracy and clarity
5. **Apply Formatting**: Apply consistent formatting according to standards

### 🔄 Documentation Update Workflow [⬆️](#-table-of-contents)

```bash
# Create documentation branch
git checkout -b docs/update-guides

# Make documentation changes
# ...

# Commit changes
git add docs/
git commit -m "Update documentation guides"

# Push changes
git push -u origin docs/update-guides

# Create pull request
gh pr create --title "Update documentation guides" --body "Updates the user guides with new features"
```

## 👨‍💻 Development Workflows [⬆️](#-table-of-contents)

### 🔄 Feature Development Workflow [⬆️](#-table-of-contents)

1. **Create Feature Branch**:
   ```bash
   git checkout -b feature/new-feature
   ```

2. **Implement Feature**:
   - Write code for the feature
   - Add tests
   - Update documentation

3. **Create Pull Request**:
   ```bash
   git push -u origin feature/new-feature
   gh pr create --title "Add new feature" --body "Implements new feature"
   ```

4. **Code Review**:
   ```bash
   gh pr view --web # Open PR in browser for review
   ```

5. **Merge Feature**:
   ```bash
   gh pr merge --squash
   ```

### 🐛 Bug Fix Workflow [⬆️](#-table-of-contents)

1. **Create Bug Fix Branch**:
   ```bash
   git checkout -b fix/bug-description
   ```

2. **Implement Fix**:
   - Identify root cause
   - Write fix
   - Add tests

3. **Create Pull Request**:
   ```bash
   git push -u origin fix/bug-description
   gh pr create --title "Fix bug" --body "Fixes #123"
   ```

4. **Merge Fix**:
   ```bash
   gh pr merge --squash
   ```

### 🚀 Release Workflow [⬆️](#-table-of-contents)

1. **Create Release Branch**:
   ```bash
   git checkout -b release/v1.0.0
   ```

2. **Update Version**:
   - Update version numbers
   - Update CHANGELOG.md

3. **Create Release**:
   ```bash
   gh release create v1.0.0 --title "v1.0.0" --notes "Release notes for v1.0.0"
   ```

4. **Merge to Main**:
   ```bash
   git checkout main
   git merge release/v1.0.0
   git push
   ```

---

Made with Power, Love, and AI • ⚡️❤️🤖 •  POWERBRIDGE.AI 