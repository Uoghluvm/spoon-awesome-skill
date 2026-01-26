# Contributing to Spoon Awesome Skill

Thank you for your interest in contributing! This guide will help you submit high-quality skills.

## Skill Categories

| Directory | Purpose | Target Users |
|-----------|---------|--------------|
| `spoonos-skills/` | SpoonOS agent development patterns | Developers building agents |
| `web3-skills/` | Blockchain & DeFi integrations | Web3 developers |
| `web2-skills/` | Traditional API & service integrations | Full-stack developers |
| `dev-skills/` | Development workflow & tooling | All developers |

## Directory Structure

```
spoon-awesome-skill/
├── spoonos-skills/          # SpoonOS framework skills
│   └── your-skill/
├── web3-skills/             # Blockchain integrations
│   └── your-skill/
├── web2-skills/             # Web2 API integrations
│   └── your-skill/
├── dev-skills/              # Development tools
│   └── your-skill/
├── CONTRIBUTING.md
└── README.md
```

## Skill Format

Each skill MUST follow this structure:

```
your-skill/
├── SKILL.md              # Required: Skill definition
├── README.md             # Required: Detailed documentation
├── scripts/              # Required: At least one script
│   ├── main_tool.py      # Primary tool implementation
│   └── helper.py         # Optional helper scripts
└── references/           # Optional: Additional docs
    └── api_reference.md
```

### SKILL.md (Required)

```yaml
---
name: your-skill-name
description: Brief description (used for skill triggering). Max 200 chars.
version: 1.0.0
author: Your Name <your@email.com>
tags: [tag1, tag2, tag3]
---

# Skill Name

Brief overview (2-3 sentences).

## Quick Start

[Minimal working example - under 20 lines]

## Scripts

| Script | Purpose |
|--------|---------|
| [main_tool.py](scripts/main_tool.py) | Description |

## Environment Variables

| Variable | Required | Description |
|----------|----------|-------------|
| `API_KEY` | Yes | Your API key |

## Best Practices

1. Practice 1
2. Practice 2
```

### README.md (Required)

Detailed documentation including:
- Full API documentation
- All configuration options
- Error handling
- Examples for each use case

### Scripts (Required)

At least one Python script implementing the tool. Must follow SpoonOS patterns:

```python
#!/usr/bin/env python3
"""
Tool Name - Brief description.

Author: Your Name
Version: 1.0.0
"""

from spoon_ai.tools.base import BaseTool
from pydantic import Field

class YourTool(BaseTool):
    name: str = "your_tool"
    description: str = "What this tool does"
    parameters: dict = Field(default={
        "type": "object",
        "properties": {
            "param1": {"type": "string", "description": "Description"}
        },
        "required": ["param1"]
    })

    async def execute(self, param1: str) -> str:
        # Implementation
        return "Result"
```

## Pull Request Requirements

### 1. PR Title Format

```
[category] Add skill-name skill

Examples:
[web3-skills] Add compound-lending skill
[web2-skills] Add slack-integration skill
[dev-skills] Add code-review skill
```

### 2. PR Description Template

Your PR description MUST include the following sections:

```markdown
## Skill Overview

- **Name**: your-skill-name
- **Category**: web3-skills / web2-skills / dev-skills / spoonos-skills
- **Description**: What this skill does

## Demo: Effect Demonstration

### Agent Configuration

```python
# Show the agent setup
from spoon_ai.agents import SpoonReactSkill

agent = SpoonReactSkill(
    name="demo_agent",
    skill_paths=["path/to/skills"],
    scripts_enabled=True
)
await agent.activate_skill("your-skill-name")
```

### Input Prompt

```
[The exact prompt given to the agent]

Example:
"Check the current gas prices on Ethereum and suggest optimal transaction timing"
```

### Intermediate Outputs

Show the step-by-step execution:

```
Step 1: Agent activates skill "your-skill-name"
  → Tools loaded: [tool1, tool2]

Step 2: Agent calls tool1 with params: {"chain": "ethereum"}
  → Output: {"gas_price": "25 gwei", "timestamp": "..."}

Step 3: Agent processes result and calls tool2
  → Output: {"recommendation": "..."}
```

### Final Output

```
[The final response from the agent]

Example:
"Current Ethereum gas prices:
- Base Fee: 25 gwei
- Priority Fee: 2 gwei
- Estimated Cost: $3.50 for simple transfer

Recommendation: Gas prices are moderate. Good time for non-urgent transactions."
```

### Screenshots (Required)

**You MUST include screenshots showing the running example output.**

Attach screenshots that clearly show:
1. The agent running with your skill loaded
2. The input prompt being processed
3. Tool execution and intermediate outputs
4. The final response from the agent

Example screenshot requirements:
- Terminal/console output showing execution
- Clear, readable text
- Full output visible (no truncation)

## Checklist

- [ ] SKILL.md follows the required format
- [ ] README.md includes detailed documentation
- [ ] At least one script in `scripts/` directory
- [ ] Scripts follow SpoonOS BaseTool pattern
- [ ] Environment variables documented
- [ ] **Screenshots of running example included**
- [ ] No API keys or secrets committed
```

### 3. Required Demo Evidence

Your PR MUST demonstrate the skill working with a real agent. Include:

| Requirement | Description |
|-------------|-------------|
| **Agent Used** | Which SpoonOS agent class (SpoonReactSkill, SpoonReactMCP, etc.) |
| **Input Prompt** | The exact user query |
| **Skill Activation** | How the skill was activated |
| **Tool Calls** | Which tools were called with what parameters |
| **Intermediate Results** | Step-by-step outputs from each tool |
| **Final Output** | The complete agent response |

### Example Demo Format

```
═══════════════════════════════════════════════════════════════════
                    SKILL DEMO: gas-tracker
═══════════════════════════════════════════════════════════════════

AGENT: SpoonReactSkill
MODEL: gpt-4o
SKILLS LOADED: ["gas-tracker"]

───────────────────────────────────────────────────────────────────
INPUT PROMPT:
───────────────────────────────────────────────────────────────────
"What are the current gas prices on Ethereum? Should I send my
transaction now or wait?"

───────────────────────────────────────────────────────────────────
EXECUTION TRACE:
───────────────────────────────────────────────────────────────────

[Step 1] Skill Activation
  → Activated: gas-tracker
  → Tools available: [get_gas_price, get_gas_history, estimate_cost]

[Step 2] Tool Call: get_gas_price
  → Input: {"chain": "ethereum"}
  → Output: {
      "base_fee": "23.5 gwei",
      "priority_fee": "1.5 gwei",
      "total": "25 gwei",
      "usd_per_transfer": "$2.80"
    }

[Step 3] Tool Call: get_gas_history
  → Input: {"chain": "ethereum", "hours": 24}
  → Output: {
      "avg_24h": "35 gwei",
      "min_24h": "18 gwei",
      "max_24h": "85 gwei",
      "current_percentile": 28
    }

[Step 4] Agent Reasoning
  → "Current gas (25 gwei) is below 24h average (35 gwei)"
  → "At 28th percentile - relatively cheap"

───────────────────────────────────────────────────────────────────
FINAL OUTPUT:
───────────────────────────────────────────────────────────────────

📊 **Ethereum Gas Report**

**Current Prices:**
- Base Fee: 23.5 gwei
- Priority Fee: 1.5 gwei
- Total: 25 gwei (~$2.80 for transfer)

**24h Analysis:**
- Average: 35 gwei
- Range: 18 - 85 gwei
- Current: 28th percentile (cheaper than 72% of last 24h)

**Recommendation:** ✅ Good time to transact!
Current prices are 29% below the 24h average. If your transaction
isn't urgent, you could wait for potential dips below 20 gwei
during off-peak hours (typically 2-6 AM UTC).

═══════════════════════════════════════════════════════════════════
```

## Code Quality Requirements

### Python Standards

- Python 3.10+ compatible
- Type hints required
- Docstrings for all classes and public methods
- Async/await for I/O operations
- Error handling with meaningful messages

### Security Requirements

- NO hardcoded API keys or secrets
- All sensitive data via environment variables
- Input validation on all parameters
- Safe error messages (no stack traces to users)

## Review Process

1. **Automated Checks**: CI validates structure and format
2. **Code Review**: Maintainers review code quality
3. **Demo Verification**: We verify the effect demonstration works
4. **Documentation Review**: Ensure docs are clear and complete

## Getting Help

- Open an issue for questions
- Join our Discord for discussions
- Tag maintainers in PR for urgent reviews

## License

By contributing, you agree that your contributions will be licensed under MIT License.
