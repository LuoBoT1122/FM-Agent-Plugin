---
name: FM-Agent Help
description: Use when the user asks to "fm-agent help", "how to use fm-agent", "fm-agent usage", "fm-agent commands", or needs information about FM-Agent plugin capabilities.
version: 0.1.0
---

Provide help and usage information for the FM-Agent plugin.

## Overview

This plugin integrates FM-Agent into Claude Code for automated code reasoning and bug detection. FM-Agent uses LLM-based Hoare-style verification to analyze codebases.

## Available Commands

| Command | Description |
|---------|-------------|
| `/fm-agent:install` | Clone FM-Agent to the plugin data directory |
| `/fm-agent:config` | Show/modify FM-Agent configuration |
| `/fm-agent:run` | Execute FM-Agent analysis on current project (background) |
| `/fm-agent:diagnose` | View bug analysis results |
| `/fm-agent:help` | Show help information |

## install

Clone FM-Agent from official repository to the plugin data directory (`${CLAUDE_PLUGIN_DATA}/FM-Agent/`):
```bash
git clone -b opensource https://ipads.se.sjtu.edu.cn:1312/ipads-storage/codebase/ai4s/logic.git "${CLAUDE_PLUGIN_DATA}/FM-Agent"
```
Then run `${CLAUDE_PLUGIN_DATA}/FM-Agent/install.sh` to install dependencies.

## config

Check if `OPENROUTER_API_KEY` env var is set; if not, prompts user for the key and writes it to `${CLAUDE_PLUGIN_DATA}/.env`

Read config file `${CLAUDE_PLUGIN_DATA}/FM-Agent/config.py`, list configuration and let user select which to modify

**Configurable settings**:
- `LLM_OPENROUTER_API_BASE_URL` - OpenRouter API endpoint
- `LLM_MODEL` - LLM model via OpenRouter (default: anthropic/claude-sonnet-4.6)
- `MAX_SPC_ITER` - Maximum specification iterations (default: 5)
- `GRANULARITY` - Analysis granularity (default: 40)
- `MAX_WORKERS` - Maximum concurrent workers (default: 10)
- `OPENCODE_MAX_RETRIES` - Maximum retry attempts (default: 5)

## run

Execute FM-Agent from plugin data directory to analyze the current project directory (`./`):
- Run as background task to avoid blocking
- Check for existing `fm_agent/` in project directory and uses `--resume` flag if present
- Notify user when analysis completes

## diagnose

Read FM-Agent output from `./fm_agent/`:
- **Summary first**: Show `bug_validation/summary.json` with totals
- **Details on request**: Show individual bug reports (`<source>--<function>.md`)
- Bug reports include: specification claim, actual behavior, code evidence, trigger condition, probe script, probe output

## Prerequisites

- Ubuntu 22.04+ or compatible Linux distribution
- Python 3.12+

## Output Directory

Analysis results in `./fm_agent/`:
- `bug_validation/summary.json` - Aggregated summary
- `bug_validation/<source>--<function>.md` - Bug detail reports
- `bug_validation/<source>--<function>.result.json` - Machine-readable results

## Supported Languages

Rust, C, C++, Python, Java, Go, CUDA, JavaScript, TypeScript, ArkTS