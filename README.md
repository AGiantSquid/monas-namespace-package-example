# monas-namespace-package-example
Example demonstrating how to use [monas](https://github.com/frostming/monas) to set up a mono-repo of packages that are scoped to a namespace.

## Overview 

In this project, there are 3 packages that all belong to the `acme` package namespace:

| Package Name | Import Name |
| ------------ | ----------- |
| `acme-lib-a` | `acme.lib_a` |
| `acme-lib-b` | `acme.lib_b` |
| `acme-lib-c` | `acme.lib_c` |

They can be installed together, or separately.

Some of these packages depend on each other. 
`acme-lib-a` depends on `acme-lib-b`, and `acme-lib-b` in turn depends on `acme-lib-c`.
monas can handle transitive dependencies, so installing `acme-lib-a` will also install `acme-lib-b` and `acme-lib-c`.

## Namespace packages

The namespacing is achieved by the folder structure:

```
.
├── acme-lib-a
│   ├── pyproject.toml
│   └── src
│       └── acme
│           └── lib_a
│               └── __init__.py
├── acme-lib-b
│   ├── pyproject.toml
│   └── src
│       └── acme
│           └── lib_b
│               └── __init__.py
├── acme-lib-c
│   ├── pyproject.toml
│   └── src
│       └── acme
│           └── lib_c
│               └── __init__.py
└── pyproject.toml
```

Each package is named with kebab-casing to conform to the Python packaging normalization spec (e.g. `acme-lib-a`) but needs to be imported with the usual python syntax for using a package given this directory structure (e.g. `import acme.lib_a`)

## Usage

To install all packages, run `monas install`

This will create a .venv in the root of each of the three projects, with the necessary dependencies installed for each environment. 

To only install one package, run `monas install --include acme-lib-c`

The above command would create a .venv in the `acme-lib-c` directory.

To use a package, cd into the project root directory, activate the virtual environment, and import the library.

```
cd acme-lib-a
source .venv/bin/activate
python -c "import acme.lib_a"
```
