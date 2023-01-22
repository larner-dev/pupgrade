![PUPGRADE](./pupgrade-logo.png)

[Plop](https://github.com/plopjs/plop) is a wonderful code generation / scaffolding tool, but upgrading projects when the original template changed is a pain. PUPGRADE (short for plop-upgrade) is a tool that makes this upgrade process easy.

## How it works

...

PUPGRADE supports both a cli interface as well as an API for programmatic use.

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
