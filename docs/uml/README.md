# KiwiDB — UML Diagrams (PlantUML)

PlantUML source for the core UML diagram set for the KiwiDB LSM-tree key-value store.
Every diagram is grounded in the actual code under `app/`.

Each `.puml` has already been rendered to a matching `.svg` and `.png` in this folder
(e.g. `02-class.puml` → `02-class.svg` / `02-class.png`). Re-render any time with the
commands below after editing the source. Rendering the full set needs **Graphviz**
(`brew install graphviz`) for the class, state, component, deployment and use-case
layouts; sequence and activity diagrams use PlantUML's built-in engine.

## Diagram index

| File | Diagram | Type |
|------|---------|------|
| `00-architecture.mmd` | Architecture overview — labeled data flows (Mermaid) | Block / overview |
| `01-use-case.puml` | Actors and use cases | Use Case |
| `02-class-highlevel.puml` | Major classes and relationships (overview) | Class (high-level) |
| `02-class.puml` | Full class structure of the engine | Class (detailed) |
| `03-seq-put.puml` | Write (`put`) flow | Sequence |
| `04-seq-get.puml` | Read (`get`) flow | Sequence |
| `05-seq-flush.puml` | Two-phase flush pipeline | Sequence |
| `06-seq-compaction.puml` | Compaction via subprocess | Sequence |
| `07-seq-recovery.puml` | Crash recovery on `open` | Sequence |
| `08-activity-write.puml` | Write path with freeze/backpressure | Activity |
| `09-activity-read.puml` | Read path with bloom branch | Activity |
| `10-activity-compaction.puml` | Compaction selection + execution | Activity |
| `11-activity-recovery.puml` | Recovery procedure | Activity |
| `12-state-memtable.puml` | MemTable lifecycle | State Machine |
| `13-state-sstable.puml` | SSTable file lifecycle | State Machine |
| `14-state-flushslot.puml` | Flush slot lifecycle | State Machine |
| `15-state-engine.puml` | Engine lifecycle | State Machine |
| `16-component.puml` | Subsystems and interfaces | Component |
| `17-deployment.puml` | Runtime nodes and artifacts | Deployment |

## How to render

### Option A — VS Code (easiest)
Install the **PlantUML** extension (`jebbs.plantuml`), open any `.puml`, press
`Alt+D` to preview, then right-click → *Export Current Diagram* → PNG/SVG.

### Option B — Command line
Needs Java + Graphviz + the PlantUML jar:

```bash
# one-time setup
brew install graphviz
curl -L -o plantuml.jar https://github.com/plantuml/plantuml/releases/latest/download/plantuml.jar

# render one file to PNG (or use -tsvg / -tpdf)
java -jar plantuml.jar -tpng docs/uml/02-class.puml

# render everything in the folder, to SVG and PNG
java -jar plantuml.jar -tsvg docs/uml/*.puml
java -jar plantuml.jar -tpng docs/uml/*.puml
```

The image is written next to the `.puml` file with the same base name
(e.g. `02-class.puml` → `02-class.png`).

### Option C — Online
Paste the file contents into the editor at <https://www.plantuml.com/plantuml>.

## Rendering the Mermaid version (`00-architecture.mmd`)

The Mermaid source renders natively on GitHub and in most Markdown editors. To
produce image files locally use the Mermaid CLI:

```bash
npm install -g @mermaid-js/mermaid-cli       # one-time
mmdc -i docs/uml/00-architecture.mmd -o docs/uml/00-architecture-mermaid.svg -b white
mmdc -i docs/uml/00-architecture.mmd -o docs/uml/00-architecture-mermaid.png -b white -s 2
```

If `mmdc` cannot find a browser, point it at an installed one with a puppeteer
config: `-p pptr.json` where `pptr.json` is
`{ "executablePath": "/Applications/Google Chrome.app/Contents/MacOS/Google Chrome", "args": ["--no-sandbox"] }`.

The architecture overview is Mermaid-only (`00-architecture.*`); all other diagrams
(`01`–`17`) are PlantUML.

**Colour key for the architecture figure** (the in-figure legend was dropped to keep it
compact — add this to the report caption): green = in memory · blue = on disk ·
amber = background work · black = engine.

## Using the images in the LaTeX report

Render to PNG or PDF, then include:

```latex
\begin{figure}[H]
  \centering
  \includegraphics[width=\textwidth]{uml/02-class.png}
  \caption{KiwiDB class diagram}
  \label{fig:class}
\end{figure}
```

SVG keeps text crisp at any zoom; PDF is the cleanest for LaTeX/`pdflatex`
(`-tsvg` then convert, or render `-tpdf` directly).
