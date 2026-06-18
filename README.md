# QuantaLang TextMate Grammar

> The editor-facing TextMate grammar that teaches syntax highlighters how to read `.quanta` files.

[![license: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
![grammar](https://img.shields.io/badge/grammar-TextMate-blue.svg)
![version](https://img.shields.io/badge/version-0.1.0-informational.svg)
[![CI](https://github.com/HarperZ9/quantalang-tmLanguage/actions/workflows/ci.yml/badge.svg)](https://github.com/HarperZ9/quantalang-tmLanguage/actions/workflows/ci.yml)
![deps: none](https://img.shields.io/badge/deps-none-success.svg)
[![part of: Quanta ecosystem](https://img.shields.io/badge/part_of-Quanta_ecosystem-00b3a4.svg)](https://github.com/HarperZ9/quanta-universe)

Official TextMate grammar for **[QuantaLang](https://github.com/HarperZ9/quantalang)**.
This is the small editor-facing layer that teaches syntax highlighters how to
read `.quanta` files without carrying compiler behavior or backend claims.

## What this is

A language definition that tells syntax-highlighting engines how to render `.quanta` source files. The same grammar powers:

- The QuantaLang VS Code extension
- Syntax highlighting on GitHub (via [github-linguist](https://github.com/github-linguist/linguist))
- Any TextMate-compatible editor (Sublime Text, TextMate, Atom, etc.)

## Files

| Path | Purpose |
|------|---------|
| `grammars/quantalang.tmLanguage.json` | The grammar definition |
| `language-configuration.json` | Comment tokens, brackets, surrounding-pair rules |
| `samples/` | Representative `.quanta` files used by linguist for language detection |

## QuantaLang in brief

QuantaLang is an effects-oriented systems language. Keywords in this grammar fall into several categories:

- **Classical systems keywords** - `fn`, `struct`, `enum`, `trait`, `impl`, `let`, `mut`, `pub`, `mod`, `use`, `if`, `else`, `match`, `loop`, `while`, `for`, `in`, `return`, `break`, `continue`, `ref`, `move`, `unsafe`, `extern`, `async`, `await`, `dyn`, `where`, `typeof`, `sizeof`, `true`, `false`, `Self`, `self`, `crate`, `super`
- **Effect system** - `with`, `effect`, `handle`, `resume`, `perform`
- **AI/neural primitives** - `ai`, `neural`, `infer`
- **Module system** - `module` (ecosystem-level), alongside standard `mod`
- **Macros** - `macro`, `macro_rules`
- **Reserved** - `abstract`, `become`, `do`, `final`, `override`, `priv`, `try`, `yield`, `union`, `default`, `auto`, `box`

Primitive types recognized: `i8`, `i16`, `i32`, `i64`, `i128`, `isize`, `u8`, `u16`, `u32`, `u64`, `u128`, `usize`, `f32`, `f64`, `bool`, `char`, `str`, `String`, and common standard types (`Vec`, `Option`, `Result`, `Box`, `Rc`, `Arc`, `HashMap`, `HashSet`, `BTreeMap`, `BTreeSet`).

## Using this grammar

### VS Code

Install the [QuantaLang VS Code extension](https://marketplace.visualstudio.com/items?itemName=HarperZ9.quantalang) - it bundles this grammar.

### Sublime Text / TextMate

Place the grammar file in the appropriate packages directory. Sublime Text accepts `.tmLanguage.json` directly since ST4.

### github-linguist

This repository is included as a submodule by `github-linguist/linguist` under `vendor/grammars/quantalang-tmLanguage`. Consumers do not need to install anything.

## License

MIT. See [LICENSE](LICENSE).
