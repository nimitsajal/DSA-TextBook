# Algorithms: The Java Way

**A Complete Computer Science Textbook for Google-Level Software Engineering**

> *A serious, comprehensive textbook — not a cheat sheet — covering the full spectrum from algorithmic thinking through competitive-programming techniques, developed chapter by chapter.*

---

## About This Book

This textbook is designed for software engineers preparing for technical interviews at top-tier companies and for anyone who wants a rigorous, complete understanding of data structures and algorithms using Java.

The curriculum covers **18 Parts** and **73 Chapters**, progressing from foundational algorithmic thinking through advanced techniques including Dynamic Programming, Graph Algorithms, Segment Trees, and Interview Mastery.

**Implementation language:** Java (exclusively).  
**Style:** Rigorous but readable — every topic includes analogies, dry runs, complexity proofs, Java pitfalls, and interview insights.

---

## Project Structure

```
algorithms-the-java-way/
├── README.md                    ← You are here
├── INDEX.md                     ← Full 18-part, 73-chapter curriculum index
├── chapters/
│   ├── chapter-01-algorithmic-thinking.md    ✅ Complete
│   └── (future chapters added here)
├── assets/
│   ├── diagrams/                ← Mermaid source files (.mmd) for complex diagrams
│   └── images/                  ← Static images when Mermaid is insufficient
└── .gitignore
```

---

## Current Status

| Chapter | Title | Status |
|---------|-------|--------|
| Chapter 1 | Algorithmic Thinking: The Art of Problem Solving | ✅ Complete |
| Chapter 2 | Mathematical Foundations for Programmers | 🔲 Planned |
| Chapter 3 | Java Toolkit for Data Structures and Algorithms | 🔲 Planned |
| Chapter 4 | Recursion: Learning to Think Recursively | 🔲 Planned |
| Chapter 5 | Asymptotic Analysis: Measuring Algorithm Efficiency | 🔲 Planned |
| Chapters 6–73 | See [INDEX.md](INDEX.md) | 🔲 Planned |

---

## How to Read This Book

### In VS Code
Install the [Markdown Preview Enhanced](https://marketplace.visualstudio.com/items?itemName=shd101wyy.markdown-preview-enhanced) extension, which renders:
- **Mermaid diagrams** (flowcharts, sequence diagrams, decision trees)
- **LaTeX math** (`$O(n \log n)$`)
- **GitHub-style callout blocks**
- Syntax-highlighted Java code

Open any `.md` file and press `Ctrl+Shift+V` to preview.

### On GitHub
All diagrams use [Mermaid](https://mermaid.js.org/), which GitHub renders natively in `.md` files. LaTeX math is rendered via MathJax. No additional setup needed.

### In IntelliJ IDEA
Use the built-in Markdown editor. Install the [Mermaid plugin](https://plugins.jetbrains.com/plugin/20146-mermaid) for diagram rendering.

---

## Callout Conventions

Every chapter uses a consistent set of callouts. Here is what each means:

| Label | Meaning |
|-------|---------|
| 📚 **History Corner** | Historical context for the concept or algorithm |
| 🔗 **Analogy** | A real-world parallel that builds intuition |
| 💡 **Did You Know?** | Surprising or counterintuitive fact |
| 📌 **Important Note** | A key concept that is frequently misunderstood |
| ⚠️ **Warning** | A common mistake or Java-specific pitfall |
| 🔑 **Tip** | A practical technique or insight |
| 🤔 **Think Before You Code** | A question to pause and reason through |
| 🏆 **Mini Challenge** | A short exercise to consolidate the topic |
| 🎯 **Interview Insight** | How this topic appears in technical interviews |
| ⚡ **Myth vs Reality** | Corrects a common misconception |
| 🔥 **Hard Problem Corner** | A challenging extension problem |
| 🔄 **Pattern Connections** | Links this topic to related patterns in other chapters |
| 🌍 **Real-World Application** | Where this algorithm is used in production systems |

---

## How to Contribute a New Chapter

1. Check [INDEX.md](INDEX.md) for the chapter number and planned sections.
2. Create `chapters/chapter-NN-kebab-case-title.md` following the structure of Chapter 1.
3. Use the callout conventions above consistently.
4. Add Mermaid diagrams where they clarify the algorithm.
5. Include: learning objectives, all sections, at least one analogy, a dry run table, complexity analysis, loop invariant (where applicable), Java pitfalls, an interview insight, a mini challenge, a quick quiz with answers, and practice problems.
6. Update the status table in this README.
7. Commit with the message: `feat(chNNN): add Chapter N — Title`.

---

## Design Principles

- **Java only.** Every code example is complete, compilable Java.
- **Diagrams as source.** All diagrams are Mermaid (text) — editable, diffable, version-controllable.
- **Depth over breadth.** Each topic is covered thoroughly, not superficially.
- **Interview-ready.** Every chapter ends with practice problems and interview framing.
- **One chapter per file.** Chapters are independent, composable units.
- **No build step.** Plain Markdown — readable in any editor, renderable on GitHub.

---

## License

See [LICENSE](LICENSE) when added. All content is original.
