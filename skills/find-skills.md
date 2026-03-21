# Find Skills

This skill helps you discover and install skills from the open agent skills ecosystem at skills.sh.

## When to Use This Skill
- User asks "find a skill for X"
- User wants to extend Jarvis capabilities
- User asks "is there a skill for X"

## What is skills.sh?
skills.sh is an open ecosystem of SKILL.md packages that extend AI agent capabilities.
Skills are modular instruction files that give agents specialised knowledge.

## How to Find Skills
1. Browse https://skills.sh for available community skills
2. Copy the SKILL.md content from any skill page
3. Save it as a .md file in your jarvis-skills/skills/ folder on GitHub
4. Invoke it in Jarvis with: /skill <filename-without-extension>

## How to Install a Skill via CLI (on pavilion-server)
npx skills add <owner/repo> --skill <skill-name>

## Common Skill Categories
- Web Development: react, nextjs, typescript
- DevOps: docker, kubernetes, ci-cd
- Documentation: readme, changelog
- Home Automation: n8n-workflow, solar-monitor
- Productivity: git, workflow, automation
