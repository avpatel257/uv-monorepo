# Setting up the monorepo

```bash
mkdir uv-monorepo
cd uv-monorepo
uv init
uv add --group dev ruff pyright
mkdir projects
uv init --package --lib projects/lib-one
uv init --package --lib projects/lib-two
uv lock
```

- `uv init` initializes a project. A workspace is a directory that contains one or more packages. It’s a way to group packages together and manage their dependencies, while allowing cross-package dependencies.

- `uv add --group dev` adds dependencies to the root project. Development groups are only used when working on the project and are not registered as dependencies when the project is published. Downstream projects will not inherit these dependencies.

- `uv init --package --lib projects/lib-one` initializes a new package in the projects directory. The --lib flag tells uv that this package is a library. This is important because it will add a [build-system] section to the pyproject.toml file, which is required for uv to know how to build the package.

- `uv lock `creates a uv.lock file that contains the resolved dependencies for the workspace. This file is used by uv to determine which versions of the dependencies to install.


After this section, you should see something like this (non-essential files are omitted):


```txt
.
├── README.md
├── projects
│   ├── lib-one
│   │   ├── pyproject.toml
│   │   └── src
│   │       └── lib_one
│   │           └── __init__.py
│   └── lib-two
│       ├── pyproject.toml
│       └── src
│           └── lib_two
│               └── __init__.py
├── pyproject.toml
└── uv.lock

```

Projects recognized by uv as workspace members share the same uv.lock file, environment, can be added as dependencies to each other, and can be managed with uv commands.

# Module Dependencies

To demonstrate how one project can be added as a dependency to another, let’s add lib-one as a dependency to lib-two:

```bash
uv add --package lib-two lib-one
```

The --package flag tells uv to execute the command in the context of the lib-two package. The lib-one package is added as a dependency to lib-two’s pyproject.toml and the root uv.lock file is updated automatically.

