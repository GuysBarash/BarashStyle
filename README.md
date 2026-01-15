# Chaptered Research Scripts

A coding paradigm for computational research and experimental workflows.

## What Is This?

This is a structured approach to writing research code that prioritizes **reproducibility**, **readability**, and **iterative development**.

Code is organized as **executable chapters** using section gates—a pure Python pattern that behaves like Jupyter notebooks but maintains all the benefits of standard `.py` files.

## Philosophy

**Code as executable narrative for computational experiments.**

Not a library. Not traditional software architecture. An executable document that tells the story of your computation from beginning to end.

Research code is read and modified far more than it's abstracted and reused. This paradigm treats code like a lab notebook with the rigor of a scientific paper.

## The Zen of Barash

What is essential is visible to the eye.

A story is told from beginning to end, not in pieces scattered about.

If you cannot see where something comes from, it does not truly belong.

A thing that is simple and works is worth more than a thing that is clever and confuses.

When you write something once, it lives where you wrote it.  
When you write it twice, it still lives where you wrote it.  
Only when you write it three times does it ask for a home of its own.

A path that changes with the wind is not a path at all.  
Make your paths absolute, so they lead home no matter where you wander.

Heavy work is part of life.  
Do it once, do it well, so you need not do it again.

Each chapter is a room in a house.  
When you enter, you should know what you will find inside.  
When you leave, you should know what you are taking with you.

Changes should be like stones in a garden - you move one stone, not the whole garden.

If you cannot explain it to a child, perhaps it is not yet ready.

## Core Pattern: Section Gates
```python
section_setup = True
if section_setup:
    # Initialize environment
    glados = Glados()
    logger = glados.get_logger()
    # outputs: glados, logger

section_parameters = True
if section_parameters:
    patient_id = 'test_001'
    threshold = 0.95
    # outputs: patient_id, threshold

section_compute = True
if section_compute:
    results = expensive_computation(data)
    results.to_csv(cache_dir / "results.csv")
    # outputs: results, results.csv
```

**What This Gives You:**
- Toggle chapters on/off by setting gates to `False`
- Skip expensive computations on re-runs
- Clean conversion to Jupyter notebooks (each gate → one cell)
- Linear execution, no hidden dependencies
- Version control friendly (pure Python)
- Clear boundaries for modification and testing

## When To Use This

**Ideal for:**
- ML experiments and model prototyping
- Data analysis and exploratory research
- Scientific computing workflows
- Algorithm development and comparison
- Computational experiments that need reproducibility
- Agentic systems that need clear execution boundaries
- Production batch processing and data pipelines
- Containerized computational workflows

**Not ideal for:**
- Interactive web applications with many endpoints
- Real-time systems requiring hot-reloading
- Large teams building shared libraries
- Code that needs horizontal scaling across many machines

## Key Principles

### 1. Chapters, Not Functions
Logic stays linear and visible. Functions only for genuine reuse across multiple scripts (3+ uses).

### 2. Absolute Paths Always
```python
root = Path(__file__).parent.absolute()
data_dir = root / "data"
```
Scripts work regardless of execution directory. Paths never change with the wind.

### 3. Cache Heavy Work
Save expensive computations to disk (CSV/JSON). Skip chapters on re-runs.
```python
section_expensive = False  # Skip this, use cached data
if section_expensive:
    results = train_model(data)
    results.to_csv(cache_dir / "model_results.csv")
```

### 4. Document State Flow
Every chapter ends with `# outputs:` listing what it produces (files or variables). When you leave a room, you know what you're taking with you.

### 5. Nest When Long
Chapters over 150 lines get split into nested sub-chapters:
```python
section_process = True
if section_process:
    section_process_clean = True
    if section_process_clean:
        # cleaning logic
        # outputs: cleaned_data
    
    section_process_transform = True
    if section_process_transform:
        # transformation logic
        # outputs: transformed_data
    
    # outputs: cleaned_data, transformed_data
```

## Comparing Approaches

When experimenting with different algorithms or methods:
```python
# Configuration
ALGORITHM = 'random_forest'  # Options: 'linear', 'random_forest', 'neural_net'

if ALGORITHM == 'linear':
    model = LinearRegression()
    results = model.fit(X_train, y_train)
    # outputs: model, results

if ALGORITHM == 'random_forest':
    model = RandomForestRegressor(n_estimators=100)
    results = model.fit(X_train, y_train)
    # outputs: model, results

if ALGORITHM == 'neural_net':
    model = build_neural_net(input_dim=X_train.shape[1])
    results = model.fit(X_train, y_train, epochs=50)
    # outputs: model, results
```

All approaches produce the same outputs, making downstream chapters work regardless of choice.

## Branching Workflows

Create checkpoints where different analytical paths can diverge:
```python
section_process_data = True
if section_process_data:
    cleaned = process(raw_data)
    cleaned.to_csv(cache_dir / "cleaned_data.csv")
    # outputs: cleaned, cleaned_data.csv

# === Different pipelines can start from here ===

section_analysis_pipeline_a = True
if section_analysis_pipeline_a:
    data = pd.read_csv(cache_dir / "cleaned_data.csv")
    results_a = analyze_method_a(data)
    # outputs: results_a

section_analysis_pipeline_b = True  
if section_analysis_pipeline_b:
    data = pd.read_csv(cache_dir / "cleaned_data.csv")
    results_b = analyze_method_b(data)
    # outputs: results_b
```

Parallel approaches can live in the same file or separate files that load from shared checkpoints.

## Collaboration Model

**Approach 1: Chapter Ownership**
- Each researcher owns specific chapters in the main script
- Clear boundaries (inputs/outputs) minimize conflicts
- Merge conflicts only happen in chapters you're both editing
- Changes are local and contained—move one stone, not the whole garden

**Approach 2: Separate Files + Checkpoints**
- Work on different analytical approaches in separate files
- Both load from same cached checkpoint
- Example: `pipeline_a.py` and `pipeline_b.py` both read `cleaned_data.csv`
- Compare results without interfering with each other's work

**Approach 3: Separate Files + Concatenation**
- Write chapters in separate files during development
- Each file documents assumed inputs at top
- Concatenate into final script when ready

## Maintainability Through Transparency

Traditional code uses **abstraction** (functions, classes, modules) to manage complexity.

Chaptered scripts use **transparency** (linear flow, explicit state, visible dependencies) to manage complexity.

**This means:**
- **Onboarding:** Read the script top-to-bottom like a paper
- **Modifying:** Change one chapter, see immediate downstream effects
- **Comparing:** Swap entire analytical approaches by switching chapters
- **Debugging:** Run chapters incrementally, inspect state at each boundary
- **Reproducing:** Same file, same results, 6 months later

No hidden dependencies. All state flows are visible. If you cannot see where something comes from, it does not truly belong.

## Typical Script Structure
```python
# Imports at top
from pathlib import Path
import pandas as pd

# Constants outside chapters
MAX_ITERATIONS = 1000

if __name__ == '__main__':
    section_setup = True
    if section_setup:
        # Initialize libraries, services
        # outputs: logger, glados
    
    section_define_paths = True
    if section_define_paths:
        root = Path(__file__).parent.absolute()
        data_dir = root / "data"
        cache_dir = root / "cache"
        # outputs: root, data_dir, cache_dir
    
    section_parameters = True
    if section_parameters:
        # Set experimental parameters
        # outputs: model_params, thresholds
    
    section_load_data = True
    if section_load_data:
        # Load or generate input
        # outputs: raw_data
    
    section_process = True
    if section_process:
        # Transform and clean
        # outputs: cleaned_data, cleaned_data.csv
    
    section_compute = True
    if section_compute:
        # Run expensive operations
        # outputs: results, results.csv
    
    section_analyze = True
    if section_analyze:
        # Analyze results
        # outputs: metrics, summary_stats
    
    section_visualize = True
    if section_visualize:
        # Generate plots
        # outputs: figures/plot1.png
```

## For Agentic Systems

This pattern is particularly well-suited for building agentic computational systems:

**Clear execution boundaries:**
- Each chapter = potential decision point for the agent
- Agent can execute incrementally, inspect state, decide next action

**Built-in checkpointing:**
- Agent saves state after each chapter
- Can rollback to any chapter if something fails
- Can fork: try multiple approaches from same checkpoint

**Natural planning structure:**
- Agent plans: "I need to setup → load → clean → analyze"
- Maps directly to chapters

**Human-readable execution log:**
- Agent's actions = "executed section_X, produced outputs Y"
- Humans can read the script and understand what happened

**Tool calling pattern:**
- Each chapter can be a tool the agent calls
- Clear inputs/outputs for each tool

## Comparison to Other Approaches

| Feature | Chaptered Scripts | Jupyter Notebooks | Traditional Scripts |
|---------|------------------|-------------------|-------------------|
| Pure Python | ✓ | ✗ (JSON) | ✓ |
| Version control friendly | ✓ | ✗ | ✓ |
| Toggle execution blocks | ✓ | ✓ | ✗ |
| Standard tooling | ✓ | ✗ | ✓ |
| Narrative structure | ✓ | ✓ | ✗ |
| Reproducible | ✓ | ✗ (hidden state) | ✓ |
| Containerizable | ✓ | ✓ | ✓ |
| Agentic execution | ✓ | ✗ | ✗ |

## Design Decisions

**Why not functions?**  
Research code is read linearly, like a paper. Functions hide the narrative and create artificial boundaries. A story is told from beginning to end, not in pieces scattered about.

**Why not classes?**  
State flows forward through chapters. Object-oriented design optimizes for reuse, not exploration.

**Why not modules?**  
Keeping everything in one file maintains context. Split only when genuinely reused across 3+ scripts. When you write it once, it lives where you wrote it.

**Why not Jupyter?**  
Notebooks are great for exploration but problematic for version control, debugging, and production paths. This pattern gives you both: exploration AND reproducibility.

## Rules Document

Full coding guidelines in `barash_style_rules.instructions.md`

Key rules:
- Code organized as executable chapters
- All paths absolute, derived from `root`
- Cache expensive operations to disk
- Document outputs at end of each chapter
- Minimal documentation (code is the documentation)
- Avoid try/except unless I/O or external systems
- Functions only for genuine reuse (3+ times)

## Examples

See `run_cm_with_manual_input.py` for a complete example of:
- Patient data generation workflow
- Model prediction with caching
- Nested chapter structure
- Result logging and visualization
- Algorithm comparison pattern

## Contributing

This is a personal coding paradigm, not a collaborative framework. Fork and adapt as needed for your own work.

## License

Do whatever you want with it.
