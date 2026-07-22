# Stories & Storyteller Craft Library

A modular repository for studying storytelling craft, story indexes, and deep craft dissections across multiple storytellers.

## Repository Layout (Multi-Writer Architecture)

Every writer has their own isolated namespace under `writers/` to ensure clean separation of story craft, indexes, and output files.

```
stories/
├── README.md
├── .gitignore
└── writers/
    ├── anulata_raj_nair/
    │   ├── craft/
    │   │   ├── STORY-INDEX.md                     # 110 indexed stories grouped by series/season
    │   │   ├── 2026-07-22-top-10-craft-shortlist.md   # Top 10 craft shortlist
    │   │   └── 2026-07-22-lamba-safar.md            # Deep dissect craft analysis of Lamba Safar
    │   └── logs/                                  # Session transcripts and background context
    └── <future_writer>/                            # Clean template for adding more writers
```

## How It Works

### 1. Studying Anulata Raj Nair
- **/anulata**: Run cross-story craft signature analysis.
- **/anulata <story_name>**: Scopes exclusively to a single story and evaluates all 13 craft lenses (e.g. `/anulata Gunahgaar`).
- **/anulata <lens_keyword>**: Runs a sweep across all stories for a specific lens (e.g. `/anulata pauses`, `/anulata openings`).
- **/anulata draft: <your prose>**: Critiques your draft against her craft techniques.

### 2. Adding a New Writer
- **/craft-new <Writer Full Name>**: Automatically sets up a new stub command pointing to their NotebookLM notebook and creates `writers/<writer_slug>/craft/`.

### 3. Architecture & Separation
- **Shared Battery (`~/.claude/craft-battery.md`)**: Shared analysis engine containing zero writer-specific assumptions.
- **Writer Stubs (`~/.claude/commands/<writer>.md`)**: Configures writer variables (`WRITER`, `PRONOUN`, `NOTEBOOK_MATCH`, `OUTPUT_DIR`, `QUOTE_STYLE`).
- **Writer Output (`writers/<writer_slug>/craft/`)**: All generated markdown craft files and story indexes land directly inside the respective writer's folder.
