# Unit Economics Plugin

Claude Code plugin for unit economics analysis - calculate metrics, analyze cohorts, and optimize business models.

## Features

- **Methodology Knowledge** - Deep understanding of unit economics metrics (CAC, LTV, AMPPU, payback period, retention)
- **Case Analysis** - Analyze real business cases (Flowwow marketplace, EnglishMeow EdTech, cohort data)
- **Economics Analyst** - Autonomous agent for deep analysis with access to course materials
- **Google Sheets Integration** - Create professional spreadsheets with unit economics calculations directly in Google Sheets

## Components

### Skill: unit-economics
Automatically activates when discussing:
- Unit economics metrics (CAC, LTV, AMPPU, COGS)
- Conversion analysis and optimization
- A/B testing methodology
- Cohort analysis and retention

### Agent: economics-analyst
Specialized agent for:
- Deep case analysis with CSV data
- Finding bottlenecks in sales funnels
- Channel comparison and optimization
- Cohort retention analysis

### Command: /create-table
Generate Google Sheets spreadsheets with:
- Unit economics dashboard
- Automated calculations (CAC, LTV, AMPPU)
- Cohort analysis tables
- Funnel visualization
- Channel comparison

**Setup required**: See [Google Sheets Setup Guide](unit-economics/GOOGLE_SHEETS_SETUP.md)

## Course Materials

The plugin includes comprehensive unit economics course materials:

- **materials/glossary.md** - Complete metrics glossary with formulas
- **materials/conversion.md** - Conversion optimization methodology
- **materials/ab_testing.md** - A/B testing best practices
- **materials/cases/** - Real business case studies (CSV format)

## Installation

### Install via Claude Code (Recommended)

Inside Claude Code, run:
```shell
/plugin marketplace add izmailovilya/cc_unit_economics
/plugin install unit-economics@cc_unit_economics
```

**For Google Sheets integration**, follow the [setup guide](unit-economics/GOOGLE_SHEETS_SETUP.md) to configure MCP.

### Install from GitHub Manually
```bash
git clone https://github.com/izmailovilya/cc_unit_economics.git
cc --plugin-dir ~/cc_unit_economics/unit-economics
```

### For Project-Specific Use
Clone to your project's `.claude-plugin/` directory:
```bash
cd your-project/.claude-plugin
git clone https://github.com/izmailovilya/cc_unit_economics.git
# Plugin will be available as unit-economics
```

## Usage Examples

**Ask about metrics:**
- "How to calculate CAC?"
- "What's the difference between AMPPU and LTV?"
- "Why can't we use sessions for conversion?"

**Analyze cases:**
- "Analyze the Flowwow case and find bottlenecks"
- "Compare marketing channels effectiveness"
- "Build cohort retention analysis"

**Create spreadsheets (requires Google Sheets setup):**
- `/create-table` - Generate unit economics spreadsheet
- "Create a cohort analysis table for my SaaS"
- "Build a dashboard for tracking CAC and LTV"

## Author

Created by Ilya Izmailov based on Heroes.camp unit economics course materials.

## License

MIT
