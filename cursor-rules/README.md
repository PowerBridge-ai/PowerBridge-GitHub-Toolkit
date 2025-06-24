# 🧩 Cursor Rules

> Custom rule files for enhancing Cursor IDE with GitHub integration, documentation workflows, and development tools.

## 📋 Table of Contents

- [🔍 Overview](#-overview)
- [📄 Rule Files](#-rule-files)
- [🛠️ Installation](#-installation)
- [🚀 Usage](#-usage)
- [⚙️ Configuration](#-configuration)
- [📚 Examples](#-examples)

## 🔍 Overview [⬆️](#-table-of-contents)

The PowerBridge GitHub Toolkit includes a set of custom rule files for Cursor IDE that enhance your development workflow. These rules enable advanced functionality like voice-controlled GitHub operations, automated documentation workflows, structured debugging processes, and checkpoint systems for saving work states.

Each rule file is designed to be modular and can be used independently or in combination with other rules to create a customized development environment tailored to your needs.

## 📄 Rule Files [⬆️](#-table-of-contents)

### 📝 [AI Documentation Workflow](./ai-documentation-workflow.cursor) [⬆️](#-table-of-contents)

This rule file automates documentation creation and maintenance tasks:

- **Automated Documentation Generation** - Create documentation from code comments
- **Documentation Validation** - Check for completeness and consistency
- **Documentation Templates** - Apply standardized templates
- **Cross-Reference Management** - Maintain links between related documents

### 🎨 [AI Documentation Formatting](./ai-documentation-formatting.cursor) [⬆️](#-table-of-contents)

This rule file standardizes document formatting with consistent styles:

- **Markdown Standardization** - Apply consistent markdown formatting
- **Emoji Integration** - Add appropriate emojis to headings and sections
- **Table of Contents Generation** - Create and update table of contents
- **Back-to-Top Links** - Add navigation links throughout documents

### 🐛 [AI Debugging Workflow](./ai-debugging-workflow.cursor) [⬆️](#-table-of-contents)

This rule file implements structured debugging with emoji-based logging:

- **Emoji Logging System** - Use emojis for log categorization
- **Debug Session Management** - Start, pause, and resume debug sessions
- **Error Pattern Recognition** - Identify common error patterns
- **Solution Suggestions** - Provide potential fixes for identified issues

### 🔖 [AI Checkpoint System](./ai-checkpoint-system.cursor) [⬆️](#-table-of-contents)

This rule file creates a checkpoint system for saving and restoring work states:

- **Work State Snapshots** - Save the current state of your work
- **Checkpoint Navigation** - Move between saved checkpoints
- **Branch Management** - Create branches from checkpoints
- **Checkpoint Comparison** - Compare changes between checkpoints

### 🔄 [GitHub Automation Workflow](./github-automation-workflow.cursor) [⬆️](#-table-of-contents)

This rule file automates GitHub operations via voice commands:

- **Voice Command Integration** - Control GitHub with voice commands
- **Repository Management** - Create, clone, and manage repositories
- **Issue and PR Workflows** - Manage issues and pull requests
- **Project Management** - Track projects and milestones

## 🛠️ Installation [⬆️](#-table-of-contents)

To install the Cursor rules:

1. **Clone the Repository**:
   ```bash
   git clone https://github.com/powerbridge/github-toolkit.git
   cd github-toolkit
   ```

2. **Copy Rule Files**:
   ```bash
   # For Windows
   copy cursor-rules\*.cursor %USERPROFILE%\.cursor\rules\

   # For macOS/Linux
   cp cursor-rules/*.cursor ~/.cursor/rules/
   ```

3. **Verify Installation**:
   Open Cursor IDE and check that the rules are available in the rules menu.

## 🚀 Usage [⬆️](#-table-of-contents)

### Loading Rules in Cursor IDE [⬆️](#-table-of-contents)

1. Open Cursor IDE
2. Press `Ctrl+Shift+P` (Windows/Linux) or `Cmd+Shift+P` (macOS) to open the command palette
3. Type "Load Rule" and select the rule you want to load
4. The rule will be activated and ready to use

### Voice Commands [⬆️](#-table-of-contents)

To use voice commands with the GitHub Automation Workflow:

1. Load the GitHub Automation Workflow rule
2. Press `Alt+V` (Windows/Linux) or `Option+V` (macOS) to activate voice recognition
3. Speak a command like "Create repository my-project" or "List pull requests"
4. The command will be executed through GitHub CLI or MCP server

### Documentation Workflows [⬆️](#-table-of-contents)

To use the documentation workflows:

1. Load the AI Documentation Workflow rule
2. Press `Alt+D` (Windows/Linux) or `Option+D` (macOS) to open the documentation menu
3. Select the desired documentation action
4. Follow the prompts to complete the documentation task

## ⚙️ Configuration [⬆️](#-table-of-contents)

Each rule file can be configured by editing the configuration section at the top of the file:

```javascript
// Configuration
const config = {
  // Common settings
  enableVoiceCommands: true,
  logLevel: 'info',
  
  // Rule-specific settings
  documentationTemplatesPath: './templates',
  checkpointStoragePath: './checkpoints',
  githubToken: process.env.GITHUB_TOKEN,
};
```

You can customize these settings to match your preferences and environment.

## 📚 Examples [⬆️](#-table-of-contents)

### Documentation Workflow Example [⬆️](#-table-of-contents)

```javascript
// Load the AI Documentation Workflow rule
// Press Alt+D to open the documentation menu
// Select "Generate Documentation from Code"
// Select the file or directory to document
// The documentation will be generated and saved to the specified location
```

### GitHub Automation Example [⬆️](#-table-of-contents)

```javascript
// Load the GitHub Automation Workflow rule
// Press Alt+V to activate voice recognition
// Say "Create repository my-new-project private"
// The repository will be created on GitHub
// Say "Clone repository my-new-project"
// The repository will be cloned to your local machine
```

### Debugging Workflow Example [⬆️](#-table-of-contents)

```javascript
// Load the AI Debugging Workflow rule
// Press Alt+B to start a debug session
// The rule will add structured logging statements to your code
// Run your code to see the enhanced logging output
// Press Alt+B again to analyze the logs and get suggestions
```

---

Made with Power, Love, and AI • ⚡️❤️🤖 •  POWERBRIDGE.AI 