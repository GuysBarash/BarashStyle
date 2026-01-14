# Chaptered Research Scripts

A coding paradigm for computational research and experimental workflows.

## What Is This?

This is a structured approach to writing research code that prioritizes **reproducibility**, **readability**, and **iterative development** over traditional software engineering patterns.

Code is organized as **executable chapters** using section gates—a pure Python pattern that behaves like Jupyter notebooks but maintains all the benefits of standard `.py` files.

## Philosophy

**Not a library. Not production software. An executable document.**

Research code needs different priorities than production systems:
- **Narrative flow** over modularity
- **Reproducibility** over reusability  
- **Exploration** over optimization
- **Transparency** over abstraction

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

**Benefits:**
- Toggle chapters on/off by setting gates to `False`
- Skip expensive computations on re-runs
- Clean conversion to Jupyter notebooks (each gate → one cell)
- Linear execution, no hidden dependencies
- Version control friendly (pure Python)

## When To Use This

**Good for:**
- ML experiments and model prototyping
- Data analysis and exploratory research
- Scientific computing workflows
- Algorithm development and testing
- One-off computational experiments

**Not good for:**
- Production APIs or web services
- Reusable libraries
- Team codebases with multiple contributors
- Long-term maintained software

## Key Principles

### 1. Chapters, Not Functions
Logic stays linear. Functions only for genuine reuse across multiple scripts.

### 2. Absolute Paths Always
```python
root = Path(__file__).parent.absolute()
data_dir = root / "data"
```
No relative paths. Scripts work regardless of execution directory.

### 3. Cache Heavy Work
Save expensive computations to disk (CSV/JSON). Skip chapters on re-runs.
```python
section_expensive = False  # Skip this, use cached data
if section_expensive:
    results = train_model(data)
    results.to_csv(cache_dir / "model_results.csv")
```

### 4. Document State Flow
Every chapter ends with `# outputs:` listing what it produces (files or variables).

### 5. Nest When Long
Chapters over 150 lines get split into nested sub-chapters:
```python
section_process = True
if section_process:
    section_process_clean = True
    if section_process_clean:
        # cleaning logic
    
    section_process_transform = True
    if section_process_transform:
        # transformation logic
```

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
        # Define all file paths
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
        # outputs: cleaned_data
    
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

## Comparison to Other Approaches

| Feature | Chaptered Scripts | Jupyter Notebooks | Literate Programming | Traditional Scripts |
|---------|------------------|-------------------|---------------------|-------------------|
| Pure Python | ✓ | ✗ (JSON) | ✗ (custom markup) | ✓ |
| Version control friendly | ✓ | ✗ | ✗ | ✓ |
| Toggle execution blocks | ✓ | ✓ | ✗ | ✗ |
| Standard tooling | ✓ | ✗ | ✗ | ✓ |
| Narrative structure | ✓ | ✓ | ✓ | ✗ |
| Reproducible | ✓ | ✗ (hidden state) | ✓ | ✓ |

## Design Decisions

**Why not functions?**  
Research code is read linearly, like a paper. Functions hide the narrative and create artificial boundaries.

**Why not classes?**  
State flows forward through chapters. Object-oriented design optimizes for reuse, not exploration.

**Why not modules?**  
Keeping everything in one file maintains context. Split only when genuinely reused across 3+ scripts.

**Why not Jupyter?**  
Notebooks are great for exploration but terrible for version control, debugging, and production paths. This gives you both.

## Rules Document

Full coding guidelines in `barash_style_rules.instructions.md`

Key rules:
- No floating code (everything in chapters or at top-level)
- All paths absolute, derived from `root`
- Cache expensive operations
- Minimal documentation (code is the documentation)
- Avoid try/except unless I/O or external systems
- Single letters OK for loops, abbreviations OK for domain terms

## Examples

See `run_cm_with_manual_input.py` for a complete example of:
- Patient data generation
- Model prediction workflow  
- Nested chapter structure
- Result logging and visualization

## Contributing

This is a personal coding style, not a collaborative framework. Fork and adapt as needed.

## License

Do whatever you want with it.
