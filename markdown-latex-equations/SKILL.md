---
name: markdown-latex-equations
description: Repair, format, number, and validate LaTeX mathematics in Markdown files. Automatically inspect every newly created or substantively rewritten Markdown file for mathematical notation, variables, or formulas; if found, apply editable LaTeX formatting. Also use when a user asks to display, center, edit, convert, or number formulas, including Chinese requests such as “Markdown 公式显示不出来”, “公式居中编号”, “LaTeX 可编辑公式”, or “修复 Markdown 数学公式”.
metadata:
  version: 1.1.0
---

# Markdown LaTeX Equations

Use this skill for Markdown files whose mathematical notation must render correctly in a MathJax/KaTeX-capable Markdown viewer while remaining editable as plain LaTeX source.

## Automatic trigger for Markdown artifacts

Before delivering any newly created or substantively rewritten .md file, inspect its intended content for mathematical notation.

Activate the formatting workflow automatically when the file contains one or more of the following:

- an equation, equality, inequality, probability, optimization, loss, recurrence, set definition, or mathematical derivation;
- variables embedded in prose, such as Y, X_t, p_i, F_{t+1}, \\bar p, or a named matrix/vector/tensor;
- mathematical operators or notation such as =, \\Pr, \\sum, \\arg\\min, \\mathbb, \\mathbf, \\lambda, \\tau, subscripts, superscripts, or Greek symbols;
- an explicitly requested model definition, algorithm, mathematical method, experiment metric, or formula-bearing table.

When this trigger fires:

1. Apply this skill before the Markdown artifact is delivered.
2. Convert prose variables to inline math where appropriate.
3. Convert standalone mathematical relationships to centered, numbered display equations.
4. Run the validation checks in this skill.

Do not activate solely because a Markdown document contains ordinary numbers, dates, filenames, code identifiers, URLs, citation years, units, or a non-mathematical table. If a file contains no mathematical notation, leave it as normal Markdown.

## Scope and core rule

The source Markdown remains the editable artifact. Do not turn equations into images, screenshots, SVG paths, HTML tables, or Unicode approximations unless the user explicitly asks for a non-editable output.

Use standard math delimiters:

- Inline math: $ ... $ when it sits within prose.
- Display math: a blank line, then $$, the LaTeX expression, and closing $$ on its own line.

Most MathJax/KaTeX Markdown renderers center display math automatically. Do not insert manual spaces or Markdown table cells to force centering.

## Workflow

1. Inspect the target Markdown before editing.
   - Find malformed delimiters such as literal \[ / \] that the active renderer does not support, doubled \\[ / \\], unmatched delimiters, and accidentally unescaped inline LaTeX.
   - Count all display blocks before changing the file.
   - Identify the Markdown renderer if the user supplies it. If not supplied, use standard $/$$ syntax and state that a MathJax/KaTeX-capable preview is required.

2. Preserve the mathematics.
   - Change delimiters and layout only unless the formula itself is clearly malformed.
   - Correct genuine syntax errors only when the intended expression is unambiguous.
   - Do not silently alter variables, signs, subscripts, indices, or citations.

3. Convert display equations.
   - Use a display block with $$ on the opening and closing lines.
   - Place the LaTeX expression between the delimiters, then add \tag{1.1} before the closing delimiter.
   - Keep \tag{...} inside the same display block. MathJax/KaTeX renders it as a right-aligned equation number with parentheses.
   - Use \begin{aligned} ... \end{aligned} for multi-line formulas.
   - End equations without a prose period inside display math unless punctuation is mathematically intended.

4. Number equations hierarchically.
   - Ask the user for a numbering convention only if the document has no evident section structure.
   - Default to section.equation: 1.1, 1.2, 2.1.
   - Use a letter suffix for tightly paired or derived expressions: 3.1.a, 3.1.b.
   - Do not renumber equations that are explicitly cross-referenced unless every affected reference is updated in the same change.
   - Place an explanatory label above the first equation only if it improves reading; the number itself belongs in \tag.

5. Keep inline math compact.
   - Use inline LaTeX for Y=1, \\bar p, and f(EW)\\ne E[f(W)].
   - Do not use display math for a single symbol embedded in a sentence.
   - Repair accidental plain-text parentheses around variables, such as (Y) when the intended notation is inline LaTeX Y, only when context is unambiguous.

6. Validate after editing.
   - Every $$ must have a matching closing $$.
   - Every display block must include at most one \tag{...}.
   - Tags must be unique and follow the selected hierarchy.
   - No legacy display delimiters should remain after a conversion unless the target renderer explicitly supports them.
   - Check for malformed LaTeX such as unmatched {}, \begin{aligned} / \end{aligned} mismatches, and stray backslashes.
   - Open the rendered Markdown in the user’s preview when available. Confirm the first, a multi-line, a sub-numbered, and the final display equation render centered with a number.

## Compatibility boundary

$$ ... $$ is editable LaTeX source and generally gives the best Markdown compatibility. Rendering and click-to-edit behavior ultimately belong to the Markdown application. In an editor that does not enable MathJax/KaTeX, no source-only change can force equation rendering; report that limitation and name the required renderer setting.

## Reporting

State:

- the target file;
- the number of display equations repaired;
- the numbering scheme applied;
- the validation result;
- any renderer limitation that remains.
