# Greybeard

**Greybeard** is a definitive, battle-tested set of core behavioral rules designed to elevate any AI coding assistant into a pragmatic, senior-level autonomous engineering agent.

Rather than focusing on syntax or framework-specific knowledge, Greybeard defines the **"how"**—the decision priorities, problem-solving methodologies, and strict behavioral guardrails that prevent common AI pitfalls like over-engineering, hallucination, and unnecessary code churn.

## 🌟 Why Greybeard?

Most AI coding assistants fail not because they lack knowledge, but because they lack discipline. They often rewrite working code, guess APIs instead of reading documentation, and prioritize complex, abstract solutions over simple, maintainable ones. Greybeard fixes this by acting as a strict behavioral framework that prioritizes user intent, verification, and simplicity.

## 📖 Rules vs. Skills: What's the Difference?

To use Greybeard effectively, it's crucial to understand the distinction between **Rules** and **Skills** in the context of AI assistants:

*   **Rules (The "How"):** These are absolute behavioral constraints and guiding principles. They dictate *how* the AI should think, prioritize, and communicate. Rules are universal across all languages and frameworks. 
    *   *Example:* "Never guess. Always verify before proposing a fix." or "Prefer evidence over assumptions." 
    *   **Greybeard now includes both the Ultimate Rules (RULES.md) and a curated `skills/` directory of 24 Elite Core Skills.**
*   **Skills (The "What"):** These are task-specific, contextual instructions. They dictate *what* steps the AI should take to accomplish a specific technical goal. 
    *   *Example:* "How to deploy this Next.js app to Vercel" or "The standard way to write a React component in this repository."

**The Golden Workflow:** You inject Greybeard's `RULES.md` and copy the `skills/` directory into your agent's config folder. This creates an unshakeable, disciplined AI foundation. You can then add your own project-specific Skills on top of this.

## 🚀 Usage

Greybeard is designed to be platform-agnostic. You can inject these rules into the system prompt or project-level configuration of your favorite AI coding tool.

### Cursor
Create a `.cursorrules` file in the root of your workspace and copy the contents of `RULES.md` into it.

### Windsurf
Create a `.windsurfrules` file in the root of your workspace and copy the contents of `RULES.md` into it.

### Claude Code / Google Antigravity / Custom CLI Agents
Inject the contents of `RULES.md` directly into your agent's system prompt or global instruction file (e.g., `AGENTS.md` for Antigravity).

### GitHub Copilot / ChatGPT / Claude Web
Paste the contents of `RULES.md` into the "Custom Instructions" / "System Instructions" settings, or simply paste it at the beginning of a complex conversation to set the operational baseline.

## 📜 The Rules

Read the full set of rules in [RULES.md](./RULES.md).

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](./LICENSE) file for details.