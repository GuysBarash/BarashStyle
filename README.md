---
applyTo: '**'
description: 'Project-wide rules for LLM behavior, Barash style'
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

## Structure
- Avoid helper methods used only once.
- Prefer monolithic functions when logic is single-use.
- Do not split logic into functions unless reused or clearly justified.

## Sections
- Avoid "floating" code.
- Code outside a logic section is allowed only when necessary
  (function/class definitions, constants, imports, CLI arg parsing, main guard).
- All executable logic inside a function should be inside section gates.

- When separating logic inside a function, use this exact format:

  section_{title} = True  
  if section_{title}:  
      {code}  
      # outputs: {files_created_or_modified_or_none}

- At the end of each section, add a short comment listing outputs:
  - If the section creates/modifies files: `# outputs: path/to/file1, path/to/file2`
  - If no files are created/modified: `# outputs: none`

## Imports
- All imports must be at the top of the file.
- Do NOT place imports inside functions or methods.
- Do not use conditional or lazy imports unless explicitly justified.

## Loops and Naming
- When iterating in loops, use meaningful variable names.
- Avoid generic names like `i`, `j`, `k` when a semantic name is possible.
- Prefer names such as `row_i`, `idx_patient`, `step_n`.

## Paths and Routes
- Define a single project root as an absolute path.
- The root should be derived from `__file__` when possible.
- Do NOT rely on the current working directory.
- Every path used must be an absolute path.
- All paths must be generated relative to the defined root.
- Do NOT mix relative and absolute paths.

## Data Handling (Pandas)
- When storing meaningful intermediate results (values likely inspected or debugged),
  prefer `pandas.Series` or `pandas.DataFrame` when reasonable.
- This applies especially to counts, min/max, mean/median, and distributions.
- Prefer inspectability and debugging convenience over raw scalar values.

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
- Avoid global state.

## Explanations
- Keep explanations short and technical.
- If suggesting multiple approaches, say which one is better and why.
- Call out bad ideas directly.

## Style
- No emojis.
- No motivational or conversational language.
- Do not restate the prompt.

