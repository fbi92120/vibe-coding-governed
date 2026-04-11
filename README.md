# Vibe Coding, Governed

A method for building software with LLMs where specs come first, the human decides, and the code follows.

Vibe coding is powerful. Ungoverned vibe coding is a lottery. This repository documents a method that keeps the human in control while leveraging LLMs for both specification and implementation.

## The method

**METHODE_SPECS_CO-CONSTRUCTION.md** defines a 9-step process for co-constructing specifications with an LLM before writing any code:

1. Define the real problem (not the imagined solution)
2. Start from actual usage, not the deliverable
3. Test risky hypotheses before specifying
4. Make decisions one at a time, not in bulk
5. Write the constitution (non-negotiable rules) before code
6. Define the implementation sequence
7. Freeze specs before the first prompt
8. Execute: one prompt = one module = one test
9. Capitalize: write the experience report

The key principle: **specs eliminate ambiguity before the code encounters it.** The LLM co-constructs the specs (Claude.ai), then a different LLM session implements them (Claude Code). The human orchestrates, validates, and decides at every step.

## Supporting files

| File | Purpose |
|---|---|
| **CLAUDE.global.md** | Global instructions for Claude Code across all projects |
| **CLAUDE.projects.md** | Shared conventions for all projects in ~/Projects/ |

## Case studies

The method has been applied end-to-end on two projects:

### YT Knowledge Extractor

Stateless CLI tool: YouTube URL in, structured Markdown knowledge sheet out.

- **Architecture**: linear data flow, abstract LLM provider layer, passive orchestrator
- **Constitution**: 8 rules defined before code
- **Result**: 10 prompts, working V1

See **YT_EXTRACTOR_RETOUR_EXPERIENCE.md** for the full experience report.

### Wiki LLM

Stateful knowledge base maintained by an LLM from source files. Inspired by [Karpathy's LLM Wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f).

- **Architecture**: 7 modules, passive orchestrator, 3 ingestion modes, Obsidian as reading interface
- **Constitution**: 15 rules (stateful systems need integrity rules that stateless systems don't)
- **Result**: 12 prompts, 3533 lines of Python, 23/23 tests, 88 wiki pages produced
- **Specs/code ratio**: 1:2.7 (1286 lines of specs for 3533 lines of Python)
- **Time**: ~6h30 specs + ~3h50 coding = ~10h20 total

See **WIKI_LLM_RETOUR_EXPERIENCE.md** for the full experience report.

### The structural difference

YT Extractor is **stateless**: each run is independent. Wiki LLM is **stateful**: each ingestion modifies a persistent system. This difference drove 7 additional constitution rules, contract tests on byte-level invariants, and a validator with before/after snapshots. The method handled both — the specs surface the complexity before the code hides it.

## Repositories

- [wiki-llm](https://github.com/fbi92120/wiki-llm) — Wiki LLM source code and wiki
- [yt-knowledge-extractor](https://github.com/fbi92120/yt-knowledge-extractor) — YT Knowledge Extractor source code

## License

Not specified yet.
