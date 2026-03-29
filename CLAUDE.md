# CLAUDE.md — Maths, CS & AI Compendium

This file gives Claude Code and Claude Desktop context about this repository so that AI assistance is fast and accurate.

## Project Overview

**Maths, CS & AI Compendium** is an open, intuition-first textbook covering mathematics, computer science, and artificial intelligence from the ground up. It is written for curious practitioners — AI/ML engineers, researchers, and students — who want deep understanding rather than surface familiarity.

- **Site**: <https://henryndubuaku.github.io/maths-cs-ai-compendium/>
- **Stack**: [MkDocs](https://www.mkdocs.org/) + [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/) + MathJax
- **License**: Open source educational resource

## Repository Layout

```
maths-cs-ai-compendium/
├── chapter 01: vectors/          # Chapter directories (source content)
│   ├── 01. vector spaces.md
│   └── ...
├── chapter 02: matrices/
│   └── ...
│   ...                           # Chapters 01–20 follow the same pattern
├── chapter 20: bleeding edge AI/
├── images/                       # SVG diagrams (referenced from chapters)
├── javascripts/mathjax.js        # MathJax configuration
├── mkdocs.yml                    # Site configuration and navigation
├── llms.txt                      # AI-readable overview of all chapters
├── CLAUDE.md                     # This file — Claude Code/Desktop context
├── README.md                     # Human-readable project description
├── .github/workflows/deploy-docs.yml
└── .gitignore
```

> **Note**: the `docs/` directory is generated at CI build time (via symlinks to the chapter directories) and is `.gitignore`d. Do not edit it directly.

## Chapters at a Glance

| # | Chapter | Topics |
|---|---------|--------|
| 01 | Vectors | Vector spaces, norms, dot/cross products, basis |
| 02 | Matrices | Properties, types, decompositions |
| 03 | Calculus | Differential, integral, multivariate, optimisation |
| 04 | Statistics | Distributions, hypothesis testing, inference |
| 05 | Probability | Counting, Bayes, information theory |
| 06 | Machine Learning | Classical ML, deep learning, reinforcement learning |
| 07 | Computational Linguistics | NLP, embeddings, transformers |
| 08 | Computer Vision | CNNs, detection, generation |
| 09 | Audio and Speech | DSP, ASR, TTS |
| 10 | Multimodal Learning | VLMs, cross-modal generation |
| 11 | Autonomous Systems | Perception, robot learning, VLAs |
| 12 | Graph Neural Networks | GNNs, attention, 3D graphs |
| 13 | Computing and OS | Architecture, OS, concurrency |
| 14 | Data Structures & Algorithms | Big-O, trees, graphs, sorting |
| 15 | Production Software Engineering | Linux, Git, testing, DevOps |
| 16 | SIMD and GPU Programming | CUDA, AVX, Triton, Vulkan |
| 17 | AI Inference | Quantisation, serving, edge inference |
| 18 | ML Systems Design | Infrastructure, cloud, design examples |
| 19 | Applied AI | Finance, biology, drug discovery, agents, healthcare |
| 20 | Bleeding Edge AI | Quantum ML, neuromorphic, brain-machine interfaces |

## Building and Previewing Locally

```bash
# Install dependencies
pip install "mkdocs<2" "mkdocs-material[imaging]"

# Create docs/ directory with symlinks (mirrors CI step)
mkdir -p docs
cp README.md docs/index.md
ln -s ../images docs/images
ln -s ../javascripts docs/javascripts
find . -maxdepth 1 -type d -name 'chapter *' | while IFS= read -r dir; do
  dir="${dir#./}"
  ln -s "../$dir" "docs/$dir"
done

# Serve locally with live reload
mkdocs serve

# Build static site
mkdocs build
```

The local site is available at <http://127.0.0.1:8000>.

## Content Conventions

- Every chapter file starts with a `# Heading` followed by an italic one-sentence description in `*...*`.
- Concepts are introduced as prose paragraphs or bullet lists, not raw facts.
- Code blocks use fenced triple-backticks with language identifiers.
- Inline maths uses `$...$` (LaTeX); display maths uses `$$...$$`.
- Diagrams are SVG files in `images/` and are referenced as `../images/<name>.svg`.
- Keep the tone direct and educational — explain *why*, not just *what*.

## Integration with Claude Tools

### Claude Code (`claude` CLI)

Claude Code reads this `CLAUDE.md` automatically when you open the repository. Key things Claude can help with:

- **Writing or extending chapter content**: paste existing chapter text and ask for a new section in the same style.
- **Generating SVG diagrams**: describe the concept; Claude can produce SVG markup.
- **Fixing MkDocs configuration**: the nav tree is in `mkdocs.yml`.
- **Reviewing content accuracy**: Claude can cross-check explanations against its training data.

### Claude Desktop (MCP)

To add this compendium as a resource in Claude Desktop via the [Model Context Protocol](https://modelcontextprotocol.io/):

1. Open **Claude Desktop → Settings → Developer → Edit Config** (or `~/.claude.json`).
2. Add the filesystem MCP server pointed at this repository:

```json
{
  "mcpServers": {
    "maths-cs-ai-compendium": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "/path/to/your/local/clone"
      ]
    }
  }
}
```

3. Replace `/path/to/maths-cs-ai-compendium` with the absolute path to your local clone.
4. Restart Claude Desktop — the compendium chapters will be available as context.

Once connected, you can ask Claude Desktop questions like "Explain Flash Attention using the compendium's chapter 17" and it will read the actual source files to ground its answer.

### Cowork / Team Usage

- Shared reading context: share the `llms.txt` URL with any AI tool that accepts a URL for project context.
- The site's raw Markdown files are the source of truth; link directly to them for precise citations.
- For project-wide search, use `grep -r "keyword" "chapter*"` or the MkDocs built-in search.
