# Prerequisites

This skill requires Python dependencies to be installed before running any scripts.

## Environment Setup

Before executing any script in this skill, run:

```bash
# Navigate to the skill directory
cd $SKILL_DIR

# Create and activate Python virtual environment
python3 -m venv .venv
source .venv/bin/activate

# Install Python dependencies
pip install pyyaml requests
```

## Dependencies

### Python

| Package | Required | Purpose |
|---------|----------|---------|
| `pyyaml` | Yes | Parse YAML config files (`research_interests.yaml`) and frontmatter |
| `requests` | Optional | Semantic Scholar API calls (fallback to `urllib` if not installed) |

**Standard Library Modules** (no installation needed):
- `xml.etree.ElementTree` - Parse arXiv API XML responses
- `urllib.request` - HTTP requests (fallback for Semantic Scholar)
- `json`, `re`, `os`, `sys`, `time`, `logging`, `datetime`, `pathlib`

## Running Scripts

After environment setup, activate the virtual environment before running any Python scripts:

```bash
source $SKILL_DIR/.venv/bin/activate

# Scan existing notes to build keyword index
python3 scripts/scan_existing_notes.py \
  --vault "/path/to/vault" \
  --output existing_notes_index.json

# Search and filter arXiv papers
python3 scripts/search_arxiv.py \
  --config "$OBSIDIAN_VAULT_PATH/99_System/Config/research_interests.yaml" \
  --output arxiv_filtered.json \
  --max-results 200 \
  --top-n 10 \
  --categories "cs.AI,cs.LG,cs.CL,cs.CV,cs.MM,cs.MA,cs.RO"

# Link keywords in generated notes
python3 scripts/link_keywords.py \
  --index existing_notes_index.json \
  --input "10_Daily/2026-03-30论文推荐.md" \
  --output "10_Daily/2026-03-30论文推荐_linked.md"
```

## Quick Start (One-time Setup)

```bash
# Setup virtual environment and install dependencies
cd $SKILL_DIR
python3 -m venv .venv
source .venv/bin/activate
pip install pyyaml requests

# Verify installation
python3 -c "import yaml; import requests; print('Dependencies installed successfully')"
```

## Environment Variables

The skill uses `OBSIDIAN_VAULT_PATH` environment variable to locate the vault:

```bash
# Set in ~/.zshrc or ~/.bash_profile
export OBSIDIAN_VAULT_PATH="/path/to/vault"

# Or pass directly to scripts
python3 scripts/scan_existing_notes.py --vault "/path/to/vault"
```
