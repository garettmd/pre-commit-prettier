# pre-commit-prettier

A [pre-commit](https://pre-commit.com/) hook for running [Prettier](https://prettier.io/) to format your code.

## Usage

Add the following to your `.pre-commit-config.yaml`:

```yaml
repos:
  - repo: https://github.com/your-org/pre-commit-prettier
    rev: v1.0.0
    hooks:
      - id: prettier
```

By default the hook runs `prettier --write --list-different --ignore-unknown` on staged files matching any of these types:

- CSS
- GraphQL
- HTML
- JavaScript / JSX
- JSON / JSON5
- Less
- Markdown
- SCSS
- TypeScript / TSX
- Vue
- YAML

## Overriding defaults

Consumers can override the hook's behavior in their `.pre-commit-config.yaml`.

### Passing extra arguments

Use `args` to pass additional flags to Prettier. These are inserted between the flags defined in `entry` and the list of filenames:

```yaml
hooks:
  - id: prettier
    args: [--config, .prettierrc.json, --prose-wrap, always, --single-quote]
```

### Restricting file types

Override `types_or` to limit which file types the hook runs on:

```yaml
hooks:
  - id: prettier
    types_or: [javascript, typescript, json]
```

Or use a `files` pattern instead:

```yaml
hooks:
  - id: prettier
    files: '^src/.*\.(js|ts|tsx)$'
```

### Excluding files

Use `exclude` to skip files that match a pattern:

```yaml
hooks:
  - id: prettier
    exclude: '(^vendor/|\.min\.js$)'
```

### Adding Prettier plugins

Use `additional_dependencies` to install Prettier plugins into the hook's environment:

```yaml
hooks:
  - id: prettier
    additional_dependencies:
      - prettier-plugin-tailwindcss@0.6.0
      - prettier-plugin-organize-imports@4.0.0
```

## How it works

When you run `git commit`, pre-commit passes the staged files that match the configured types to Prettier. The hook:

1. Formats each file in place (`--write`).
2. Prints the names of any files that were changed (`--list-different`).
3. Silently skips files Prettier doesn't recognize (`--ignore-unknown`).
4. Exits with a non-zero code if any files were reformatted, which causes pre-commit to abort the commit.

You can then review the changes, `git add` the reformatted files, and commit again.
