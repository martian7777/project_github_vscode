# Contributing to RepoDeck

Thank you for your interest in contributing to RepoDeck! We welcome contributions of all forms, including bug reports, feature requests, documentation improvements, and code changes.

Please read through these guidelines to get started.

---

## 💬 Discussions & Community

For discussions, questions, brainstorming new features, or getting help with the extension, please use our **Discussions** page on GitHub:
👉 **[RepoDeck Discussions](https://github.com/martian7777/RepoDeck/discussions)**

We encourage you to open a discussion thread before starting work on a large feature or refactoring, as it allows us to align on design, scope, and implementation details before you write any code.

---

## 🐛 Reporting Issues

If you find a bug or have a feature request:
1. Search the [Issues](https://github.com/martian7777/RepoDeck/issues) and [Discussions](https://github.com/martian7777/RepoDeck/discussions) to check if it has already been reported.
2. If it is a new issue, open a new [Issue Report](https://github.com/martian7777/RepoDeck/issues/new).
3. Provide as much context as possible, including:
   - Your VS Code or editor fork version (e.g., Cursor, Windsurf, VSCodium).
   - Your operating system.
   - Exact steps to reproduce the issue.
   - Relevant error logs from the VS Code Developer Tools console (`Help > Toggle Developer Tools > Console`) or the RepoDeck output channel.

---

## 🛠️ Development Setup

To set up your local development environment:

### Prerequisites
- [Node.js](https://nodejs.org/) (v18+)
- [Git](https://git-scm.com/)

### Step-by-Step Setup
1. **Fork the Repository**: Fork the [RepoDeck repository](https://github.com/martian7777/RepoDeck) to your GitHub account.
2. **Clone the Fork**:
   ```bash
   git clone https://github.com/<your-username>/RepoDeck.git
   cd RepoDeck
   ```
3. **Install Dependencies**:
   ```bash
   npm install
   ```
4. **Start the Watcher**:
   ```bash
   npm run watch
   ```
   This compiles TypeScript and Preact files and watches for any changes to automatically re-compile.
5. **Debug the Extension**:
   - Open the RepoDeck directory in VS Code.
   - Press `F5` (or go to `Run and Debug` sidebar and click **Run Extension**).
   - A new *Extension Development Host* window will open with your local version of RepoDeck loaded.

---

## 🧪 Guidelines & Code Quality

To maintain high code quality:
- **TypeScript & Type Checking**: Ensure your code passes static type verification. Run `npm run typecheck` before submitting a PR.
- **Preact Frontends**: Frontend webviews are built with Preact. Ensure your components are lightweight and performant.
- **Mermaid Diagrams**: If editing visual flows in documentation, update the relevant Mermaid diagrams.
- **Commit Messages**: Write clear, descriptive commit messages.

---

## 🚀 Submitting a Pull Request

When you are ready to submit your changes:
1. Create a descriptive branch for your changes (e.g., `feature/custom-icons` or `fix/project-board-drag`).
2. Verify that there are no type check errors (`npm run typecheck`).
3. Push your branch to your forked repository.
4. Open a Pull Request from your fork back to the main RepoDeck repository.
5. Provide a clear description of the problem solved, the changes introduced, and screenshots/videos for UI changes where applicable.
