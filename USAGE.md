# Using the QuantaLang TextMate Grammar

This repository is an **editor-facing TextMate grammar**, not a compiler or a
runnable program. There is no CLI to install and no library to import. "Using"
this package means installing the grammar into a TextMate-compatible editor (or
consuming it as a vendored grammar) so that `.quanta` source files get syntax
highlighting.

The compiler and language semantics (including any `quantac` command) live in a
separate repository, [`HarperZ9/quantalang`](https://github.com/HarperZ9/quantalang).
This package only describes how `.quanta` text is tokenized for highlighting.

## What the package provides

| File | Purpose |
|------|---------|
| `grammars/quantalang.tmLanguage.json` | The TextMate grammar (scope name `source.quanta`, file type `.quanta`) |
| `language-configuration.json` | Comment tokens, brackets, auto-closing/surrounding pairs, folding markers |
| `samples/` | Representative `.quanta` files used for language detection and highlighting checks |

The grammar registers:

- Scope name: `source.quanta`
- File extension: `.quanta`

## Install / use

### VS Code

Install the [QuantaLang VS Code extension](https://marketplace.visualstudio.com/items?itemName=HarperZ9.quantalang),
which bundles this grammar. No manual file copying is needed; opening any
`.quanta` file is highlighted automatically.

### Sublime Text (ST4+)

Sublime Text accepts `.tmLanguage.json` files directly. Copy the grammar into
your `Packages/User` directory:

```bash
# macOS example path; adjust for your OS
cp grammars/quantalang.tmLanguage.json \
  "$HOME/Library/Application Support/Sublime Text/Packages/User/"
```

Open a `.quanta` file; Sublime maps the `source.quanta` scope to your color
scheme automatically.

### TextMate / other TextMate-compatible editors

Place `grammars/quantalang.tmLanguage.json` (and, where the editor supports it,
`language-configuration.json`) in the editor's grammar/bundle directory. The
exact location is editor-specific; consult that editor's grammar/bundle docs.

### github-linguist

This repository is intended to be vendored by
[`github-linguist/linguist`](https://github.com/github-linguist/linguist) under
`vendor/grammars/quantalang-tmLanguage`. Consumers of GitHub do not install
anything; the `samples/` files drive language detection.

## Validating and packaging the grammar

These are the genuine commands this repo uses (mirrored from CI in
`.github/workflows/`). They require only Python 3 (the standard library
`json.tool` module) and git.

### Validate the JSON files

```bash
python -m json.tool grammars/quantalang.tmLanguage.json > /dev/null
python -m json.tool language-configuration.json > /dev/null
```

Expected: no output and a zero exit code when the JSON is well-formed. A
malformed file prints a `JSONDecodeError` with the line/column.

Verified by author: both commands exit 0 on the current tree.

### Check for whitespace problems

```bash
git diff --check
```

Expected: no output (and exit 0) when there are no trailing-whitespace or
conflict-marker issues in the working tree.

### Build the source package (what the release workflow does)

The `release-artifacts` workflow builds `dist/quantalang-tmLanguage-0.1.0.zip`
from the publishable files using a short Python script. The committed
`dist/quantalang-tmLanguage-0.1.0.zip` is the result of that workflow.

See `.github/workflows/release-artifacts.yml` for the exact build step.

## Worked examples (highlighting)

The grammar maps QuantaLang constructs to TextMate scopes. The examples below
show which scope a given token receives, taken directly from
`grammars/quantalang.tmLanguage.json`. These are illustrative of the grammar's
behavior, not output from a tokenizer run.

### Example 1 — keywords and effect/AI primitives

Source:

```quanta
fn handle_request() {
    let x = perform infer();
}
```

Resulting scopes (illustrative):

- `fn` -> `keyword.other.fn.quanta`
- `let` -> `storage.type.quanta`
- `perform` -> `keyword.other.effect.quanta`
- `infer` -> `keyword.other.ai.quanta`
- `handle_request` (after `fn`) -> `entity.name.function.quanta`

### Example 2 — types and primitives

Source:

```quanta
let buf: Vec<u8> = Vec::new();
```

Resulting scopes (illustrative):

- `Vec`, `u8` -> `storage.type.primitive.quanta`
- A capitalized identifier not in the primitive list (e.g. a user type
  `Color`) -> `entity.name.type.quanta`

### Example 3 — numbers and string interpolation

Source:

```quanta
let n = 0xFF_u32;
let s = "value is {n}";
```

Resulting scopes (illustrative):

- `0xFF_u32` -> `constant.numeric.hex.quanta`
- The `"..."` body -> `string.quoted.double.quanta`
- `{n}` inside the string -> `meta.interpolation.quanta`, with `n` as
  `variable.other.quanta`

### Example 4 — comments and attributes

Source:

```quanta
/// Doc comment
#[derive(Clone)]
struct Color {}
```

Resulting scopes (illustrative):

- `/// Doc comment` -> `comment.line.documentation.quanta`
- `#[derive(Clone)]` -> `meta.attribute.quanta` (with `derive` as
  `entity.name.tag.quanta`)
- `struct` -> `storage.type.quanta`

To see highlighting end-to-end, open any file in `samples/` (or
`examples/demo.quanta`) in an editor that has the grammar installed.
