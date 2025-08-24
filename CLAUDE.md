# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Quick Start Commands

```bash
# Install dependencies
pip install -r requirements.txt

# Run the AI coding agent
python main.py --query "your code request" --working-dir ./project

# Run the React frontend example
cd project
npm install
npm run dev
```

## Architecture Overview

This is a **meta AI coding agent** - an AI assistant that can read, edit, search, and modify code within the `project/` directory. Built with **PocketFlow**, a 100-line LLM framework.

### Core Components

- **flow.py**: Main AI agent logic with dual-agent architecture
- **main.py**: Entry point handling CLI args and flow execution
- **utils/**: File system utilities (read, write, search, delete)
- **project/**: React/TypeScript frontend (example codebase to modify)

### Agent Architecture

```
User Query → Main Decision Agent → [Tool Selection Loop] → Format Response
                     ↓
                Edit Agent (sub-flow)
                    - Read file
                    - Plan changes
                    - Apply edits
```

### Key Design Patterns

- **Agent Pattern**: Main agent decides tools, edit agent handles file modifications
- **PocketFlow**: Graph + Shared Store architecture
- **YAML for structured responses**: Avoids JSON escaping issues

### Shared Memory Structure

```python
shared = {
    "user_query": str,
    "working_dir": str,
    "history": [...],  # Actions and results
    "edit_operations": [...],  # Planned file changes
    "response": str
}
```

### Available Tools

- `read_file`: Read complete file content
- `edit_file`: AI-planned file modifications
- `grep_search`: Pattern search in files
- `list_dir`: Directory tree visualization
- `delete_file`: Remove files

### Development Notes

- All file paths are relative to `working_dir`
- Edit operations are planned by AI then applied bottom-to-top to maintain line numbers
- Uses Anthropic API for LLM calls via `utils/call_llm.py`
- React frontend uses Vite + TypeScript + Tailwind CSS