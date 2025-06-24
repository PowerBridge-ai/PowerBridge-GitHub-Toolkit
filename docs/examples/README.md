# 📋 Examples

> Practical examples of using the PowerBridge GitHub Toolkit for various tasks and workflows.

## 📑 Table of Contents

- [📝 Documentation Examples](#-documentation-examples)
- [📊 Project Management Examples](#-project-management-examples)
- [👨‍💻 Development Workflow Examples](#-development-workflow-examples)
- [🎤 Voice Command Examples](#-voice-command-examples)
- [🔄 Automation Examples](#-automation-examples)

## 📝 Documentation Examples [⬆️](#-table-of-contents)

### 📚 Creating Project Documentation [⬆️](#-table-of-contents)

This example demonstrates how to create comprehensive documentation for a project.

```bash
# Create repository for documentation
gh repo create my-project-docs --private --description "Documentation for My Project"

# Clone repository
gh repo clone my-project-docs
cd my-project-docs

# Set up documentation structure
mkdir -p docs/{overview,installation,usage,api,examples,reference}

# Create main README
cat > README.md << 'EOF'
# My Project Documentation

## Overview

This repository contains comprehensive documentation for My Project.

## Table of Contents

- [Overview](docs/overview/README.md)
- [Installation Guide](docs/installation/README.md)
- [Usage Guide](docs/usage/README.md)
- [API Documentation](docs/api/README.md)
- [Examples](docs/examples/README.md)
- [Reference](docs/reference/README.md)
EOF

# Create overview documentation
cat > docs/overview/README.md << 'EOF'
# Overview

## Introduction

My Project is a comprehensive solution for [specific problem domain].

## Features

- Feature 1: Description
- Feature 2: Description
- Feature 3: Description

## Architecture

My Project follows a [architectural pattern] architecture with the following components:

- Component 1: Description
- Component 2: Description
- Component 3: Description

## Technologies

My Project uses the following technologies:

- Technology 1: Description
- Technology 2: Description
- Technology 3: Description
EOF

# Commit and push documentation
git add .
git commit -m "Initial documentation structure"
git push
```

### ✅ Documentation Validation Example [⬆️](#-table-of-contents)

This example demonstrates how to validate documentation quality and consistency.

```bash
# Create validation script
mkdir -p scripts
cat > scripts/validate-docs.sh << 'EOF'
#!/bin/bash

echo "Validating documentation..."

# Check for required sections
required_sections=("overview" "installation" "usage" "api" "examples" "reference")
for section in "${required_sections[@]}"; do
  if [ ! -d "docs/$section" ] || [ ! -f "docs/$section/README.md" ]; then
    echo "Error: Missing $section documentation"
    exit 1
  fi
done

# Check for broken links
echo "Checking for broken links..."
find docs -name "*.md" -exec markdown-link-check {} \;

# Check for TODOs
echo "Checking for TODOs..."
todos=$(grep -r "TODO" docs || true)
if [ ! -z "$todos" ]; then
  echo "Warning: TODOs found in documentation:"
  echo "$todos"
fi

# Check for minimum content length
echo "Checking for minimum content length..."
for file in $(find docs -name "*.md"); do
  lines=$(wc -l < "$file")
  if [ "$lines" -lt 10 ]; then
    echo "Warning: $file has less than 10 lines of content"
  fi
done

echo "Documentation validation completed"
EOF

chmod +x scripts/validate-docs.sh

# Create GitHub Actions workflow for validation
mkdir -p .github/workflows
cat > .github/workflows/validate-docs.yml << 'EOF'
name: Validate Documentation

on:
  push:
    paths:
      - 'docs/**'
  pull_request:
    paths:
      - 'docs/**'

jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Set up Node.js
        uses: actions/setup-node@v2
        with:
          node-version: '14'
      - name: Install dependencies
        run: npm install -g markdown-link-check
      - name: Validate documentation
        run: ./scripts/validate-docs.sh
EOF

# Commit and push validation
git add scripts .github
git commit -m "Add documentation validation"
git push
```

## 📊 Project Management Examples [⬆️](#-table-of-contents)

### 🏗️ Setting Up a New Project [⬆️](#-table-of-contents)

This example demonstrates how to set up a new project with GitHub project management.

```bash
# Create repository
gh repo create my-new-project --private --description "My new project"

# Clone repository
gh repo clone my-new-project
cd my-new-project

# Create project structure
mkdir -p src tests docs

# Create README
cat > README.md << 'EOF'
# My New Project

## Overview

This is a new project that [describe project purpose].

## Installation

```bash
# Installation instructions
```

## Usage

```bash
# Usage instructions
```

## Development

```bash
# Development instructions
```

## License

MIT
EOF

# Create .gitignore
cat > .gitignore << 'EOF'
# Dependencies
node_modules/
vendor/

# Build output
dist/
build/

# Environment variables
.env
.env.local
.env.*.local

# Logs
*.log
npm-debug.log*
yarn-debug.log*
yarn-error.log*

# Editor directories and files
.idea/
.vscode/
*.suo
*.ntvs*
*.njsproj
*.sln
*.sw?
EOF

# Initialize Git
git add .
git commit -m "Initial project setup"
git push

# Create project board
gh api repos/$(gh repo view --json owner,name -q '.owner.login+"/"+.name')/projects -F name="Project Board" -F body="Main project board"

# Create milestones
gh api repos/$(gh repo view --json owner,name -q '.owner.login+"/"+.name')/milestones -F title="v0.1" -F description="Initial release"
gh api repos/$(gh repo view --json owner,name -q '.owner.login+"/"+.name')/milestones -F title="v0.2" -F description="Feature enhancements"

# Create initial issues
gh issue create --title "Set up CI/CD" --body "Configure GitHub Actions for CI/CD"
gh issue create --title "Create initial documentation" --body "Write project documentation"
gh issue create --title "Implement core functionality" --body "Implement the core features"
```

### 📈 Sprint Planning Example [⬆️](#-table-of-contents)

This example demonstrates how to plan and manage a development sprint.

```bash
# Create sprint milestone
gh api repos/$(gh repo view --json owner,name -q '.owner.login+"/"+.name')/milestones -F title="Sprint 1" -F description="First sprint" -F due_on="2025-07-15T00:00:00Z"

# Get milestone ID
milestone_id=$(gh api repos/$(gh repo view --json owner,name -q '.owner.login+"/"+.name')/milestones --jq '.[0].number')

# Create sprint issues
gh issue create --title "Feature: User Authentication" --body "Implement user authentication system"
gh issue create --title "Feature: Dashboard UI" --body "Create dashboard user interface"
gh issue create --title "Feature: Data Import" --body "Implement data import functionality"

# Assign issues to milestone
for issue in $(gh issue list --json number --jq '.[].number'); do
  gh issue edit $issue --milestone $milestone_id
done

# Assign issues to team members
gh issue edit 1 --add-assignee teammate1
gh issue edit 2 --add-assignee teammate2
gh issue edit 3 --add-assignee teammate3

# Create sprint planning document
cat > sprint-1-planning.md << 'EOF'
# Sprint 1 Planning

## Sprint Goal

The goal of Sprint 1 is to implement the core functionality of the application.

## Sprint Backlog

1. User Authentication
   - Implement login/registration
   - Set up JWT authentication
   - Create user profiles

2. Dashboard UI
   - Design dashboard layout
   - Implement navigation
   - Create widget components

3. Data Import
   - Create import interface
   - Implement file parsing
   - Add validation

## Team Capacity

- Teammate1: 40 hours
- Teammate2: 40 hours
- Teammate3: 40 hours

## Sprint Schedule

- Start Date: 2025-07-01
- End Date: 2025-07-15
- Daily Standup: 9:30 AM
- Sprint Review: 2025-07-15 2:00 PM
EOF

# Commit and push sprint planning
git add sprint-1-planning.md
git commit -m "Add Sprint 1 planning document"
git push
```

## 👨‍💻 Development Workflow Examples [⬆️](#-table-of-contents)

### ✨ Feature Development Example [⬆️](#-table-of-contents)

This example demonstrates the workflow for developing a new feature.

```bash
# Create feature branch
git checkout -b feature/user-authentication

# Implement feature
mkdir -p src/auth
cat > src/auth/index.js << 'EOF'
// Authentication module

const authenticate = (username, password) => {
  // Implementation
  return { success: true, token: 'sample-token' };
};

const register = (username, password, email) => {
  // Implementation
  return { success: true, userId: 'user-123' };
};

module.exports = {
  authenticate,
  register
};
EOF

# Create tests
mkdir -p tests/auth
cat > tests/auth/auth.test.js << 'EOF'
const auth = require('../../src/auth');

describe('Authentication', () => {
  test('authenticate should return success and token', () => {
    const result = auth.authenticate('user', 'password');
    expect(result.success).toBe(true);
    expect(result.token).toBeDefined();
  });

  test('register should return success and userId', () => {
    const result = auth.register('user', 'password', 'user@example.com');
    expect(result.success).toBe(true);
    expect(result.userId).toBeDefined();
  });
});
EOF

# Update README with authentication documentation
cat >> README.md << 'EOF'

## Authentication

### Login

```javascript
const auth = require('./src/auth');
const result = auth.authenticate('username', 'password');
// result: { success: true, token: 'jwt-token' }
```

### Registration

```javascript
const auth = require('./src/auth');
const result = auth.register('username', 'password', 'email@example.com');
// result: { success: true, userId: 'user-id' }
```
EOF

# Commit changes
git add .
git commit -m "Implement user authentication feature"

# Push feature branch
git push -u origin feature/user-authentication

# Create pull request
gh pr create --title "Feature: User Authentication" --body "Implements user authentication system with login and registration functionality."

# Request reviews
gh pr edit --add-reviewer teammate1,teammate2

# Merge pull request (after reviews)
gh pr merge --squash
```

### 🐛 Bug Fix Example [⬆️](#-table-of-contents)

This example demonstrates the workflow for fixing a bug.

```bash
# Create bug fix branch
git checkout -b fix/authentication-error

# Fix the bug
cat > src/auth/index.js << 'EOF'
// Authentication module

const authenticate = (username, password) => {
  // Fixed implementation
  if (!username || !password) {
    return { success: false, error: 'Missing credentials' };
  }
  return { success: true, token: 'sample-token' };
};

const register = (username, password, email) => {
  // Fixed implementation
  if (!username || !password || !email) {
    return { success: false, error: 'Missing required fields' };
  }
  return { success: true, userId: 'user-123' };
};

module.exports = {
  authenticate,
  register
};
EOF

# Update tests
cat > tests/auth/auth.test.js << 'EOF'
const auth = require('../../src/auth');

describe('Authentication', () => {
  test('authenticate should return success and token with valid credentials', () => {
    const result = auth.authenticate('user', 'password');
    expect(result.success).toBe(true);
    expect(result.token).toBeDefined();
  });

  test('authenticate should return error with missing credentials', () => {
    const result = auth.authenticate('', '');
    expect(result.success).toBe(false);
    expect(result.error).toBe('Missing credentials');
  });

  test('register should return success and userId with valid data', () => {
    const result = auth.register('user', 'password', 'user@example.com');
    expect(result.success).toBe(true);
    expect(result.userId).toBeDefined();
  });

  test('register should return error with missing data', () => {
    const result = auth.register('', '', '');
    expect(result.success).toBe(false);
    expect(result.error).toBe('Missing required fields');
  });
});
EOF

# Commit changes
git add .
git commit -m "Fix authentication error with missing credentials"

# Push bug fix branch
git push -u origin fix/authentication-error

# Create pull request
gh pr create --title "Fix: Authentication Error" --body "Fixes authentication error when credentials are missing. Resolves #123."

# Request reviews
gh pr edit --add-reviewer teammate1,teammate2

# Merge pull request (after reviews)
gh pr merge --squash
```

### 🚀 Release Example [⬆️](#-table-of-contents)

This example demonstrates how to create and manage a release.

```bash
# Create release branch
git checkout -b release/v1.0.0

# Update version numbers
# ...

# Update CHANGELOG
cat > CHANGELOG.md << 'EOF'
# Changelog

## v1.0.0 (2023-12-31)

### Features
- User authentication
- User interface
- Database integration

### Bug Fixes
- Fixed login error
- Fixed UI rendering issues
EOF

# Commit changes
git add .
git commit -m "Prepare for v1.0.0 release"
git push -u origin release/v1.0.0

# Create pull request
gh pr create --title "Release v1.0.0" --body "Prepares for v1.0.0 release" --base main

# Merge pull request
gh pr merge --squash

# Create release
gh release create v1.0.0 --title "v1.0.0" --notes "First stable release" --target main
```

## 🎤 Voice Command Examples [⬆️](#-table-of-contents)

### 🗣️ Basic Voice Commands [⬆️](#-table-of-contents)

This example demonstrates basic voice commands for GitHub operations.

```bash
# Create voice command configuration
cat > voice-commands.json << 'EOF'
{
  "commands": {
    "create repository": "gh repo create",
    "list repositories": "gh repo list",
    "create issue": "gh issue create",
    "list issues": "gh issue list",
    "create pull request": "gh pr create",
    "list pull requests": "gh pr list"
  }
}
EOF

# Set up voice recognition script
cat > voice-github.py << 'EOF'
import speech_recognition as sr
import json
import subprocess
import os

# Load commands
with open('voice-commands.json', 'r') as f:
    config = json.load(f)
    commands = config['commands']

# Initialize recognizer
recognizer = sr.Recognizer()

def execute_command(text):
    for key, cmd in commands.items():
        if key.lower() in text.lower():
            print(f"Executing: {cmd}")
            subprocess.run(cmd, shell=True)
            return True
    print("Command not recognized")
    return False

# Main loop
def main():
    print("Listening for GitHub commands...")
    with sr.Microphone() as source:
        recognizer.adjust_for_ambient_noise(source)
        while True:
            try:
                audio = recognizer.listen(source)
                text = recognizer.recognize_google(audio)
                print(f"Recognized: {text}")
                execute_command(text)
            except sr.UnknownValueError:
                print("Could not understand audio")
            except sr.RequestError:
                print("Could not request results")

if __name__ == "__main__":
    main()
EOF

# Install dependencies
pip install SpeechRecognition pyaudio

# Run voice command script
python voice-github.py
```

### 🔊 Advanced Voice Commands [⬆️](#-table-of-contents)

This example demonstrates advanced voice commands with parameters.

```bash
# Create advanced voice command configuration
cat > advanced-voice-commands.json << 'EOF'
{
  "commands": {
    "create repository (.+)": {
      "command": "gh repo create {0}",
      "description": "Creates a new repository with the specified name"
    },
    "create issue (.+) in (.+)": {
      "command": "gh issue create --title \"{0}\" --repo {1}",
      "description": "Creates a new issue with the specified title in the specified repository"
    },
    "assign issue (\\d+) to (.+)": {
      "command": "gh issue edit {0} --add-assignee {1}",
      "description": "Assigns the specified issue to the specified user"
    },
    "merge pull request (\\d+)": {
      "command": "gh pr merge {0} --squash",
      "description": "Merges the specified pull request using squash"
    }
  }
}
EOF

# Set up advanced voice recognition script
cat > advanced-voice-github.py << 'EOF'
import speech_recognition as sr
import json
import subprocess
import re
import os

# Load commands
with open('advanced-voice-commands.json', 'r') as f:
    config = json.load(f)
    commands = config['commands']

# Initialize recognizer
recognizer = sr.Recognizer()

def execute_command(text):
    for pattern, cmd_info in commands.items():
        match = re.search(pattern, text, re.IGNORECASE)
        if match:
            cmd_template = cmd_info['command']
            # Replace placeholders with captured groups
            for i, group in enumerate(match.groups()):
                cmd_template = cmd_template.replace(f"{{{i}}}", group)
            
            print(f"Executing: {cmd_template}")
            subprocess.run(cmd_template, shell=True)
            return True
    
    print("Command not recognized")
    return False

# Main loop
def main():
    print("Listening for advanced GitHub commands...")
    with sr.Microphone() as source:
        recognizer.adjust_for_ambient_noise(source)
        while True:
            try:
                audio = recognizer.listen(source)
                text = recognizer.recognize_google(audio)
                print(f"Recognized: {text}")
                execute_command(text)
            except sr.UnknownValueError:
                print("Could not understand audio")
            except sr.RequestError:
                print("Could not request results")

if __name__ == "__main__":
    main()
EOF

# Run advanced voice command script
python advanced-voice-github.py
```

## 🔄 Automation Examples [⬆️](#-table-of-contents)

### 🤖 CI/CD Pipeline Example [⬆️](#-table-of-contents)

This example demonstrates setting up a CI/CD pipeline for a project.

```bash
# Create GitHub Actions workflow for CI/CD
mkdir -p .github/workflows

# Create CI workflow
cat > .github/workflows/ci.yml << 'EOF'
name: CI

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main, develop ]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Set up Node.js
        uses: actions/setup-node@v2
        with:
          node-version: '16'
      - name: Install dependencies
        run: npm ci
      - name: Run tests
        run: npm test
      - name: Run linter
        run: npm run lint
EOF

# Create CD workflow
cat > .github/workflows/cd.yml << 'EOF'
name: CD

on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Set up Node.js
        uses: actions/setup-node@v2
        with:
          node-version: '16'
      - name: Install dependencies
        run: npm ci
      - name: Build
        run: npm run build
      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./build
EOF

# Commit and push workflows
git add .github
git commit -m "Add CI/CD workflows"
git push
```

### 📊 Automated Reporting Example [⬆️](#-table-of-contents)

This example demonstrates setting up automated reporting for a project.

```bash
# Create reporting script
mkdir -p scripts
cat > scripts/generate-report.js << 'EOF'
#!/usr/bin/env node

const fs = require('fs');
const path = require('path');
const { execSync } = require('child_process');

// Get date
const date = new Date();
const dateString = date.toISOString().split('T')[0];

// Create report directory
const reportDir = path.join('reports', dateString);
fs.mkdirSync(reportDir, { recursive: true });

// Generate issue report
const issues = JSON.parse(execSync('gh issue list --state all --json number,title,state,createdAt,closedAt,assignees').toString());
fs.writeFileSync(path.join(reportDir, 'issues.json'), JSON.stringify(issues, null, 2));

// Generate PR report
const prs = JSON.parse(execSync('gh pr list --state all --json number,title,state,createdAt,closedAt,reviewers').toString());
fs.writeFileSync(path.join(reportDir, 'prs.json'), JSON.stringify(prs, null, 2));

// Generate commit report
const commits = JSON.parse(execSync('gh api repos/owner/repo/commits --paginate').toString());
fs.writeFileSync(path.join(reportDir, 'commits.json'), JSON.stringify(commits, null, 2));

// Generate HTML report
const html = `
<!DOCTYPE html>
<html>
<head>
  <title>Project Report - ${dateString}</title>
  <style>
    body { font-family: Arial, sans-serif; margin: 20px; }
    h1 { color: #333; }
    table { border-collapse: collapse; width: 100%; }
    th, td { border: 1px solid #ddd; padding: 8px; }
    tr:nth-child(even) { background-color: #f2f2f2; }
    th { padding-top: 12px; padding-bottom: 12px; text-align: left; background-color: #4CAF50; color: white; }
  </style>
</head>
<body>
  <h1>Project Report - ${dateString}</h1>
  
  <h2>Issues</h2>
  <table>
    <tr>
      <th>Number</th>
      <th>Title</th>
      <th>State</th>
      <th>Created</th>
      <th>Closed</th>
    </tr>
    ${issues.map(issue => `
      <tr>
        <td>${issue.number}</td>
        <td>${issue.title}</td>
        <td>${issue.state}</td>
        <td>${new Date(issue.createdAt).toLocaleDateString()}</td>
        <td>${issue.closedAt ? new Date(issue.closedAt).toLocaleDateString() : 'N/A'}</td>
      </tr>
    `).join('')}
  </table>
  
  <h2>Pull Requests</h2>
  <table>
    <tr>
      <th>Number</th>
      <th>Title</th>
      <th>State</th>
      <th>Created</th>
      <th>Closed</th>
    </tr>
    ${prs.map(pr => `
      <tr>
        <td>${pr.number}</td>
        <td>${pr.title}</td>
        <td>${pr.state}</td>
        <td>${new Date(pr.createdAt).toLocaleDateString()}</td>
        <td>${pr.closedAt ? new Date(pr.closedAt).toLocaleDateString() : 'N/A'}</td>
      </tr>
    `).join('')}
  </table>
</body>
</html>
`;

fs.writeFileSync(path.join(reportDir, 'report.html'), html);

console.log(`Report generated in ${reportDir}`);
EOF

chmod +x scripts/generate-report.js

# Create GitHub Actions workflow for scheduled reporting
cat > .github/workflows/reporting.yml << 'EOF'
name: Automated Reporting

on:
  schedule:
    - cron: '0 0 * * 0'  # Run every Sunday at midnight

jobs:
  generate-report:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Set up Node.js
        uses: actions/setup-node@v2
        with:
          node-version: '16'
      - name: Install dependencies
        run: npm ci
      - name: Generate report
        run: node scripts/generate-report.js
      - name: Commit and push report
        run: |
          git config --global user.name 'GitHub Actions'
          git config --global user.email 'actions@github.com'
          git add reports
          git commit -m "Add automated report"
          git push
EOF

# Commit and push reporting setup
git add scripts .github
git commit -m "Add automated reporting"
git push
```

---

Made with Power, Love, and AI • ⚡️❤️🤖 •  POWERBRIDGE.AI 