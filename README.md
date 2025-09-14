<img width="1280" height="640" alt="agent-os-og" src="https://github.com/user-attachments/assets/f70671a2-66e8-4c80-8998-d4318af55d10" />

## Your system for spec-driven agentic development.

[Agent OS](https://github.com/sanathusk/agent-os) transforms AI coding agents from confused interns into productive developers. With structured workflows that capture your standards, your stack, and the unique details of your codebase, Agent OS gives your agents the specs they need to ship quality code on the first try—not the fifth.

Use it with:

✅ Claude Code, Cursor, gemini, or any other AI coding tool.

✅ New products or established codebases.

✅ Big features, small fixes, or anything in between.

✅ Any language or framework.

---

### Documentation & Installation

Agent OS is a folder-based system that provides structured specifications for AI coding agents. No complex installation is required—it's designed to be cloned and customized for your project.

#### Base Installation (Recommended)

The base installation sets up Agent OS centrally on your system. This is optional but recommended, as it maintains your default standards and instructions that projects will inherit from.

Navigate to your home folder (or other location). The recommended location for your base installation is your system's home directory (~/.agent-os). However, you can choose another location for your base installation.

Choose your installation based on which tools you use:

**Agent OS with Gemini cli**:
```
curl -sSL https://raw.githubusercontent.com/sanathusk/agent-os/main/setup/base.sh | bash -s -- --gemini-cli
```

**Agent OS with Claude Code**:
```
curl -sSL https://raw.githubusercontent.com/sanathusk/agent-os/main/setup/base.sh | bash -s -- --claude-code
```

**Agent OS with Cursor**:
```
curl -sSL https://raw.githubusercontent.com/sanathusk/agent-os/main/setup/base.sh | bash -s -- --cursor
```

**Agent OS with Claude Code, Cursor and Gemini cli **:
```
curl -sSL https://raw.githubusercontent.com/sanathusk/agent-os/main/setup/base.sh | bash -s -- --claude-code --cursor --gemini-cli
```

**What the base installation does...**  
It creates a central Agent OS directory with default instructions, standards, and setup files. Projects can then reference or inherit from this base setup.

**Prefer manual installation?**  
Follow these steps instead:  
1. Clone the repository to your desired base location (e.g., `~/.agent-os`):  
   ```
   git clone https://github.com/sanathusk/agent-os.git ~/.agent-os
   cd ~/.agent-os
   ```  
2. Run the base setup script: `./setup/base.sh`  
3. Customize the folders as described below.

**Important**: After installation, customize your base standards in the `standards/` folder to match your preferences. These will be inherited by projects.

#### Project-Specific Setup

For individual projects, clone the repository into your project directory and customize as needed:

1. **Clone the Repository** (per project):
   ```
   git clone https://github.com/sanathusk/agent-os.git
   cd agent-os
   ```

2. **Customize the Folders**:
   - `instructions/`: Define workflows for tasks like analyzing products, creating specs, planning, and execution. Edit the `.md` files in `core/` and `meta/` to match your processes.
   - `standards/`: Set your project's coding standards. Update `tech-stack.md`, `best-practices.md`, and `code-style/` files (e.g., for JavaScript, CSS, HTML). If using base installation, copy or symlink from your base setup.
   - `commands/`: Add or modify command definitions (`.md` files) for specific actions.
   - For this fork: Set up Gemini integration by running `./setup-gemini.sh` and configuring your API key in `config.yml` if needed.

3. **Integrate with AI Tools**:
   - **Claude Code / Cursor**: Copy the relevant instruction files into your AI tool's context or prompt the AI to reference the Agent OS structure (or base installation path).
   - **Gemini CLI** (This Fork): This fork includes Gemini-specific CLI tools for running Agent OS commands.
     - **Setup**: Run `./setup-gemini.sh` to install dependencies and configure the CLI. Add your Google Gemini API key to `config.yml` (get one from [Google AI Studio](https://aistudio.google.com/app/apikey)).
     - **Available Commands** (in `gemini-cli/commands/`):
       - `analyze-product`: Analyze a product idea or existing codebase.
       - `create-spec`: Generate detailed specs based on instructions.
       - `plan-product`: Create a product plan.
       - `execute-tasks`: Run task execution workflows.
     - **Usage Example**:
       ```
       # Navigate to your project with Agent OS
       cd your-project/agent-os
       
       # Run a command (ensure Gemini CLI is in PATH or run directly)
       ./gemini-cli/analyze-product "Describe my e-commerce app"
       ```
     - For full details, see `gemini-cli/GEMINI.md`.
   - For other tools: Prompt the AI with paths to your customized instructions and standards.

#### Updating Agent OS

To update your Agent OS files:  
- For base installation: Run `git pull` in your base directory (e.g., `~/.agent-os`).  
- For project setups: Pull updates from the repo and merge with your customizations.  
- Re-run setup scripts if needed for new features.

#### Usage

- **Workflow**: Start with `plan-product.md` or `analyze-product.md` to break down your project. Then use `create-tasks.md` and `execute-tasks.md` for implementation.
- **Example Prompt for AI**: "Follow the instructions in instructions/core/create-spec.md and standards/tech-stack.md to create a spec for [your feature]."
- The system works with any language/framework—customize `standards/` to fit your stack.

#### Best Practices

- Keep instructions clear and actionable; use Markdown for structure.
- Regularly update `standards/` as your codebase evolves.
- Test workflows by running sample commands or simulating AI prompts.
- For teams: Share the repo and collaborate on customizations.

For more details, videos, and advanced guides, visit [Builder Methods Agent OS](https://buildermethods.com/agent-os).

---

### Forked of `agent-os` from Brian Casel @ Builder Methods

The project is a fork of https://github.com/buildermethods/agent-os originally created by Brian Casel,
