---
applyTo: '**'
description: 'Project-wide rules for LLM / Copilot behavior'
---

# LLM Rules (Project)

## General
- Be concise. No filler.
- Tell the truth, even if it contradicts the user.
- Prefer correctness over elegance.

## Code Editing
- Do NOT rewrite full files unless explicitly asked.
- When editing code: show only changed lines with minimal surrounding context.
- Prefer linear, readable logic over abstraction.

## Code Organization: Chapters Not Functions
- Code is organized as **executable chapters**, not modular functions.
- Think: research script or technical notebook, not software library.
- Linear narrative flow: each chapter builds on previous chapters.
- Avoid helper methods used only once.
- Do not split logic into functions unless reused multiple times or clearly justified.

## Section Gates (Chapters)
Section gates create chapters within a script. Each chapter is a logical phase of execution.

**Format:**
```python
section_{chapter_name} = True
if section_{chapter_name}:
    # chapter code here
    # outputs: {what_this_chapter_produces}
```

**Purpose:**
- Organize code into readable phases (like sections in a paper)
- Allow skipping expensive chapters during development (set to `False`)
- Enable clean conversion to Jupyter notebooks (each gate → one cell)
- Maintain linear execution flow without function abstractions

**Chapter Names:**
- Should read like a table of contents
- Examples: `section_setup`, `section_load_data`, `section_run_model`, `section_visualize`
- Not: `section_1`, `section_helper`, `section_utils`

**Chapter Outputs:**
At the end of each section, document what it produces:
- Files: `# outputs: data/results.csv, figures/plot.png`
- Variables: `# outputs: cleaned_data, model, predictions`
- Nothing: `# outputs: none`

This tells readers (and future chapters) what state is available.

**Chapter Dependencies:**
- Chapters execute top-to-bottom only (no backward references)
- Later chapters can use outputs from earlier chapters
- Critical variables should be listed in the producing chapter's outputs
- Each chapter should be understandable by reading only that section

**Chapter Length and Nesting:**
- Target length: 50-150 lines per chapter (whatever fits on ~2 screens)
- If a chapter exceeds ~150 lines, split it into nested sub-chapters
- Nested chapters use underscore hierarchy: `section_process`, then `section_process_clean`, `section_process_transform`
- Recursively nest as needed: keep each individual chapter block readable

**Example of nested chapters:**
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

**Chapter Boundaries:**
Split chapters at:
- Phase changes (setup → load → process → analyze)
- Expensive operations you might want to skip
- Points where you'd want to re-run from that position
- When a chapter exceeds ~150 lines

Don't split:
- Tightly coupled operations (loading data + immediate validation)
- Setup steps that must happen together

## Typical Chapter Flow
Common progression for research scripts:
1. `section_setup` - Initialize libraries, services, loggers
2. `section_define_paths` - Define all file paths
3. `section_parameters` - Set experimental parameters
4. `section_load_data` - Load or generate input data
5. `section_process` - Transform/clean data
6. `section_compute` - Run expensive operations (with caching)
7. `section_analyze` - Analyze results
8. `section_visualize` - Generate plots/tables
9. `section_export` - Save final outputs

## Chapter Execution
- At the start of expensive chapters, log what's happening:
  `logger.info('Running expensive computation...')`
- When loading cached data, log the source:
  `logger.info(f'Loaded cached results from {cache_file}')`
- This makes re-runs clear about what executed vs what was skipped

## Code Outside Chapters
Code outside section gates is allowed only for:
- Imports (always at top of file)
- Function/class definitions
- Constants
- CLI argument parsing
- `if __name__ == '__main__':` guard

All other executable logic must be inside a chapter.

## File Paths
- ALL paths must be absolute paths.
- Define a `root` variable at the top of the file (outside chapters):
```python
  root = Path(__file__).parent.absolute()
```
- All other paths are derivatives of `root`:
```python
  data_dir = root / "data"
  input_file = data_dir / "input.csv"
```
- Never use relative paths like `"./data"` or `"../output"`
- This ensures the script works regardless of the directory it's run from

## Path Definition Chapter
- When working with files, include a `section_define_paths` chapter early in the script
- This chapter defines all paths used throughout the script
- Example:
```python
  section_define_paths = True
  if section_define_paths:
      root = Path(__file__).parent.absolute()
      data_dir = root / "data"
      output_dir = root / "output"
      input_file = data_dir / "raw_data.csv"
      # outputs: root, data_dir, output_dir, input_file
```

## Caching Heavy Processing
- When a chapter does expensive processing (API calls, model training, large computations), save results to disk
- Use readable formats: CSV for tabular data, JSON for structured data
- This allows skipping the heavy chapter and loading cached results on subsequent runs
- Pattern:
```python
  section_expensive_process = True
  if section_expensive_process:
      # Heavy processing here
      results = expensive_computation(data)
      results.to_csv(cache_dir / "processed_results.csv")
      # outputs: results, cache_dir/processed_results.csv
  
  section_load_or_process = True
  if section_load_or_process:
      cache_file = cache_dir / "processed_results.csv"
      if cache_file.exists():
          results = pd.read_csv(cache_file)
      else:
          results = expensive_computation(data)
          results.to_csv(cache_file)
      # outputs: results
```
- Set the expensive chapter to `False` after first run, rely on cached file

## Parameters
- Always include a `section_parameters` chapter after setup
- Define all experimental parameters in one place:
  - Model hyperparameters
  - Data filters/thresholds
  - Feature selections
- This makes it easy to modify experiments without hunting through code

## Imports
- All imports at the top of the file.
- Do NOT place imports inside functions, methods, or chapters.
- No conditional or lazy imports unless explicitly justified.

## Naming
- Domain abbreviations allowed (em, hvs, hv, cm)
- Single letters OK for short loops (i, j, k) and temporary dicts (d, params)
- Descriptive names for variables used across multiple chapters
- No need to be verbose: `results` not `computation_results`

## Data Inspection
- Use logger.info() to show key results and data shapes
- For tabular output, use tabulate for readable display
- Log file paths after creation: `logger.info(f'Saved to: {output_file}')`
- Show sample data after major transformations (first 5 rows, data shape)

## Documentation
- Keep documentation inside code minimal.
- Prefer single-line comments using `#`.
- Docstrings should be minimal and only when they add real value.
- No explanatory paragraphs inside code.

## Error Handling
- Avoid `try/except` unless necessary.
- Use `try/except` only where runtime failure is likely
  (I/O, network, external systems).
- Do not use `try/except` to hide logic or validation errors.

## Python
- Python 3.11
- Prefer existing standard libraries.
- Prefer vectorized operations (NumPy / Pandas) when beneficial.
- No `import json` unless explicitly required.
- Avoid global state (except for chapter gate variables).

## Explanations
- Keep explanations short and technical.
- If suggesting multiple approaches, say which one is better and why.
- Call out bad ideas directly.

## Style
- No emojis.
- No motivational or conversational language.
- Do not restate the prompt.
