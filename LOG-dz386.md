# LOG.md - Homework 1 Reflection

## 1. Technical Hurdles: The `.env` Trap
The hardest part was setting up the `.env` file on my Mac. macOS hides files starting with a dot, and my text editor secretly saved it as `.env.txt`. This broke the initial backend startup. Fortunately, I noticed the error in the logs and instructed Codex to run `mv .env.txt .env` to fix the filename before proceeding.

## 2. Agent UX: Codex vs. OpenClaw
Codex explicitly asks for permission before running risky commands (like `npm install` or opening local ports), unlike fully autonomous agents like OpenClaw. While this "human-in-the-loop" approach lowers execution speed, I find it highly commendable. For enterprise and financial software, data compliance and privacy are absolute red lines. Codex’s permission-first approach provides the necessary security for real-world development.