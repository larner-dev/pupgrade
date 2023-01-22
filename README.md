![PUPGRADE](./pupgrade-logo.png)

[Plop](https://github.com/plopjs/plop) is a wonderful code generation / scaffolding tool, but upgrading projects when the original template changed is a pain. PUPGRADE (short for plop-upgrade) is a tool that makes this upgrade process easy.

## How it works

Under the hood, PUPGRADE stores hashed of the original version of the template that was used to generate each project. When the underlying template changes and you run pupgrade, it looks at each file and decides what to do (overwrite, ignore, show conflict) based on the original version of the template, the new version of the template and the current version of the project code.

PUPGRADE has special handling for config files (currently only JSON is supported). Since these files are often the source of conflicts, rather than comparing the entire file it looks at each property individually to determine if it can be safely overwritten or if it needs to generate a conflict.

PUPGRADE supports both a cli interface as well as a node API for programmatic use.

## Command Line

You can run pupgrade using npx or install it locally:

### Using npx

```
npx @larner.dev/pupgrade <command> ...options
```

### Installing locally

```
npm install -g npx @larner.dev/pupgrade

pupgrade <command> ...options
```

---

### pupgrade new

> Used when you are starting a new project from a plop template:

```
pupgrade new path/to/project --template path/to/plopfile.js
```

##### Arguments

- `path`: Location where the new project should be generated.

##### options

- `-t, --template <plopfile>`: Location of the plopfile to use.
- `-h, --help`: Show help for this command.

---

### pupgrade init

> Used when you want to set up PUPGRADE in an existing project that wasn't originally generated with PUPGRADE:

```
pupgrade init path/to/project --template path/to/plopfile.js
```

##### Arguments

- `path`: Location of the existing project.

##### options

- `-t, --template <plopfile>`: Location of the plopfile that was originally used to generate this project.
- `-h, --help`: Show help for this command.

---

### pupgrade upgrade

> Used when the underlying template changes and you want to update an existing project that was originally generated with an older version of the template.

```
pupgrade upgrade path/to/project --template path/to/new/plopfile.js
```

##### Arguments

- `path`: Location of the existing project.

##### options

- `-t, --template <plopfile>`: Location of the new plopfile.
- `-u, --update`: Use this flag if you'd like to update your answers to the questions in the plopfile. Without this flag the upgrade process will assume your answers have remained unchanged.
- `-h, --help`: Show help for this command.

## Node API

## Upgrade Algorithm

For each file in the template, the tool looks at 3 versions of the file.

1. `before`: the file from the previous version of the template
1. `current`: the file from the current version of the project
1. `new`: the file from the new version of the template

| Relationship             | Action                                                                                                                                  |
| ------------------------ | --------------------------------------------------------------------------------------------------------------------------------------- |
| before == current == new | Pick **current** version. Nothing has changed.                                                                                          |
| before == new            | Pick **current** version. The underlying template hasn't changed, but the project has modified this file.                               |
| current == new           | Pick **current** version. The underlying template has changed, but the project has already been updated.                                |
| before == current        | Pick **new** version. The underlying template has changed, but the project has not modified this file, so it's probably safe to update. |
| all different            | Generate **diff**. All three versions are different so you will need to resolve the conflict manually.                                  |

The above logic applies to all non config files (currently anything other than files with a .json extension).

For config files, similar logic is used, but each property is checked rather than applying the logic to the entire file.
