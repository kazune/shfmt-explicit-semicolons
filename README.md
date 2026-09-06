# shfmt with explicit semicolons

This repository is a fork of [mvdan/sh](https://github.com/mvdan/sh). It adds
an option to make `shfmt` emit semicolons at the end of shell statements.

The default behavior remains unchanged: without the option described below,
the formatter behaves like upstream `shfmt`.

## Upstream

- Repository: [mvdan/sh](https://github.com/mvdan/sh)
- Go module: [`mvdan.cc/sh/v3`](https://pkg.go.dev/mvdan.cc/sh/v3)

## Differences from upstream

### `--explicit-semicolons`

When enabled, `shfmt` appends `;` to each shell statement that does not already
have a terminator. The semicolon is added to the end of the statement, rather
than after each command within an `&&`, `||`, or pipeline expression. It can
appear immediately before a line break or a here-document body.

Statements that end with `&`, `|&`, or `&|` keep those terminators, and existing
semicolons are not duplicated.

```sh
foo;
if cond; then
	bar;
fi;
```

The option is available through:

- CLI: `shfmt --explicit-semicolons`
- EditorConfig: `explicit_semicolons = true`
- Go API: `syntax.ExplicitSemicolons(true)`

This fork currently keeps the option long-only; there is no short flag.

## Why this fork exists

Some workflows benefit from making command termination explicit in formatted
shell code. This can be particularly useful when formatting shell commands
embedded in Makefiles.

This fork provides that output style while keeping the rest of its behavior
close to upstream.

## Usage

Install this fork from a checkout of the repository:

```sh
git clone https://github.com/kazune/shfmt-explicit-semicolons.git
cd shfmt-explicit-semicolons
go install ./cmd/shfmt
```

Format a file with explicit semicolons:

```sh
shfmt --explicit-semicolons script.sh
```

To enable the option through EditorConfig:

```ini
explicit_semicolons = true
```

The ordinary `shfmt` usage and all other formatting options are documented
upstream.

## Upstream documentation

- [Upstream README]
- [`shfmt` man page][upstream man page]
- [Syntax package documentation][syntax package]
- [Shell package documentation][shell package]
- [Interpreter package documentation][interp package]

## Upstream tracking

This fork follows upstream changes periodically. Fork-specific changes are
kept limited to the explicit-semicolon functionality. If equivalent support
is accepted upstream, this fork may be retired or reduced to a compatibility
branch.

[Upstream README]: https://github.com/mvdan/sh#readme
[upstream man page]: https://github.com/mvdan/sh/blob/master/cmd/shfmt/shfmt.1.scd
[syntax package]: https://pkg.go.dev/mvdan.cc/sh/v3/syntax
[shell package]: https://pkg.go.dev/mvdan.cc/sh/v3/shell
[interp package]: https://pkg.go.dev/mvdan.cc/sh/v3/interp
